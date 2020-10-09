---
layout: mypost
title: neutron命令行创建ELB
categories: OpenStack
---

创建的LB如果不指定tenant_id console上面是看不到的，以下命令大部分都能指定租户ID：`--tenant_id 9fbf9f44b8e44cd19933ac1fd9b30f49`，这个ID是console上面的**项目ID**，两种查看方法：
1. console -> 个人设置 -> 项目ID -> ID 
2. cps控制节点：neutron lbaas-loadbalancer-show **LB_name** 

### 1. 创建VPC和子网

1. neutron net-create net-liu
2. neutron subnet-create --name subnet-liu net-liu 10.0.0.0/24
neutron subnet-create --ip-version 6 net-zhong fd00::/112 --name subnet-ipv6

3. export token=`openstack token issue | grep id | grep -v project_id | grep -v user_id | awk '{print $4}'`
curl -k -X POST https://network.az1.dc1.domainname.com/v2.0/routers -H "User-Agent: python-neutronclient" -H "Content-Type: application/json" -H "Accept: application/json" -H "X-Auth-Token: $token" -d '{"router":{"name":"router_name","external_gateway_info":{"network_id":"294ebbf9-29d9-4248-8bc2-15efad6c172b","enable_snat":false},"distributed":true,"admin_state_up":true}}'
其中network_id 一般填写 **dummy_external_network** 的ID
4. neutron router-interface-add router_name subnet_name

### 2. 创建虚拟机镜像和规格

1. glance image-create --name="euler" --visibility public --disk-format=qcow2 --container-format=bare --file /opt/HUAWEI/image/kvm_euler_basetemplate-1.25.5.qcow2
2. nova flavor-create ovs 21 2048 20 2
nova flavor-key 22 set hw:mem_page_size=1048576

3. nova boot --flavor ovs --nic net-id=**net_id** --image euler vm_name
4. nova boot --image euler --flavor ovs --nic port-id=6a3dc5b3-1843-47da-ba35-9720bf6a589a ljn02


### 3. 放通安全组
默认安全组已放通出方向流量，只用加个入方向。安全组id用 neutron port-show <vm_port> 查看
![](http://3ms.huawei.com/hi/index.php?app=home&mod=Attach&act=showTempImage&filename=20200813032142145001m378.png)

1. neutron security-group-rule-create --direction ingress --remote-ip-prefix 0.0.0.0/0 **net_id**
2. neutron security-group-rule-create --direction ingress --remote-ip-prefix ::/0 --ethertype ipv6 **net_id**


### 4. 创建ELB
依次为创建负载均衡、监听器、后端服务器组、添加后端member、创建健康检查
1. neutron lbaas-loadbalancer-create --name lbname subnetname

2. neutron lbaas-listener-create --name lisname --loadbalancer lbname --protocol UDP --protocol-port 400
*protocol 可选 HTTP HTTPS TCP UDP*

3. neutron lbaas-pool-create --listener lisname --name pool_name --lb-algorithm SOURCE_IP --protocol UDP
    1. lb-algorithm 分配策略。可选 ROUND_ROBIN、LEAST_CONNECTIONS、SOURCE_IP
    2. protocol 一般同listener
    3. session_persistence ：会话保持策略，默认为空。可选 
        * tcp: SOURCE_IP
        * http：HTTP_COOKIE/APP_COOKIE

4. neutron lbaas-member-create --name mem_name --subnet subnetname --address 10.0.0.100 --protocol-port 8000  --weight 1 pool_name
    1. address 后端服务器地址
    2. weight 权重
5. neutron lbaas-healthmonitor-create --delay 30 --max-retries 3 --timeout 3 --type TCP --pool pool_name --name health_name
    1. 创建healthmonitor要指定pool name
    2. delay 健康检查周期
    3. type可选：TCP HTTP UDP PING （新加HTTPS）
    4. http_method 可选 'GET', 'HEAD', 'POST', 'PUT', 'DELETE', 'TRACE', 'OPTIONS', 'CONNECT', 'PATCH'





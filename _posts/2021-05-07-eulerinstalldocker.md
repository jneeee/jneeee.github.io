---
layout: post
title: euler安装使用docker
categories: [代码]
tags: docker
---

virtualbox配置两个网卡：一个NAT，一个主机网络
主机网络用来宿主机（windows）访问虚机，NAT用来访问外网。

## 1 配置代理
```
#系统代理
export http_proxy=http://域账号:密码@proxy.huawei.com:8080
export no_proxy='*.huawei.com,127.0.0.1,192.168.*'
#Git 代理
git config --global http.proxy http://域账号:密码@proxy.huawei.com:8080
```

## 2 配置yum镜像
yum直接用huawei镜像，不用配代理
```shell
cat /etc/yum.repos.d/EulerOS.repo 
[base]
name=EulerOS-2.0SP5 base
baseurl=http://hexie/euler/2.5/os/x86_64/
enabled=1
gpgcheck=1
gpgkey=http://hexie/euler/2.5/os/RPM-GPG-KEY-EulerOS

yum clean all
yum makecache
yum install -y make gcc openssl-devel numactl-devel libpcap popt-devel kernel-devel-`uname -r`  golang git
yum install -y docker-engine
```

## 3 配置docker源
经测试内网的镜像（hexie）不能用，设置一个友商镜像，默认使用系统代理。参考 https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
## 4 docker命令
https://xiaoxiami.gitbook.io/docker/docker-de-shi-yong/docker-ke-hu-duan-chang-yong-ming-ling/docker-cao-zuo-zhi-ling
```
docker search nginx
docker pull nginx
docker run -d -P [--name containner_name] image_name
# -P 把每个容器端口都映射，实用
docker ps 
docker exec -it $container_id bash
# 修改docker内的文件（拷贝出来修改后再拷贝回去，里面没vim的情况）
docker cp -p $container_id:/path ./
```

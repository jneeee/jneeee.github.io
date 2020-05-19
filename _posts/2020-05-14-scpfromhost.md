---
layout: mypost
title: 从host向虚机拷贝文件
categories: OpenStack
---
> 遇到没有eip的虚机或者是“假的”eip虚机，需要拷贝tcpdump过去的时候，可以尝试从host的名空间向其拷贝，方法和名空间登录虚机类似。

1. 首先找到虚机的id `nova list --all-t|grep [vm name]`
2. 找到虚机的网络id `nova interfaces-list [vm id]`
3. 找到虚机的host：
  * `nova show [vm id] |grep host` 得到host id
  * `cps host-list |grep [host id]`
4. 去到host 节点，`ip netns |grep [net id]` 得到qdhcp-[netid]
5. 进入该名空间：`ip netns exec qdhcp-[netid] bash`
6. 使用登录或者拷贝命令，记得加上`-o "StrictHostKeyChecking no"` ，如：
  * scp -o "StrictHostKeyChecking no"  /root/file root@ip:/root
  > scp A B 从A向B拷贝，加 -r 拷贝文件夹
  
  * ssh -o "StrictHostKeyChecking no" root@ip


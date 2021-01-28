---
layout: mypost
title: TCP 中的 KeepAlive 机制
categories: network
---
## 一、ELB TCP长链接偶现超时的问题
Linux TCP 的保活超时时间默认是7200s，即一个连接需要超过7200s不活跃才开始发送心跳包。而 **ELB的TCP会话超时时间为300s**，这样有可能导致ELB已经释放链接端口而Client和Server没有释放，从而导致业务链接超时。

系统参数查看方法：
```shell
cat /proc/sys/net/ipv4/tcp_keepalive_time
cat /proc/sys/net/ipv4/tcp_keepalive_intvl
cat /proc/sys/net/ipv4/tcp_keepalive_probes
```
type1 8.0 已加入超时发送 Reset 包的机制。`[R.]`对长链接业务的影响？
## 二、解决办法
在两个server上修改tcp_keepalive_time的值<300。
修改 /etc/sysctl.conf 的全局配置：
```conf
net.ipv4.tcp_keepalive_time=200
net.ipv4.tcp_keepalive_intvl=75
net.ipv4.tcp_keepalive_probes=9
```
执行如下命令使其生效：
```shell
sysctl –p
```
使用如下命令查询参数的值：
```shell
sysctl -a | grep keepalive
```

## 三、使用场景
一般我们使用KeepAlive时会修改空闲时长，避免资源浪费，系统内核会为每一个TCP连接 建立一个保护记录，相对于应用层面效率更高。

常见的几种使用场景：

- 检测挂掉的连接（导致连接挂掉的原因很多，如服务停止、网络波动、宕机、应用重启等）
- 防止因为网络不活动而断连（使用NAT代理或者防火墙的时候，经常会出现这种问题）
- TCP层面的心跳检测

KeepAlive通过定时发送探测包来探测连接的对端是否存活， 但通常也会许多在业务层面处理的，他们之间的特点：

- TCP自带的KeepAlive使用简单，发送的数据包相比应用层心跳检测包更小，仅提供检测连接功能
- 应用层心跳包不依赖于传输层协议，无论传输层协议是TCP还是UDP都可以用
- 应用层心跳包可以定制，可以应对更复杂的情况或传输一些额外信息
- KeepAlive仅代表连接保持着，而心跳包往往还代表客户端可正常工作

和Http中Keep-Alive的关系：
- HTTP协议的Keep-Alive意图在于连接复用，同一个连接上串行方式传递请求-响应数据
- TCP的KeepAlive机制意图在于保活、心跳，检测连接错误


<hr>

参考：
https://blog.biezhi.me/2017/08/talk-tcp-keepalive.html




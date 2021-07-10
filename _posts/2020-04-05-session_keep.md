---
published: true
title: ELB会话保持测试方法
layout: post
author: Jn
category: 电脑
tags: 
- elb
---

前提：
* 至少两个后端member
* 会话保持类型 HTTP cookie 会话保持

测试分一下两种情况，
1. 绑了EIP的负载均衡器，直接本地浏览器不停刷新就好。观察回显是否变化。
2. 没绑定EIP的负载均衡需要用curl命令测试:

```bash
# 请求到cookie
[root@host-10-0-0-35 ~]# curl http://10.0.0.250 -i 
HTTP/1.1 200 test server
Date: Mon, 15 Jun 2020 08:03:18 GMT
Content-Type: text/html
Transfer-Encoding: chunked
Connection: keep-alive
Server: elb
Set-Cookie: 5e7a231c-0fe6-47cf-b715-fbf0336ea3a1=WyIxMzUwNzc2NDIxIl0;\
 Expires=Mon, 15-Jun-20 08:04:18 GMT; \
Domain=10.0.0.250; Path=/; HttpOnly

<h1>port:26000</h1>
# 携带cookie请求
[root@host-10-0-0-35 ~]# curl http://10.0.0.250 -b "5e7a231c-0fe6-47cf-b715-fbf0336ea3a1=WyIxMzUwNzc2NDIxIl0"
<h1>port:26000</h1>
```
如上的`Set-Cookie`字段是elb发起的，和后端member没关系。**第一次访问**时候使用`curl http://10.0.0.250 -i `得到Set-Cookie:后面的字段，接下来使用`curl http://10.0.0.250 -i -b "xx=xx"`带着这个cookie去请求，按理说后端回显就不会变化了。

参考链接：[阮一峰curl命令详解](http://www.ruanyifeng.com/blog/2019/09/curl-reference.html)

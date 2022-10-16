---
published: true
title: Python requests学习
layout: post
author: Jn
category: 电脑
tags: 
- Python
---

用`requests`可以做一个功能完整强大的爬虫。用webide平台可以帮我直接测试它，非常方便   
重要学习参考 [Requests 的一些高级特性](http://docs.python-requests.org/zh_CN/latest/user/advanced.html#advanced)  
ubantu 14.0安装requests的步骤
```bash
sudo apt-get install python-pip
pip install requests
```
简单的动手
```python
>>> url = 'http://baidu.com'
>>> s.get(url)
<Response [200]>
>>> s.headers
{'Connection': 'keep-alive', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.18.4'}
>>> s.cookies
<RequestsCookieJar[]>
```
调取一个GitHub api 。
```python
# -*- coding: utf-8 -*-
"""
spider/url.py
"""
import requests

url = 'https://api.github.com/repos/requests/requests/git/commits/a050faf084662f3a352dd1a941f2c7c9f886d4ad'

r = requests.get(url)
if (r.status_code == requests.codes.ok):
    print 'headers:',r.headers['content-type']
commit_data = r.json()

print "heys:",commit_data.keys()
```
## 关于douban发广播爬虫
以前写过一个自己登陆douban再发广播的，没成功。因为验证码的问题。这次直接使用cookies，一次搞定！code：
```python
# -*- coding: utf-8 -*-
"""
requests学习实战
"""
import requests
url = 'https://www.douban.com/'

headers = {
'Accept':'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
'Connection':'keep-alive',
'Host':'www.douban.com',
'User-Agent':'Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36',
}
cookies = {
}
data = {
'ck':'xxxx',
'comment':'succese!成功！'
}
#使用with 可以很好的结束会话
with requests.Session() as s:
    r = s.post(url,headers=headers,cookies=cookies,data=data)
    print r.status_code #200为成功
    
print 'over!'
```
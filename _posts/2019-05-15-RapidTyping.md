---
layout: post
title:  "RapidTyping"
date:   2019-05-15
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Blue-whale]
tags: [python,web]
icon: icon-html
---

[题目链接: Rapid Typing](http://123.207.149.64:23331/captcha/)

F12查看网页源码，发现网页的图片路径是一串由$Base64$加密的字符串，然后在某位大佬的提示下，$python$恐惧症患者开始了垂死挣扎

> 获取图片路径

由于图片的路径是$Base64$加密的，在解密之前应该先提取密文字符串

``` python
import base64
import requests
import re
url = 'http://web1.blue-whale.me:23331/captcha/'
s  = requests.Session()
r = s.get(url)
str = r.text
#print str
str1 = re.findall(r"base64,(.+?)\" /></p>", str)[0]
str2 = base64.b64decode(str1)
```

然后，将所有字符进行排序

``` python
x = re.findall(r"x=\"(.+?)\"", str2)
y = re.findall(r";\">(.+?)<", str2)
Z = zip(x, y)
Z.sort(key = (lambda x:int(x[0])))
#print x
#print y
x_new, y_new = zip(*Z)
#print y_new
payload = ''.join(y_new)
```

尝试一发提交以后发现...输入的验证码直接在网址的最后`?code=114514`

然后听说白给？

```python
#print payload
url1= 'http://web1.blue-whale.me:23331/captcha/?code=' + payload
r = s.get(url1)
str = r.text
print str
```

从打印出的网站源码中获取flag即可
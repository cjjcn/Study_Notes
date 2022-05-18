---
title: buu-re-相册-Android逆向初步
tags:
  - CTF
  - RE
  - buu
  - Android逆向
abbrlink: '47225507'
date: 2021-03-30 15:58:48
---

题目中提示是拿到邮箱即为flag，又提示整个apk有病毒，那就只能从静态分析的角度看了。将xiangce1.apk拖入jadx后按照hint在全文中搜索mail关键词，找到了一个函数`sendMailByJavaMail`

![img](https://i.loli.net/2021/03/30/uXv3yq6zOFWINHM.png)

进入后详细查看

![img](https://i.loli.net/2021/03/30/z35SpItZTkmKiW9.png)

这应该就是所要找的发送邮件的函数了，接着反找调用过它的地方

![img](https://i.loli.net/2021/03/30/1dgvOn6B8FCzl9M.png)

![img](https://i.loli.net/2021/03/30/acdEgmGeyA2HZMN.png)

到这一步已经看到发送邮件的变量了，回溯变量看到它的值是从本地文件内拿出来以后进行base64解密后即可得到答案，于是把`libcore.so`拖入IDA打开，并在字符串中找到相应的base64加密的字符串

![img](https://i.loli.net/2021/03/30/YXxNsnIDZT7FqAj.png)

总共有三个字符串类似base64加密，一一尝试后发现第二个是邮箱地址`18218465125@163.com`，本题得解


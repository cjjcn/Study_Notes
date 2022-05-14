---
title: buuctf-misc-10/09
tags: CTF
abbrlink: d61633e9
date: 2020-10-10 11:50:22
---
# 1.金三胖
典型入门题目，将GIF图片导入StegSolve，Analyse->Frame Browser，逐帧查看，将闪过的几张falg内容拼接起来即可
<!--more-->
# 2.二维码
到手先扫描，告诉我This is a Secret（？？？），然后下意识跟着上一题的思路想到图层扫描，经过几次仔细查看后我确定这个图片并没有什么隐藏的图层。。。然后想起来用老套路，放入Binwalk结果发现了夹在里面的压缩文件，爆破得到压缩文件密码即得Flag

# 3.N种方法解决
解压看到一个exe，放入winhex发现一个压缩文件，打开是图片，详细信息里面有base64特征的一串，解密后即得flag

# 4.大白
放入winhex发现图片高度被修改过，将高度修改回来即得flag

# 5.你竟然赶我走
winhex打开查看编码，在最后可发现flag的ASCII码

# 6.基础破解
梅开二度，爆破密码，得到base64，解密即为FLAG

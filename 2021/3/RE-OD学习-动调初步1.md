---
title: RE-OD学习-动调初步1
tags:
  - CTF
  - RE
abbrlink: bb8a7f89
date: 2021-03-15 18:28:21
---

>软件来源：吾爱破解CrackMe

## OD动态调试无壳

第一个程序拿到后，打开发现是一个简单的类似于登录界面的窗口，填入几组账号密码组合后均提示失败，使用PEID检查发现此程序无壳，于是在OD内打开

![img](https://attach.52pojie.cn/forum/202101/26/215946rlllxnscn0ztsnld.png)

接着使用中文搜索关键字“成功”，找到了对应的字符串，接着回溯回去之后在上方发现了jmp指令，将其nop掉，重新调试程序，发现成功进入

![img](https://attach.52pojie.cn/forum/202101/26/222830w9iak0nk90kyriki.gif)

## 多次调用比较

有两种方法，第一种是直接找到中文搜索恭喜关键字找到之后，判断上方的jz是有条件跳转，nop掉尝试一下是否可以直接绕过，结果是可以，直接过。

第二种是找到关键字后，在输出信息之前的寄存器变量赋值时打断点找寄存器，就可以找到真正的密码所在

![img](https://attach.52pojie.cn/forum/202101/27/122526xldnrsjaw5f3jsqq.jpg)

## 附几种跳转的总结

![img](https://attach.52pojie.cn/forum/202101/26/174834uu0za4wjeirukhsz.png)
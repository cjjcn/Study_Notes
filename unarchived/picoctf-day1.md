---
title: picoCTF-day1
tags: CTF
abbrlink: fa61788
date: 2020-10-11 22:34:26
---
# Lets Warm Up

If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?

这应该就是签到题了吧？爱了爱了，十六进制转十进制再打表（突然ACM）转ASCII，得到‘p’，按照格式输入即可
<!--more-->
# The Numbers

下载图片后得到一串数字，根据格式盲猜开头是picoCTF，然后又是一波打表- -，根据这几个字母和他们的数字就硬解出了这道题，拿到了Flag

# 2Warm

进制转换，啊这，这才是签到题吧- -

# Insp3ct0r

打开网页源代码，找到1/3的Flag，然后观察代码，发现一个css和一个js，可能答案藏在这里，打开之后分别找到第二第三部分，合并即可

# Glory of Garden

下载图片，一看就是老套路，第一页题目盲猜不会太深，直接开stegsolve看图片信息，果然找到了flag

# vault-door-training

题目太长不看，下载java文件打开源码即得flag

# Warmed UP

梅开二度，仿照前面进制转换

# vault-door-1
本题是前面vault变种，打开源码看应该是一个登录的密码核对的处理机制。
```java
import java.util.*;

class VaultDoor1 {
    public static void main(String args[]) {
        VaultDoor1 vaultDoor = new VaultDoor1();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
	String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
	}
    }

    // I came up with a more secure way to check the password without putting
    // the password itself in the source code. I think this is going to be
    // UNHACKABLE!! I hope Dr. Evil agrees...
    //
    // -Minion #8728
    public boolean checkPassword(String password) {
        return password.length() == 32 &&
               password.charAt(0)  == 'd' &&
               password.charAt(29) == '3' &&
               password.charAt(4)  == 'r' &&
               password.charAt(2)  == '5' &&
               password.charAt(23) == 'r' &&
               password.charAt(3)  == 'c' &&
               password.charAt(17) == '4' &&
               password.charAt(1)  == '3' &&
               password.charAt(7)  == 'b' &&
               password.charAt(10) == '_' &&
               password.charAt(5)  == '4' &&
               password.charAt(9)  == '3' &&
               password.charAt(11) == 't' &&
               password.charAt(15) == 'c' &&
               password.charAt(8)  == 'l' &&
               password.charAt(12) == 'H' &&
               password.charAt(20) == 'c' &&
               password.charAt(14) == '_' &&
               password.charAt(6)  == 'm' &&
               password.charAt(24) == '5' &&
               password.charAt(18) == 'r' &&
               password.charAt(13) == '3' &&
               password.charAt(19) == '4' &&
               password.charAt(21) == 'T' &&
               password.charAt(16) == 'H' &&
               password.charAt(27) == 'f' &&
               password.charAt(30) == 'b' &&
               password.charAt(25) == '_' &&
               password.charAt(22) == '3' &&
               password.charAt(28) == '6' &&
               password.charAt(26) == 'f' &&
               password.charAt(31) == '0';
    }
}
```

但是想要破解密码需要一点点技巧（一个一个手动核对实在是太麻烦了- -），感谢lrj学长的精心指导，让我想起了vs code可以对同样的字符片段进行批量操作，把前面的字符全部删除之后，利用python，把==变成：，然后以数字作为key进行sort排序，然后批量提取即可得到flag，这个记录可圈可点

# what's a net cat?

没啥说的，打开Kali，打开终端
```
nc jupiter.challenges.picoctf.org 54271
```
即可得到flag

# Strings it

这个下载下来，hint说的是不用打开，那就是hexo了，打开根据编码搜索‘ctf’无果，更换大小写‘CTF’查找几次之后即找到了flag

# Easy1

这不就是OI的打表题吗，爱了爱了，根据给的table把他进行解码就可以得到flag

# where are the robots

打开链接以后，根据题目，应该指的是robot.txt，那就访问robot，又得到一个html，即可得到flag

# 总结
dalao们都说picoCTF的题循序渐进，适合入门，看来确实是这样，今后就在这儿开坑了

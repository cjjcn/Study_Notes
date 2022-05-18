---
title: CTF学习记录-Misc-图片隐写
tags: CTF
abbrlink: 9ab7588f
date: 2020-11-20 16:14:05
---

# 直接附加

## 常用文件头/文件尾

| 文件名    | 文件头         | 文件尾      |
| --------- | -------------- | ----------- |
| JPEG(jpg) | FF D8 FF       | FF D9       |
| PNG(png)  | 89 50 4E 47    | AE 42 60 82 |
| GIF(gif)  | 47 49 46 38    | 00 3B       |
| zip       | 50 4B 03 04    | 50 4B       |
| RAR       | 52 61 72 21    |             |
| WAV       | 57 41 56 45    |             |
| avi       | 41 56 49 20    |             |
| MPEG(mpg) | 00 00 01 BA/B3 |             |
| mov       | 6D 6F 6F 76    |             |



# EXIF隐写

## exiftool工具常用命令

```
//对于图片Lighthouse.jpg
//为图片添加一个comment标签，内容为ExifModifyTesting
exiftool -comment=ExifModifyTesting ./Lighthouse.jpg
//查看EXIF信息
exiftool ./Lighthouse.jpg
```



# LSB

LSB全称least significant bit，最低有效位

PNG文件中的图像像数一般是由RGB三原色（红绿蓝）组成，每一种颜色占用8位，取值范围为0x00~0xFF，即有256种颜色，一共包含了256的3次方的颜色，即16777216 种颜色

人类的眼睛可以区分约1000万种不同的颜色

这意味着人类的眼睛无法区分余下的颜色大约有6777216种

LSB隐写就是修改RGB颜色分量的最低二进制位（LSB），而人类的眼睛不会注意到这前后的变化

每个像数可以携带3比特的信息

## 检测隐写常用工具 - stegsolve - zsteg



# 盲水印

## 添加与提取

[BlindWaterMark](https://github.com/chishaxie/BlindWaterMark) 频谱隐写可直接使用Adobe Audition




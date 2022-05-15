---
title: CTF-NEFU校赛-题解
tags:
  - CTF
  - NEFU
abbrlink: 211cd2ab
date: 2020-12-21 15:31:44
---

>Write by NEFUNSI：
>
>ghosin
>
>0ERROR

# 签到

***

## signin

下载 `signin.txt` 打开得到一串 `base64`，解码得到 `flag{we1come_t0_NEFUCTF!}` 。

***

## Re_SignUp

下载 `SignUp` ，使用 `010 Editor` 打开发现是 `Linux` 可执行文件。用 `ida64` 打开可看到 `main` 函数处有字符串 `xr yeI^r}.s{A)qAL/h{lM{AI.(rz?c` ，根据 `Hint` 中的 `xor` 使用[在线工具爆破](https://gchq.github.io/CyberChef/#recipe=XOR_Brute_Force(1,100,0,'Standard',false,true,false,'')&input=eHIgeWVJXnJ9LnN7QSlxQUwvaHtsTXtBSS4ocno/Yw)可得到 `fl>g{W@lc0me_7o_R1verSe_W06ld!}` 。

# MISC

## untitle

下载压缩包，发现加密的 `flag.txt` 和没加密的 `untitle.wav` 。解压 `untitle.wav` ，发现是倒放。使用 `Audacity` 逆转后听歌识曲得到 [`Untitled`](https://music.163.com/#/song?id=1499834227) 。  
以 `Untitled` 作为密码，使用 `WinRAR` 解压 `flag.txt` 发现报错和 fakeflag 。

> ! G:\labs\nefuctf\misc\111flag111 (2).rar: 无法设置 C:\Users\ghosi\AppData\Local\Temp\Rar$DRb13916.37568\flag.txt 的安全数据
> 这个安全 ID 不能分配为此对象的所有者。

怀疑是 [`NTFS交换数据流隐写`](https://joner11234.github.io/article/85357d8d.html) ，使用 `NtfsStreamsEditor2` 扫描得到 `flag{M4ke_m1sc_gr3at_ag@in}` 。

***

## 蛇图攻击

下载得到一个 zip 压缩包，解压得到 `看到了吗flag就在那.zip` 和 `蛇图.jpg`，在 `看到了吗flag就在那.zip` 中看到要找的 `flag.txt` 和已有明文的 `蛇图.jpg` ，使用 `ZIP已知明文攻击` 即可得到 `flag.txt` ，内容为 `flag{congratulation~~}` 。这里注意在压缩 `蛇图.jpg` 的时候要使用 **`Bandizip`** ，使用其他压缩软件可能会使 `AZPR` 类软件出现类似 `No matching files in selected archives` 的错误。

***

## 别人家的孩子

下载附件打开听一下，发现左右声道不同。拖进 `Audacity` ，去掉左声道的歌曲，右声道前半部分为莫斯电码，记录下内容为 `..-./.-../.-/--./-/-----/-.../...--` ，可解出内容为 `flagt0b3` ，后半部分为 [`dtmf`](https://baike.baidu.com/item/DTMF)，使用 `dtmf2num` 读取到 `121111117114115101108102` ，可解出 `yourself`。至此拼接得到 `flag{t0b3yourself}` 。

## 谁还没受过伤呢

根据题意，心愿小姐失恋了，然后附了一个QQ号：2508898249，因此就从QQ号下手找线索。

![img](https://i.loli.net/2020/12/21/5GgDxH2Rf1AjhWk.png)

打开资料页可以看出信息量不大，主要就是有生日，其他信息不出意外应该都在空间里面

打开空间，首先看到几条说说，其中包含一条：

![img](https://i.loli.net/2020/12/21/exkSGU8NCw1Lq5j.png)

百度网盘打开是一个IDOL的文件夹，下载以后是9张许嵩的照片，这里注意了，第9张照片明显体积偏大，而照片又很糊，说明照片里面有东西![img](https://i.loli.net/2020/12/21/w9GREemj5N7APrK.png)

继续翻说说，又发现了一个线索说说

![img](https://i.loli.net/2020/12/21/JtdrfA9ji5FcIHR.png)

不知道有什么用，暂且留在这里不动。

看看其他地方，相册里面有一个有问题的照片，根据问题很明显就是前面心愿小姐所@的人就是问题的答案，填入DK_night后，得到一张图片，扫描图片上的二维码即可得到前一半flag

然后继续看，留言板里有一段话

```
G1v3_me_s0m3_rEd_P4cKe7eS_t0_g3t_7h3_h1nt!
```

不知道有什么用，暂且放着。

空间里能得到的线索就这么多了，再回头看看照片，第九张照片用010Editor打开后发现有隐写的zip格式的压缩包，分离出来以后如图![img](https://i.loli.net/2020/12/21/X5y4RHrWDAefukp.png)

hint已经很明显了，是生日按照格式输入就是密码，但是填入前面的日期却无法解密，仔细观察后发现上面的生日是农历，将其日期转成公历以后即可得到2001.5.14(这里这个日期有点坑，转成2001年的四月廿二好像日期不太对，换思路转为今年的四月廿二才为正确日期，成功解密)，还有一种方法是知道了密码格式之后直接放进cracker里面按照格式爆破即可得到，速度也很快，这里就不作具体说明了。

打开flag.doc以后发现里面是空的，ctrl+a之后发现有字符，文字换颜色之后发现是个假flag，说明线索不在文档里，再看看文档的详细信息![img](https://i.loli.net/2020/12/21/geIH4byZfwMAPWB.png)

可以看出备注和管理者里面各有一堆乱序字符，其中管理者里面的字符有明显的Base64加密标志，解密一次后发现变成了乱码，怀疑是加密后的成分残缺导致的错位，考虑到上面备注内的不明所以的乱码，将其和此段合并后解密(N次套娃)后即可得到后半flag

```
even_1f_i_L0Ve_U}
```

# CRYPTO

***

## RSA1.0

> 前置知识: [https://xz.aliyun.com/t/6459](https://xz.aliyun.com/t/6459)

```python
from Crypto.Util.number import *
import gmpy2

# 由题目可知
c = 45771827077895268121546606393404724664015354394782828337635308358292455516045347632184096192996835878084629358486100781061415523065007935763870549459874855232581330470311918549209690663379064436674445311856972558138654423953641880869235765375826041038321658329327107626577671053285966037170551853374920563440
e = 65537

n = 51644741119502351404435614782384203642721946675648874025147110583432277606213746585645021132709993574641883435114709470653847057718236543709859136714778649404725233027975626929711061148770545740304700844663373207681750776232949001954620241734085537399155514559867656395449958909477471179617512586129540850043
# yafu 分解 n 可得 p 和 q
p = 7186427563087403123764567435071522152765535485843889386595810558353757249884932307781450308876687526723957522099910738089951348400929674280355762675680509
q = 7186427563087403123764567435071522152765535485843889386595810558353757249884932307781450308876687526723957522099910738089951348400929674280355762675680727

# 计算私钥 d
d = gmpy2.invert(e, (p-1)*(q-1))

# 解密 m
m = pow(c,d,n)

print(long_to_bytes(m))
# b'flag{now_y0u_know_rsa}'

```

***

## are you file?

下载 `6.txt` ，发现是 `Ook!` 编码，使用[在线工具](https://www.splitbrain.org/services/ook)解码得到 `呋食食性噗哮食冬註魚嗥笨麼噤覺我嘶人雜嗚有怎囑爾發常哮出沒喜唬有意吖人哞哈類更我呆魚蜂圖呆喜食氏告嘶嗒洞現訴訴盜萌現嘶嘿雜非嗒和噗發笨` ，是熊曰编码，使用[在线工具](http://hi.pcmoe.net/index.html)解码得到 `G2aZlXllv1_uvohfim3}deBb=f{yhenaotGmkbFXgfpfoadelf!`，猜测栅栏密码，使用[在线工具](https://www.qqxiuzi.cn/bianma/zhalanmima.php)，每组字数 `3` 解码得到 `GmG23ma}kZdbleFXBXlbgl=fvfp1{f_youhavedonehalfofit!` 。根据结尾 `_youhavedonehalfofit!` 可知已经解开一半了。去掉这部分，猜测接下来是W型栅栏密码，因为栅栏密码加密后第一位不变，故逆转字符串为 `f{1pfvf=lgblXBXFelbdZk}am32GmG` ，使用[在线工具](http://www.atoolbox.net/Tool.php?Id=777)，栏数为 `3` 可得到 `flag{bml1X3BpX2FfeGlvbmdfZGk=}` 。解码其中的 `base64` 可得到 `flag{niu_pi_a_xiong_di}` 。

# Web

> 详见：[f1oat's blog - 校赛web-wp](

# RE

## Test_your_math

IDA打开后查看字符串，发现flag关键字![img](https://i.loli.net/2020/12/21/xVCm8J2NhjWk659.png)

查找之后反向寻找其在程序中的位置

![img](https://i.loli.net/2020/12/21/TahMxNjCkoU3PSt.png)

继续在主函数回溯其判断输入数据正确性的位置![img](https://i.loli.net/2020/12/21/IWMS8kvAHunGfVl.png)

得到一个判断输入数据正确性的函数，转为源代码形式![img](https://i.loli.net/2020/12/21/wT1nSc89xFdIghm.png)

很明显可以看出是一个解方程，只需使用hint中提到的z3解出答案方程答案即可得到flag

## maze

拿到题目打开后有一个很明显的UPX壳，无脑用UPX_SHELL脱壳后重新打开

可以看出这应该是关键代码的一部分。

这里jnz跳转到了下一行代码，相当于没跳转，![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203161152133.png)而下面的`call near ptr 0EC85D78Bh`调用了一个不是地址的地址，可以推断出这段代码添加了花指令，IDA分析失败了。

可以确定这个jnz指令是花指令，还有下面的call指令。
先将jnz指令nop掉。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203163906627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1BhbG1lcjk=,size_16,color_FFFFFF,t_70)

之后就是这个call指令，不能全部nop，因为后面那个东西可能是有效代码。摁d将其先转换为字节数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203164142113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1BhbG1lcjk=,size_16,color_FFFFFF,t_70)

经过试验（先将第一个数据nop掉再转成代码，不行再将前俩数据nop……）发现 `db 0E8h`这条指令是添加的花指令，将其nop掉，nop掉之后IDA自动的将后面的数据转换为代码数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020316432998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1BhbG1lcjk=,size_16,color_FFFFFF,t_70)

此时数据地址是红色的，将关键代码全部选中，摁p键将其声明为函数
然后就可以F5伪代码了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203165617297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1BhbG1lcjk=,size_16,color_FFFFFF,t_70)

根据题目maze可知这是一个迷宫题。
控制的有两个数据，一个是`dword_408078`，另一个是`dword_40807C`
双击进去查看初始数据。
初始状态：

```
dword_408078=7
dword_40807C=0
```

然后经过14次移动需要使

```
dword_408078=5
dword_40807C=-4
```


然后在内存中找到迷宫![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203165444324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1BhbG1lcjk=,size_16,color_FFFFFF,t_70)

提取出来排列之后![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203165745706.png)

用wasd控制行走，dword_408078控制左右，dword_40807C控制上下。
s是上，w是下，a是右，d是左。
行走路径为ssaaasaassdddw
则flag为`flag{ssaaasaassdddw}`


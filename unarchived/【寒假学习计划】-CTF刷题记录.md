---
title: 【寒假学习计划】-CTF刷题记录
tags:
  - CTF
  - 假期学习
abbrlink: 5e6c9a08
date: 2022-02-09 11:05:59
---

# RE

## BUUCTF-re-[GWCTF 2019]re3（SMC自修改代码 | AES加密）

使用IDA64载入,在侧边栏找到main函数查看

![main函数](http://image.0error.net/202202101138143.png)

由第9行代码可得，输入关键字长度为32，第14行`mprotect`将程序修改为可读可写可执行，第15行开始则对程序进行了SMC自修改，给函数异或了0x99。

发现内容疑似还有加密，进行`FindCrypt`分析，得出有以下加密方式

![FindCrypt](http://image.0error.net/202202101138764.png)

进入`sub_402219`查看，此时看到的是一些无意义的数据

![402219未解译前](http://image.0error.net/202202101139908.png)

编写idc脚本对上述内容进行解密

```c
#include <idc.idc>

static main()
{
    auto addr = 0x402219;
    auto i = 0;
    for(i=0;i<224;i++)
    {
        PatchByte(addr+i,Byte(addr+i)^0x99);
    }
}
```

解密后将这部分函数内容进行`Force`分析

![402219解密后](http://image.0error.net/202202101139086.png)

接着将分析内容生成为函数并转换为伪代码

![402219解析为函数后](http://image.0error.net/202202101139864.png)

![402119伪代码](http://image.0error.net/202202101140467.png)

接着分析`40207B`

![40207B](http://image.0error.net/202202101140666.png)

经过分析可知，仅有第10行、14行的加密有实际作用，基本逻辑为将base64密码表进行两次`sub_401CF9`加密后赋给`a1`，通过动态调试可以得到此处的结果

![breakpoint](http://image.0error.net/202202101140531.png)

![breakreasult](http://image.0error.net/202202101141643.png)

最终结果即`unk_603170`为

```
CB8D493521B47A4CC1AE7E62229266
```

接着继续分析`sub_402219`

![402119伪代码](http://image.0error.net/202202101141043.png)

可得加密后的密文为

```
BC0AADC0147C5ECCE0B140BC9C51D52B46B2B9434DE5324BAD7FB4B39CDB4B5B
```

再分析`sub_40196E`

![40196E](http://image.0error.net/202202101141745.png)

可得大致流程如下：

1. 经过MD5后得到AES ECB模式的密钥
2. 将输入分为两部分，分别进行AES128加密
3. 与密文进行比较

因此可写出脚本

```python
from Crypto.Cipher import AES
from Crypto.Util.number import *
key = long_to_bytes(0xcb8d493521b47a4cc1ae7e62229266ce)
mi = long_to_bytes(0xbc0aadc0147c5ecce0b140bc9c51d52b46b2b9434de5324bad7fb4b39cdb4b5b)
lun = AES.new(key, mode=AES.MODE_ECB)
flag = lun.decrypt(mi)
print(flag)
```

运行结果：

![flag](http://image.0error.net/202202101142292.png)


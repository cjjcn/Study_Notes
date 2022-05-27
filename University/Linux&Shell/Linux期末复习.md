#Linux #NEFU 
# 紧急加入知识点
## Linux中各类特殊变量的含义
`$#` 是传给脚本的参数个数
`$0` 是脚本本身的名字
`$1` 是传递给该shell脚本的第一个参数
`$2` 是传递给该shell脚本的第二个参数
`$@` 是传给脚本的所有参数的列表
`$*` 是以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个
`$$` 是脚本运行的当前进程ID号
`$?` 是显示最后命令的退出状态，0表示没有错误，其他表示有错误
# 文件管理与常用命令
## Linux文件和目录结构

- / 根目录
- /etc 供系统维护管理用的命令和配置文件
- /tmp 存放临时文件
- /boot 存放系统引导文件
- /bin 系统常用命令，如ls, ln, cp, cat等
- /home 用户目录
- /root 超级管理员目录
- /dev 存放设备文件
- /usr
  - /usr/include C语言头文件存放目录
  - /usr/tmp 存放临时文件
  - /usr/bin 存放常用命令，如echo, grep, kill
  - /lib,/usr/lib 存放各种库文件，指C语言的链接库文件，以及terminfo终端库等等

## 文件通配符和文件类型

- `*` 可执行普通文件
- `/` 目录
- `@` 符号链接
- `|` FIFOs
- `=` 套接字
- `-` 普通文件
- `p` 目录文件
- `b/c` 设备文件
- `l` 链接文件
- `p` 管道文件

## Command
### sort排序
sort将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将他们按升序输出。
- `-u` 在输出行中去除重复项
- `-r`sort默认排序方式为升序，加此选项改为降序
- `-o`重定向输出文件
- `-n`以数字形式排序，默认为字符串形式
- `-t`设定排序索引的间隔符
- `-k`指定排序的列数
- `-f`忽略大小写
- `-b`忽略每一行前面所有的空白部分

### cat

- -n 编号
- -b 空白行不编号
- -s 压缩连续空行

### ls

- -a 全部信息
- -l 长信息
- -F 附加文件类型

### more

- -d 提示
- -f 计算实际显示行数
- -s 合并显示空行
- +/pattern 搜索字串并在此之后开始显示
- -num 每行显示的行数
- +num 从num行开始显示

------------

- space/n 下一屏
- b 上一屏
- enter 下一行
- /pattern 搜索

### less

- -a 最后一页
- -m 百分比
- -M 百分比 行号 总行数
- -N 行号
- -p test 搜索`test`
- -s 合并显示空行

### head

- -num

### tail

- -num

### cp

- -a = cp -drp
- -d 保留文件链接
- -f 强制执行
- -i 交互提醒
- -p 更新修改日期与访问权限
- -r 递归复制文件树
- -v 可视化操作
- -l 不复制而只产生硬链接
- -s 不复制而只产生软链接

### mv

- mv -i 交互提醒
- -f 强制执行

### rm

- -r 递归删除文件夹
- -f 强制执行
- -i 交互提醒

### diff

- -b 忽略空格
- -c 上下文格式显示
- -C `num` 显示`num`行上下文

-------

- `<`为删除
- `>`为新增

### touch

### mkdir

- -p 帮助确保目录全部存在，如果不存在就新建

### rmdir

### tar

- -c 打包
- -x 解包
- -v 可视化
- -f 不交互
- -z 对文件进行压缩
- -czvf 压缩
- -tzvf 列出压缩内容
- -xzvf 解压

### ln

- ln 硬链接
- -s 软链接

# VIM
![vim](https://image.0error.net/img/20221317599-20220429201853323-432917606.png)
![vim_KeyBind](https://image.0error.net/img/20221317599-20220429203916105-1231791213.jpg)

# Linux C
## CMake
makefile示例
```makefile
build: clean main

mymath.o: include/mymath.h src/mymath.c
        gcc -fPIC src/mymath.c -I./include -c -o src/mymath.o

welcome.o: include/welcome.h src/welcome.c
        gcc -fPIC src/welcome.c -I./include -c -o src/welcome.o

libtest.so: mymath.o welcome.o lib
        gcc -shared -o lib/libtest.so src/*.o

lib:
        mkdir -p ./lib

main: libtest.so main.c
        gcc main.c -o main -I./include lib/libtest.so

clean:
        rm -f src/*.o
        rm -rf lib
        rm -f main

.PHONY: clean build
```
# GCC
### 编译顺序
```shell
project
main.c
	-src
	-include
	-lib
```
指定的指令
test.c 
- `-E`(预编译) test.i 
- `-S`(生成汇编) test.s 
- `-c`(生成机器码) test.o 
- `-o`(生成目标文件，链接) test
![编译流程](https://image.0error.net/img/2022/20220526163349.png)
### 静态库
```shell
gcc test.c -c test.o
ar rcs testLib test.o
gcc -Llib -testLib
```
### 动态库
```shell
gcc -fPIC test.c -c
gcc -shared -o testLib.so test.o
gcc -Llib -testLib
```
## Account
### uid&gid
表示用户与组群
文件有owner uid&gid
每个uid有与其对应的gid
显示文件系统时需要通过访问/etc/passwd以及/etc/group将uid/gid转化为实际的用户名与组群名
### /etc/passwd
username : pwd(x见shadow) : uid : gid : desc : home_path : shell_path  
uid为0是系统管理员即root  
uid为1-499是系统账号，系统服务使用  
uid为500-65535即为可登录用户账号
### /etc/shadow
username : pwd(加盐后的) : 最近更改密码的日期(linux时间戳) : 密码不可被更改的天数(0即为可随意更改) : 密码需要重新变更的天数 : 密码需要重新变更的提示天数 : 密码过期后的宽限天数 : 账号失效日期 : 保留字段
### /etc/group
groupname : pwd(x见gshadow) : gid : supported_accounts
### Manage Account
#### useradd
useradd {username}
- `-c` 描述信息
- `-s` shell path
- `-d` home path
- `-p` password
#### passwd
#### userdel
userdel {username}
- `-f` 强制
- `-r` 递归删除所有信息
#### su
su {username}
- '-l' 读取/profile的shell环境变量
- exit返回主用户
#### sudo
sudo {exec}默认为root
- `-u` 指定用户执行
#### /etc/sudoers
username ALL=(ALL) ALL  
在任何地方均可运行sudo
#### who
#### last
#### lastlog
## Shell
### Bash
#### type
type {name}
- `-t` 根据name的类型返回一个单词（别名，关键字，函数，内建，文件），否则返回空值。
- `-p` 如果外部命令则显示完整的文件名
- `-a` 根据PATH变量将所有含有name的命令进行罗列，包括别名
#### 变量
echo ${var}
- var=x (**=前后均无空格，变量开头不能为数字**)
- var="var is ${VAR}"变量可以递归调用
- 双引号内特殊字符可以保持原有特性，单引号内特殊字符除空格以外会全部**原样输出**
- 如果要将var的值或一个命令的运行结果赋予另一个变量需要var1=$(var2)
- export VAR可以使可以使VAR成为环境变量
- `unset` 可以取消环境变量
#### 别名alias
```shell
alias cmd="*"
unalias cmd
```
#### 历史
history {num}
- 直接输入可以得到最近的历史命令
- `-c` 清空
- `-a` 将新增的历史数据写入到一个文件
- `-r` 将文件内的内容读入history
- `-w` 将history记录的内容写入文件
history为异步更新，用户登陆状态只会存在内存中，断开连接后才会落盘写入文件

!可以匹配历史命令

#### Bash环境信息
运行顺序：
当前目录或指定目录->alias->builtin->$PATH搜寻到的命令  
Bash启动是会自动从环境变量文件读入变量，分为全局与私有的  
non-login shell与login shell读取的配置文件不一致  
login shell读取顺序/etc/profile->~/.bash_profile->~/.bashrc  
non-login shell只读取/.bashrc
#### 组合按键
| 命令   | 效果         |
| ------ | ------------ |
| Ctrl+c | 终止当前命令 |
| Ctrl+d | EOF          |
| Ctrl+m | Enter        |
| ctrl+s | 暂停输出     |
| ctrl+q | 恢复输出     |
| ctrl+u | 删除整行命令 |
| ctrl+z | 暂停挂起命令 | 
#### 通配符与特殊符号
| 符号  | 意义                                                         |
| ----- | ------------------------------------------------------------ |
| *     | 代表任意长度的字符                                           |
| ?     | 代表一个任意字符                                             |
| [x]   | 代表一个括号内的所有字符（必须有，非任意）                   |
| [a-z] | 代表有一个按括号内编码顺序的范围的一个字符（必须有，非任意） |
| [\^x] | 代表一个非括号内的字符（字符串过滤）                                                             |
#### 数据流的重定向
数据重定向可以将stdin,stdout,stderr传送到不同的设备或位置  
exec 1>stdin 2>stderr(>是覆盖，>>是追加)  
2>/dev/null可以清除错误信息  
2>&1可以将正确与错误信息写入同一个文件
#### 命令运行的判断依据
cmd1;cmd2连续命令下达，但是不考虑命令连续性即命令与命令之间可能乱序
$?获取前一个命令的返回值 $?=0时即命令运行正确  
cmd1 && cmd2 cmd2在cmd1运行完毕且正确时才运行  
cmd1 || cmd2 cmd2在cmd1运行完毕但错误时才运行
### Tunnel
管道命令界定符号|  
|仅能处理前一个命令的正确输出结果，无法直接输出错误结果
#### 颉取命令
颉取命令通常是以行为单位  
cut -d 分隔符 -f 第几段  
grep -v反向选择  
grep -n输出行号  
grep -i忽略大小写  
常用如  
cmd | grep -v pattren1 | grep pattern2
#### 排序命令
sort -t 分隔符 -k 第几个  
uniq -c 去重计数  
wc -l 行  
wc -w 单词  
wc -c 字符
#### 双向重定向命令
tee -a 追加  
重定向一个副本至file中
#### 字符转换命令
tr -d 删除一个字符串  
tr -s 取代重复字符  
tr ‘[a-z]’’[A-Z]’ 实现大小写转换  
tr -d '*'删除内容

col 过滤控制字符  
col -x 将tab换成空格  
col -b 过滤所有控制符

join -t 指定字段分隔符  
join num 指定分析字段  
join -1/2 指定分析文件  
最好事先sort

paste -d 指定分隔符，默认是tab  
paste - 意为来自管道

expand -t num 指定一个tab替代为多少个空格
#### 分割命令
split file PREFIX将文件按大小或行数进行分割  
split -b 指定分割文件的最大大小  
split -l 按行数分割  
PREFIX为分割后的文件名的前导符
#### 参数替换命令
xargs cmd  
xargs -p 提示  
xargs -n 一组命令的量  
xargs -e’\*’ 停止命令的标识  
能够将一个不支持管道的命令，接受来自其他命令的stdout
#### -号
stdin与stdout可以通过-来替代
# 正则表达式
## 特殊字符
egrep "" filename
| 符号    | 说明                                         |
| ------- | -------------------------------------------- |
| ^       | \^word word在行首                            |
| $       | word$ word在行尾                             |
| .       | 一定有一个任意字符                           |
| \       | 将特殊字符转义为一般符号                     |
| *       | 重复前一个字符，0到无穷多个.\*即为任意字符串 |
| [\^]    | 舍去或欲求的字符集合                         |
| [n1-n2] | 欲求的字符范围                               |
| {n,m}   | 连续n到m个的前一个字符                       |
| +       | 一个或一个以上                               |
| \?      | 0个或1个                                     |
| \|      | 或                                           |
| ()      | 组群                                             |
# 文件格式化与处理
sed -n 安静模式  
sed -i 直接修改文件内容  
sed -f 写入文件  
动作:[n1],[n2]funnction n1,n2为动作施加行  
function:a之下追加,c取代,d删除,i之上插入,p打印选择的数据,s替换 如sed ‘2,5d’  
sed s/‘pt1’/‘pt2’/g 全局替换

awk -F 指定分隔符 
```shell
awk -F ’ ’ ‘$3<10{print $1 “\t” $3}’
```
diff -Naur oldfile newfile > patch  
patch -p0 < patch(oldfile->newfile)  
patch -R -p0 <patch(oldfile->newfile->oldfile)
# Shell编程
## 特征
纯文本，从上往下，从左到右  
\可以延伸至下一行  
/#为注释  
/#!/bin/bash  
$((运算内容))
## test
test -e dictory
## $特例
\$\# 参数个数  
$@ 输入参数  
$n 第n个参数
## 循环
```shell
if [ * ]; then
    do sth
elif [ * ]; then
    do sth
else
    do sth
fi

while [ * ]
do
    do sth
done

until [ * ]
do
    do sth
done

for i in $(seq 1 100)
do
    do sth
done

for (( i=0; i<=100; i=i+1 ))
do
    do sth
done
```
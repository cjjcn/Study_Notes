#Linux #NEFU 
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

## VIM
![image](https://img2022.cnblogs.com/blog/1317599/202204/1317599-20220429201853323-432917606.png)
![KeyBind](https://img2022.cnblogs.com/blog/1317599/202204/1317599-20220429203916105-1231791213.jpg)

# CMake
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
## GCC
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

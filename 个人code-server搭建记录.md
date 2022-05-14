---
title: 个人code-server搭建记录
tags:
  - 记录
  - vscode
abbrlink: 51f81c34
date: 2021-10-06 18:24:19
---

# 起因

最近因为越来越多~~极其繁琐~~的专业课，而我的游戏本我实在是背不动了。。。所以想在自己的pad上搭建一个开发环境，一个是解放一下自己的书包，一个是让自己的服务器发挥点作用。。

# 环境

Tencent Cloud 

Ubuntu 20.04 LTS

1H2G+1M

# 搭建

安装docker一类的就不再赘述了,完成上述步骤后，直接安装code-server的docker镜像

```shell
#启动code-server容器
# -p 主机端口:docker端口，-v 主机目录:docker容器中目录
docker run -itd --name my-code -u root -p 80:8080 -v /data/my-code:/home/coder/project -e PASSWORD=123456 codercom/code-server:latest --auth password
#进入容器 -it后面跟着的是容器名称
docker exec -it my-code /bin/bash
#安装coderunner插件后进入容器，通过apt安装各类环境即可使用
apt-get update
apt-get install gcc
apt-get install g++
apt-get install python
```

这里要注意，直接安装环境插件是不适配的，所以需要使用apt去给docker内安装环境

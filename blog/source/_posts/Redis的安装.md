---
title: Redis的安装
tags:
  - Redis
categories:
  - 技术
topic:
  - redis
abbrlink: 590946299
date: 2024-10-19 18:11:04
---
# Redis 安装

## Windows 下安装

**下载地址：**<https://github.com/tporadowski/redis/releases>。

Redis 支持 32 位和 64 位。这个需要根据你系统平台的实际情况选择，这里我们下载 **Redis-x64-xxx.zip**压缩包到 C 盘，解压后，将文件夹重新命名为 **redis**。

![](//www.runoob.com/wp-content/uploads/2014/11/3B8D633F-14CE-42E3-B174-FCCD48B11FF3.jpg)

打开文件夹，内容如下：

![](//www.runoob.com/wp-content/uploads/2014/11/C2CEBAA0-30B9-4340-8D23-78F6FEB8CBE2.png")

打开一个 **cmd** 窗口 使用 cd 命令切换目录到 **C:\\redis** 运行：

redis-server.exe redis.windows.conf

如果想方便的话，可以把 redis 的路径加到系统的环境变量里，这样就省得再输路径了，后面的那个 redis.windows.conf 可以省略，如果省略，会启用默认的。输入之后，会显示如下界面：

![Redis 安装](//www.runoob.com/wp-content/uploads/2014/11/redis-install1.png)

这时候另启一个 cmd 窗口，原来的不要关闭，不然就无法访问服务端了。

切换到 redis 目录下运行:

redis-cli.exe -h 127.0.0.1 -p 6379

设置键值对:

set myKey abc

取出键值对:

get myKey

![Redis 安装](//www.runoob.com/wp-content/uploads/2014/11/redis-install2.jpg)

* * *

## Linux 源码安装

**下载地址：**<http://redis.io/download>，下载最新稳定版本。

本教程使用的最新文档版本为 2.8.17，下载并安装：

\# wget http://download.redis.io/releases/redis-6.0.8.tar.gz
# tar -xzvf redis-6.0.8.tar.gz
# cd redis-6.0.8
# make

执行完 make 命令后，redis-6.0.8 的 src 目录下会出现编译后的 redis 服务程序 redis-server，还有用于测试的客户端程序 redis-cli：

下面启动 redis 服务：

\# cd src
# ./redis-server

注意这种方式启动 redis 使用的是默认配置。也可以通过启动参数告诉 redis 使用指定配置文件使用下面命令启动。

\# cd src
# ./redis-server ../redis.conf

**redis.conf** 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

启动 redis 服务进程后，就可以使用测试客户端程序 redis-cli 和 redis 服务交互了。 比如：

\# cd src
# ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"

* * *

## Ubuntu apt 命令安装

在 Ubuntu 系统安装 Redis 可以使用以下命令:

\# sudo apt update
# sudo apt install redis-server

### 启动 Redis

\# redis-server

### 查看 redis 是否启动？

\# redis-cli

以上命令将打开以下终端：

redis 127.0.0.1:6379>

127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。

redis 127.0.0.1:6379> ping
PONG

以上说明我们已经成功安装了redis。

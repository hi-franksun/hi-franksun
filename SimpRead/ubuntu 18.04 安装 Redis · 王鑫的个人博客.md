\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[wangxin1248.github.io\](https://wangxin1248.github.io/linux/2018/07/ubuntu18.04-install-redis.html)

12 July, 2018. It was a Thursday.

view:

Redis 是一款内存键值存储，以其灵活性，性能和广泛的语言支持而闻名。本教程将演示如何在 Ubuntu 18.04 服务器上安装和配置 Redis。主要内容包括：

1.  安装 Redis
2.  Redis 配置
3.  Redis 控制

一、安装 Redis
----------

使用 apt 从官方 Ubuntu 存储库来安装 Redis：

```
$ sudo apt update
$ sudo apt install redis-server


```

二、Redis 配置
----------

打开 Redis 的配置文件

```
$ sudo vi /etc/redis/redis.conf


```

在文件中，找到 supervised 指令。 该指令允许您声明一个 init 系统来管理 Redis 作为服务，从而为您提供对其操作的更多控制。 受 supervised 指令默认设置为 no 。 由于您正在运行使用 systemd init 系统的 Ubuntu，请将其更改为 systemd ：

```
\# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY\_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART\_JOB or NOTIFY\_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd


```

重新加载 Redis 服务文件以反映您对配置文件所做的更改：

```
$ sudo service redis restart


```

查看 Redis 的运行状态：

```
$ sudo systemctl status redis


```

输出如下结果表示运行成功：

![](https://wangxin1248.github.io/assets/images/2018-07/05-ubuntu-install-redis.png)

现在已经成功安装和配置了 Redis 并且已经开始运行。

三、Redis 控制
----------

### redis 服务控制

启动 redis 服务：

```
$ sudo service redis start


```

关闭 redis 服务：

```
$ sudo service redis stop


```

重启 redis 服务：

```
$ sudo service redis restart


```

### redis 客户端连接

在命令行中输入如下命令来登陆进 redis 客户端

### redis 远程连接

打开配置文件

```
$ sudo vi /etc/redis/redis.conf


```

将 bind 127.0.0.1 ::1 改为 bind 0.0.0.0

之后重新启动 redis

四、Redis 设置密码
------------

打开 Redis 的配置文件

```
$ sudo vi /etc/redis/redis.conf


```

找到下面这一行

将注释符号去掉，将后面修改成自己的密码，如:
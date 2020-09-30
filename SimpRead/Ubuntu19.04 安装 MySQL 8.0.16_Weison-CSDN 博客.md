\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[blog.csdn.net\](https://blog.csdn.net/weixx3/article/details/94133847)

Ubuntu19.04 安装 MySQL 8.0.16
===========================

环境信息：  
OS：Ubuntu19.04  
OpenJDK：8  
MySQL: 8.0.16

在`Ubuntu`中，默认情况下，只有最新版本的`MySQL`包含在`APT`软件包存储库中, 要安装它，只需更新服务器上的包索引并安装默认包`apt-get`。

`MySQL 连接工具 Workbench 安装见 ↓`  
`Ubuntu19.04 安装 MySQL Workbench 8.0.16` **见**–> [链接?](https://blog.csdn.net/weixx3/article/details/94117380)

1\. 添加 MySQL APT Repository
---------------------------

把`MySQL APT repository` 添加至系统的软件仓库列表里，需要下载`mysql-apt-config`包；  
`下载页面`：[https://dev.mysql.com/downloads/file/?id=487007](https://dev.mysql.com/downloads/file/?id=487007)  
![](https://img-blog.csdnimg.cn/20190629105941699.png)  
点击安装后进行 2 的步骤。

2\. 通过 apt 安装 MySQL
-------------------

```
#命令1
sudo apt-get update
#命令2
sudo apt-get install mysql-server

```

![](https://img-blog.csdnimg.cn/20190629110849710.png)

3\. 配置 MySQL
------------

### 3.1 配置初始化信息

```
sudo mysql\_secure\_installation

```

配置说明：

```
weison@ubuntu19-04:~$ sudo mysql\_secure\_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No: N（选择N，不会进行密码的强校验）
Please set the password for root here.

New password: 

Re-enter new password: 
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : N（选择N，不删除匿名用户）

 ... skipping.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : N（选择N，允许root远程连接）

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : N（选择N，不删除test数据库）

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y（选择Y，修改权限立即生效）
Success.

All done! 


```

### 3.2 配置访问权限

在 Ubuntu 下 MySQL 缺省是只允许本地访问的，使用 workbench 连接工具是连不上的；  
如果你要其他机器也能够访问的话，需要进行配置；

#### 3.2.1 配置远程访问

```
\# 登陆mysql
sudo mysql -uroot -p
#切换数据库
use mysql;
#查询用户表命令：
select User,authentication\_string,Host from user;

```

![](https://img-blog.csdnimg.cn/20190629120642703.png)

访问权限说明：`host默认都是localhost访问权限`

<table><thead><tr><th>host 值</th><th>访问权限</th></tr></thead><tbody><tr><td>localhost</td><td>本地连接</td></tr><tr><td>%</td><td>本地 + 远程连接</td></tr></tbody></table>

```
#设置权限与密码
GRANT ALL PRIVILEGES ON \*.\* TO 'root'@'%' IDENTIFIED BY 'root';  
#刷新cache中配置
flush privileges;  

```

第二个`'root'`为你给新增权限用户设置的密码，`%`代表所有主机，也可以是具体的 ip；

![](https://img-blog.csdnimg.cn/20190629124013843.png)

#### 3.2.2 新建数据库和用户

这个之前就建好了，所以上边查询 User 的时候能看到：

```
##1 创建数据库studentService
CREATE DATABASE studentService;
##2 创建用户teacher(密码admin) 并赋予其studentService数据库的远程连接权限
GRANT ALL PRIVILEGES ON teacher.\* TO studentService@% IDENTIFIED BY "admin";

```

4.mysql 服务命令
------------

*   4.1 检查服务状态

```
systemctl status mysql.service
或
sudo service mysql status

```

显示如下结果说明 mysql 服务是正常的：

```
weison@ubuntu19-04:~$ systemctl status mysql.service
● mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2019-06-29 11:10:10 CST; 15min ago
 Main PID: 8545 (mysqld)
    Tasks: 28 (limit: 4915)
   Memory: 173.4M
   CGroup: /system.slice/mysql.service
           └─8545 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid

6月 29 11:10:10 ubuntu19-04 systemd\[1\]: Starting MySQL Community Server...
6月 29 11:10:10 ubuntu19-04 systemd\[1\]: Started MySQL Community Server.


```

![](https://img-blog.csdnimg.cn/20190629112553128.png?)

*   4.1 mysql 服务启动停止

```
#停止
sudo service mysql stop
#启动
sudo service mysql start

```

5\. 使用 workbench 连接数据库
----------------------

用第 3 步新建的用户`teacher`登陆数据库`studentService`，root 用户我是咋都没登陆进取。

*   5.1 打开 workbench 进行连接配置：  
    ![](https://img-blog.csdnimg.cn/20190629132632133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXh4Mw==,size_16,color_FFFFFF,t_70)
*   5.2 配置完成后，执行 sql：  
    ![](https://img-blog.csdnimg.cn/20190629132935231.png)  
    参考资料：  
    \[1\]. [https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
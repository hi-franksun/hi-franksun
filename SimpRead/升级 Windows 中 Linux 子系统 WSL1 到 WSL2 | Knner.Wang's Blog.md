\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[knner.wang\](https://knner.wang/2020/04/14/upgrade-wsl1-to-wsl2.html)

在最新版本的 Windows 中，已经可以使用 WSL2 了，对于忍受了很长时间的 WSL1 的速度的我，当然是升级到 WSL2 了。

WSL 2 的主要目标是提高文件系统性能并增加完全的系统调用兼容性，WSL 2 是对基础体系结构的一次重大改造，它使用虚拟化技术和 Linux 内核来实现其新功能。因为它采用的完整的 Linux 内核，所以例如 Docker，Kubernetes 都可以安装了。

WSL2 官网文档：[https://aka.ms/wsl2](https://aka.ms/wsl2)

> 参考：
> 
> [https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-about](https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-about)
> 
> [https://github.com/microsoft/WSL2-Linux-Kernel](https://github.com/microsoft/WSL2-Linux-Kernel)

*   WSL 2 中的 Linux 内核是根据最新的稳定版分支（基于 kernel.org 上提供的源代码）在内部构建的。此内核专门针对 WSL 2 进行了优化。 它针对大小和性能进行了优化，在 Windows 上可提供令人惊艳的 Linux 体验，并将通过 Windows 更新提供维护，这意味着你将获得最新的安全修复和内核改进，无需自己管理它。  
    此外，此内核将是开源的。 可以在此处找到此 Linux 内核的完整源代码。
*   WSL 2 使用最新、最强大的虚拟化技术在轻量级实用程序虚拟机 (VM) 中运行其 Linux 内核。 但是，WSL 2 不会是传统的 VM 体验。 传统的 VM 体验可能启动速度慢，是独立的，消耗大量资源，需要你花费时间进行管理。 WSL 2 没有这些属性。 它仍然能提供 WSL 1 的卓越优势：Windows 和 Linux 之间高度集成，启动极快，资源占用较少，最重要的是，不需要你配置或管理虚拟机。 虽然 WSL 2 确实使用 VM，但它将在幕后进行管理和运行，因此你将具有与 WSL 1 相同的用户体验。
*   文件密集型操作（例如 git clone、npm install、apt update、apt upgrade）和其他操作的速度都明显提升。 实际的速度提升将取决于你运行的应用程序以及它与文件系统的交互方式。 在解压压缩的 tarball 时，WSL 2 的初始版本的运行速度比 WSL 1 快达 20 倍，在各种项目上使用 git clone、npm install 和 cmake 时，大约快 2-5 倍。
*   Linux 二进制文件使用系统调用来执行许多功能，例如访问文件、请求内存、创建进程，等等。 虽然 WSL 1 使用的是由 WSL 团队构建的转换层，但 WSL 2 包括了自己的 Linux 内核，具有完全的系统调用兼容性。 这将引入一组全新的应用，你可以在 WSL 内部运行它们，例如 Docker 和其他应用。 此外，对 Linux 内核的任何更新都可以立即准备好，以便添加到你的计算机中，而不是等待 WSL 团队来实现更改并添加它们。

*   Windows 版本：2004，需要开启 Windows 预览体验计划，慢速的即可，如下图：
    
    ![](https://knner.wang/images/upgrade-wsl1-to-wsl2/winver.png)
    
    ![](https://knner.wang/images/upgrade-wsl1-to-wsl2/2004.png)
    

*   由于开了 WSL2（需要依赖 Hyper-v）将会导致 VMware Workstation 无法使用，至少目前还不能兼容，虽然说 Hyper-v 已经和 VMware 确认后期将会兼容，但是还是未知的时间，所以请确认后在进行升级，如下图：
    
    ![](https://knner.wang/images/upgrade-wsl1-to-wsl2/vmware-err.png)
    
    ```
    VMware Workstation 与 Device/Credential Guard 不兼容。在禁用 Device/Credential Guard 后，可以运行 VMware Workstation。有关更多详细信息，请访问 http://www.vmware.com/go/turnoff\_CG\_DG。
    
    
    ```
    

但最近，VirtualBox 和 VMware 都发布了支持 Hyper-V 和 WSL2 的版本！ 可[在此处了解有关 VirtualBox 的更改的详细信息](https://www.virtualbox.org/wiki/Changelog-6.0)，并可[在此处了解有关 VMware 的更改的详细信息](https://blogs.vmware.com/workstation/2020/01/vmware-workstation-tech-preview-20h1.html)。

*   WSL2 无法访问 GPU、串行或者 USB 设备。
*   在 Hyper-V 管理器中是看不到 WSL2 虚拟机的。
*   WSL2 不支持 IPv6。

确认以上步骤，即可升级 WSL2 了。

启用`虚拟机平台`和`适用于Linux的Windows子系统`两个组件：启用方式，分为两种方式，任选其一即可：

[](#命令行方式启用Windows功能 "命令行方式启用Windows功能")命令行方式启用 Windows 功能
----------------------------------------------------------

Windows + x 快捷键，选择`Windows PowerShell(管理员)`，输入一下命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart


```

这个更简单一些，推荐。

[](#图形化方式启用Windows功能 "图形化方式启用Windows功能")图形化方式启用 Windows 功能
----------------------------------------------------------

由于需要用到控制面板，所以先开启控制面板：

### [](#添加控制面桌面快捷方式 "添加控制面桌面快捷方式")添加控制面桌面快捷方式

Windows 设置–> 个性化–> 主题–> 桌面图标设置–> 勾选控制面板，如下图：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/desktop-icon0.png)

勾选所需要的：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/desktop-icon.png)

### [](#通过控制面板开启Windows功能 "通过控制面板开启Windows功能")通过控制面板开启 Windows 功能

打开桌面控制面板，找到`程序`，点击进去，选择`启用或者关闭Windows功能`：如下图：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/win-func1.png)

点击进入：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/win-func2.png)

依次勾选：

*   Hyper-V
*   适用于 Linux 的 Windows 子系统。

这两种方式开启 Windows 功能后，都需要重启的。

[](#确认WSL1名称以及版本 "确认WSL1名称以及版本")确认 WSL1 名称以及版本
----------------------------------------------

下面就开始升级 WSL1 到 WSL2 了，首先查看 WSL1，确认名称为 Ubuntu，版本为 1：

```
C:\\Users\\admin>wsl -l -v
  NAME      STATE           VERSION
\* Ubuntu    Running         1


```

[](#升级WSL1到WSL2-1 "升级WSL1到WSL2")升级 WSL1 到 WSL2
----------------------------------------------

### [](#首先安装内核： "首先安装内核：")首先安装内核：

下载地址：[https://aka.ms/wsl2kernel](https://aka.ms/wsl2kernel)

直接一路下一步即可。

如果不安装，在升级的时候也会提示安装：

```
C:\\Users\\admin>wsl --set-default-version 2
WSL 2 需要更新其内核组件。有关信息，请访问 https://aka.ms/wsl2kernel


```

### [](#升级到WSL2 "升级到WSL2")升级到 WSL2

查看当前 WSL1 的名称为 Ubuntu，通过下面命令，将 Ubuntu 升级为 WSL2：

需要等待将近 30 分钟，可能我的 WSL1 的文件太多导致的。

```
wsl --set-version Ubuntu 2
正在进行转换，这可能需要几分钟时间...
有关与 WSL 2 的主要区别的信息，请访问 https://aka.ms/wsl2
转换完成。


```

这里只是将 Ubuntu 这个分支升级为了 WSL2，如果后续安装其他的子 Linux，默认还是 WSL1 的，可以通过下面的命令更改默认为 WSL2 版本，这样新安装的子系统就都是 WSL2 了。

```
wsl --set-default-version 2


```

注意：这种方式只是更改默认的版本为 WSL2，并不会更新 WSL1 到 WSL2 版本。

转换完后，查看：

```
C:\\Users\\admin>wsl -l -v
  NAME      STATE           VERSION
\* Ubuntu    Stopped         2


```

目前是 Stopped 的，通过执行 wsl 命令启动并进入 Ubuntu：

[](#网络 "网络")网络
--------------

WSL2 可以认为就是一个完整的 Linux，只是微软将其优化，与 Windows 更加融合。

所以 WSL2 中已经有了 IP 地址：

```
$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.27.148.229  netmask 255.255.240.0  broadcast 172.27.159.255
        inet6 fe80::215:5dff:fede:2245  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:de:22:45  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10  bytes 796 (796.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.27.144.1    0.0.0.0         UG    0      0        0 eth0
172.27.144.0    0.0.0.0         255.255.240.0   U     0      0        0 eth0


wanghk@wanghk-laptop:/mnt/c/Users/wanghk$ cat /etc/resolv.conf



nameserver 172.27.144.1


```

在 Windows 中查看，也多出了两块虚拟网卡：

```
以太网适配器 vEthernet (Default Switch):

   连接特定的 DNS 后缀 . . . . . . . :
   描述. . . . . . . . . . . . . . . : Hyper-V Virtual Ethernet Adapter
   物理地址. . . . . . . . . . . . . : 00-15-5D-0D-7D-B4
   DHCP 已启用 . . . . . . . . . . . : 否
   自动配置已启用. . . . . . . . . . : 是
   本地链接 IPv6 地址. . . . . . . . : fe80::8c05:5e96:f21a:f9b3%44(首选)
   IPv4 地址 . . . . . . . . . . . . : 172.28.224.1(首选)
   子网掩码  . . . . . . . . . . . . : 255.255.240.0
   默认网关. . . . . . . . . . . . . :
   DHCPv6 IAID . . . . . . . . . . . : 738202973
   DHCPv6 客户端 DUID  . . . . . . . : 00-01-00-01-25-2C-68-8D-F8-CA-B8-26-A3-F9
   DNS 服务器  . . . . . . . . . . . : fec0:0:0:ffff::1%1
                                       fec0:0:0:ffff::2%1
                                       fec0:0:0:ffff::3%1
   TCPIP 上的 NetBIOS  . . . . . . . : 已启用

以太网适配器 vEthernet (WSL):

   连接特定的 DNS 后缀 . . . . . . . :
   描述. . . . . . . . . . . . . . . : Hyper-V Virtual Ethernet Adapter 
   物理地址. . . . . . . . . . . . . : 00-15-5D-82-D0-E6
   DHCP 已启用 . . . . . . . . . . . : 否
   自动配置已启用. . . . . . . . . . : 是
   本地链接 IPv6 地址. . . . . . . . : fe80::b5b1:a0db:d0ff:5647%50(首选)
   IPv4 地址 . . . . . . . . . . . . : 172.27.144.1(首选)
   子网掩码  . . . . . . . . . . . . : 255.255.240.0
   默认网关. . . . . . . . . . . . . :
   DHCPv6 IAID . . . . . . . . . . . : 838866269
   DHCPv6 客户端 DUID  . . . . . . . : 00-01-00-01-25-2C-68-8D-F8-CA-B8-26-A3-F9
   DNS 服务器  . . . . . . . . . . . : fec0:0:0:ffff::1%1
                                       fec0:0:0:ffff::2%1
                                       fec0:0:0:ffff::3%1
   TCPIP 上的 NetBIOS  . . . . . . . : 已启用


```

### [](#Windows访问Linux服务： "Windows访问Linux服务：")Windows 访问 Linux 服务：

体验跟 WSL1 是一样的，直接通过`localhost`访问即可。

### [](#Linux访问Windwos服务： "Linux访问Windwos服务：")Linux 访问 Windwos 服务：

这里更改了，没有办法通过`localhost`访问，首先需要确认 Windows 的 IP 地址：在子 Linux 中查看 nameserver 这个就是 Windows 的地址：

```
$ cat /etc/resolv.conf
nameserver 172.27.144.1


```

需要使用这个 IP + 端口的方式访问本地 Windows 的服务。

需要注意的是：WSL2 目前还不支持 IPv6.

### [](#Linux子系统安装SSH，windows可以SSH远程连接子系统 "Linux子系统安装SSH，windows可以SSH远程连接子系统")Linux 子系统安装 SSH，windows 可以 SSH 远程连接子系统

在 windows 中，可以通过执行`wsl`/`bash`命令，可以进入 Ubuntu，如果需要是用`Scrt`或者`Xshell`呢，就需要在 Linux 安装 SSHd，并开机自启动 SSHd 服务。

#### [](#安装sshd服务： "安装sshd服务：")安装 sshd 服务：

```
sudo apt-get purge openssh-server

sudo apt-get install openssh-server

sudo vim /etc/ssh/sshd\_config
PermitRootLogin no
AllowUsers yourusername
PasswordAuthentication yes
UsePrivilegeSeparation no


sudo service ssh --full-restart


```

#### [](#开机自动启动sshd "开机自动启动sshd")开机自动启动 sshd

创建扩展名为. bat 的文件，内容如下：

```
@echo off
%1(start /min cmd.exe /c %0 :&exit)
C:\\Windows\\System32\\bash.exe -c "sudo /etc/init.d/ssh start > /tmp/start\_sshd.log 2>&1"


```

将该文件放到：

```
\# 将admin换成你的用户名：
C:\\Users\\admin\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\Startup


```

### [](#将WSL2的IP地址更新到Windows-hosts文件中，通过ubuntu-wsl名称访问WSL2 "将WSL2的IP地址更新到Windows hosts文件中，通过ubuntu.wsl名称访问WSL2")将 WSL2 的 IP 地址更新到 Windows hosts 文件中，通过 ubuntu.wsl 名称访问 WSL2

虽然 windows 的内能通过 localhost（BUG：无法通过 127.0.0.1 访问）访问 WSL2 的端口，但是有些应用的访问还是有问题，比如`pycharm`或者`goland`中的 Database，如果通过 localhost 则连接不上 WSL2 内的`mysql`端口，这时候就需要将 WSL2 的 IP 地址更新到 Windows hosts 文件中了。

> github 地址：
> 
> [https://github.com/shayne/go-wsl2-host](https://github.com/shayne/go-wsl2-host)

通过 release 下载 exe 文件，保存好到特定的位置，以后就不要移动了。

以管理员身份运行 cmd，运行下列命令：

```
wsl2host.exe install

# 提示输入用户和密码，对于都是windows 网络账号的，可以直接回车，不用输入。如果是本地账户，可以输入用户名或者密码。


```

Win + R 打开运行窗口，输入：`services.msc`，打开 Windows 系统服务，找到：wsl2host，按如下图修改：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/wsl2host-svc-1.png)

上图修改完后，先点击应用，然后再点击`常规`，启动服务，如下图：

![](https://knner.wang/images/upgrade-wsl1-to-wsl2/wsl2host-svc-2.png)

查看 hosts 文件，会有一条记录：

```
172.23.103.128 ubuntu.wsl  # managed by wsl2-host


```

往后就可以通过`ubuntu.wsl`访问了。

[](#磁盘 "磁盘")磁盘
--------------

WSL2 已经是使用 vhdx 的虚拟硬盘了，一般的存放位置在于：

将 admin 换成你的用户名：

```
C:\\Users\\admin\\AppData\\Local\\Packages\\CanonicalGroupLimited.UbuntuonWindows\_79rhkp1fndgsc\\LocalState>ls -lh
total 5G
-rw-rw-r--    1 wanghk   wanghk      5.4G Apr 14 14:19 ext4.vhdx
-rw-rw----    1 root     root           0 Apr 14 09:35 fsserver
drwxrwxr-x    2 wanghk   wanghk      4.0K Apr 14 10:06 temp


```

默认的 vhd 磁盘大小最大为 256G，当超过的时候，会提示磁盘不足，可以通过扩展 vhd 来解决：

> 参考官方文档：中文：
> 
> [https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-ux-changes#understanding-wsl-2-uses-a-vhd-and-what-to-do-if-you-reach-its-max-size](https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-ux-changes#understanding-wsl-2-uses-a-vhd-and-what-to-do-if-you-reach-its-max-size)

### [](#Linux访问Windows文件系统 "Linux访问Windows文件系统")Linux 访问 Windows 文件系统

访问方式还是没有变化，直接通过：

```
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb        251G  5.0G  234G   3% /
tmpfs           6.3G     0  6.3G   0% /mnt/wsl
tools           100G   85G   15G  86% /init
none            6.3G     0  6.3G   0% /dev
none            6.3G   12K  6.3G   1% /run
none            6.3G     0  6.3G   0% /run/lock
none            6.3G     0  6.3G   0% /run/shm
none            6.3G     0  6.3G   0% /run/user
tmpfs           6.3G     0  6.3G   0% /sys/fs/cgroup
C:\\             100G   85G   15G  86% /mnt/c 
D:\\             138G  129G  9.4G  94% /mnt/d 


```

[](#内核 "内核")内核
--------------

```
$ uname -a
Linux admin-laptop 4.19.84-microsoft-standard 


```

[](#wsl-conf "wsl.conf")wsl.conf
--------------------------------

WSL2 同样支持`/etc/wsl.conf`的，具体请参考：[https://docs.microsoft.com/zh-cn/windows/wsl/wsl-config](https://docs.microsoft.com/zh-cn/windows/wsl/wsl-config)

这里使用阿里云的源，安装参考：

[https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.3e221b110Km2NH](https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.3e221b110Km2NH)

[](#安装Docker-ce "安装Docker-ce")安装 Docker-ce
------------------------------------------

```
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb \[arch=amd64\] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb\_release -cs) stable"

sudo apt-get -y update
sudo apt-get -y install docker-ce










```

[](#配置docker "配置docker")配置 docker
---------------------------------

```
sudo mkdir /etc/docker
sudo cat > /etc/docker/daemon.json <<EOF
{
  "log-level": "warn",
  "selinux-enabled": false,
  "max-concurrent-downloads": 10,
  "max-concurrent-uploads": 5,
  "live-restore": true,
  "default-shm-size": "128M",
  "registry-mirrors": \["https://nxxxxxx5.mirror.aliyuncs.com","http://exxxxxd.m.daocloud.io"\],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "20m",
    "max-file": "5"
  },
  "storage-driver": "overlay2",
  "storage-opts": \[
    "overlay2.override\_kernel\_check=true"
  \]
}
EOF


```

[](#启动Docker "启动Docker")启动 Docker
---------------------------------

因为 Ubuntu 还是 18.03，不支持 systemd 方式，需要使用`service`，或者`/etc/init.d/docker`:

```
$ sudo /etc/init.d/docker 
Usage: service docker {start|stop|restart|status}

$ sudo /etc/init.d/docker status
 \* Docker is not running

$ sudo /etc/init.d/docker start
 \* Starting Docker: docker                                                                    \[ OK \] 

$ sudo docker info
Client:
 Debug Mode: false

Server:
 Containers: 2
  Running: 1
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 19.03.8
 Storage Driver: overlay2
  Backing Filesystem: <unknown>
  Supports d\_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.84-microsoft-standard
 Operating System: Ubuntu 18.04.3 LTS
 OSType: linux
 Architecture: x86\_64
 CPUs: 4
 Total Memory: 12.42GiB
 Name: wanghk-laptop
 ID: PKID:EVOC:CTIZ:6I7H:QWOW:BBFZ:62IH:GQ56:4O3X:J4RY:ESLW:GO7U
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://nxxxxxxxx5.mirror.aliyuncs.com/
  http://exxxxxxxxd.m.daocloud.io/
 Live Restore Enabled: true


```

注意：WSL2 中 docker 不支持，cgroupdriver=systemd 方式，所以不要加下面的配置，而是使用默认的：`cgroupfs`

```
"exec-opts": \["native.cgroupdriver=systemd"\]


```

[](#运行nginx-Docker容器 "运行nginx Docker容器")运行 nginx Docker 容器
----------------------------------------------------------

下载 nginx 镜像：

```
$ sudo docker pull nginx:1.16.1-alpine


```

运行 nginx：

```
$ sudo docker run -d -p 8090:80 nginx:1.16.1-alpine


```

在 Linux 中访问：

```
$ curl localhost:8090
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
.....


```

在 Windows 中访问：

```
C:\\Users\\admin>curl localhost:8090
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
.....


```

或者通过浏览器访问，这里推荐一个 windows 的命令行包管理工具：

*   [scoop](https://scoop.sh/)
*   [Chocolatey](https://chocolatey.org/)

安装 busybox 工具，就能在 windows 中运行 curl 命令了。

> 本文到这里就结束了，欢迎期待后面的文章。您可以关注下方的公众号二维码，在第一时间查看新文章。
> 
> ![](https://knner.wang/images/knner/wx.jpg)
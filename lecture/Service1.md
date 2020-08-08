# Linux网络管理及服务（1）

## Linux网络管理命令

### ipconfig

ifconfig用于查看和更改网络接口的地址和参数。常用参数如下：

- -interface：网络接口名，如eth0和eth1
- up：激活网卡设备
- down：关闭网卡设备
- broadcast address：设置接口的广播地址
- pointpoint：启动点对点方式
- address：指定设备的IP地址
- netmask address：设置子网掩码

### ping

ping使用ICMP协议来检测整个网络连通情况

```shell
ping -c 3 192.168.1.105
```

设置回应3次

```shell
ping -c 3 www.163.com
```

查看本机是否安装TCP/IP，网卡是否工作正常

```shell
ping -c 3 127.0.0.1
```

找出最大MTU数值

```shell
ping -c 3 -s 2000 192.168.1.105
```

查看IP记录路由

```shell
ping -c 3 -R 192.168.1.105
```

### netstat

- -antpu：常用查看命令
- -a：显示已经建立连接的接口
- -rn：显示路由表状态，且直接使用ip及端口号

### traceroute

traceroute是数据包路由跟踪诊断命令，可以查看数据包在网络 上传输的路径情况

```shell
traceroute 192.168.1.105
```

```shell
traceroute www.dlpu.edu.cn
```

```shell
traceroute -n www.sohu.com
```

### dig

dig域信息搜索器，常见用法如下面的例子

```shell
dig sohu.com +nssearch
```

查看包含sohu.com的授权域名服务器，并显示网段中每台域名服务器的SOA记录

```shell
dig dlpu.edu.cn +trace
```

从根服务器开始追踪域名 dlpu.edu.cn的解析过程

```shell
dig -x 210.30.49.180
```

对210.30.49.180 进行逆向查询

```shell
dig www.163.com 
```

根据域名来查询IP地址

### arp

ARP(Address Resolution Protocol，地址解析协议)表也称 ARP缓存，包含一个本地网络上所有MAC地址到IP地址的完整映射。

```shell
arp -a
```

显示本地网络的所有入口

```shell
arp -v 
```

```shell
arp -a -n 192.168.1.105
```

```shell
arp -s 192.168.1.105 00:0C:29:75:B9:BD
```

将IP和物理地址绑定

```shell
arp -H ether
```

查看ether类型的网卡

```shell
arp -a 10.10.1.24
```

显示主机10.10.1.24的所有入口

## deb配置和使用

更新日志：/var/log/dpkg.log

其他用法：

sudo apt search 'tag' 通过关键词搜索包

sudo apt show <package> 查看包信息

sudo apt autoremove 删除不需要的包

下载的包的位置：/var/cache/apt/archives

索引目录：/var/lib/apt/

sudo apt download <package> 下载包

sudo apt source <package> 下载源码

sudo apt showsrc

更新配置：

更新源位置：/etc/apt/sources.list

源的内容：

deb http://cn.archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse

cdrom:安装的时候用的老旧的源

第一列：deb表示安装包，deb-src表示源码包

第二列：软件仓库的url地址

第三列：系统的codename

sudo lsb_release -a查看系统信息

第四列：库的内容构成
    main：官方支持，提供源码，官方维护bug

​    restricted：官方支持，不提供源码

​    universe：社区支持

​    multiverse：不开源，没有支持，可能存在危险
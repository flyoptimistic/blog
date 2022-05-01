---
title: 云服务器创建swap分区
date: 2022-05-01 10:04:06
tags: 
	- 服务器
categories:
	- 服务器



---

# 云服务器创建swap分区

## 前言

随着云服务的普及，很多人都购买了云主机来部署一些简单的服务，但通常出于成本的考虑，不会购买特别大型的机器，在有限的硬件资源下，如何让服务器响应更快？如何避免应用出现内存不足的错误？最简单的方法就是增加交换空间(Swap)。

<!-- more -->

Swap是存储盘上的一块自留地，操作系统可以在这里暂存一些内存里放不下的东西。这从某种程度上相当于增加了服务器的可用内存。虽然从swap读写比内存慢，但总比没有好，算是内存不够时的安全网。如果没有swap，则服务器一旦内存不足，就会开始终止应用以释放内存，甚至会崩溃，这会让你丢失一些还没来得及保存的数据，或者造成宕机。有些应用明确要求系统配置swap以确保数据访问的可靠性。

Swap分区作用:当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap中恢复保存的数据到内存中。这样，系统总是在物理内存不够时，才进行Swap交换。

在Windows系统中，这种交换空间叫做虚拟内存，可以在系统->高级->性能->虚拟内存中进行配置。Linux系统下的交换空间配置略为不方便。因此，本文参考了网上的多篇博文，总结记录了在CentOS 7系统的云服务器上创建并启用swap文件的过程中的命令和注意事项。

## 准备工作

### 检查系统的Swap信息

~~~shell
#查看swap使用情况,如果该命令没有返回出结果，则代表该系统尚未配置过swap。
$ swapon -s

#查看内存状态
$ free -m
~~~

### 检查可用的存储空间

~~~shell
#检查磁盘可用空间
$ df -h
~~~

合适的swap空间是多大？关于这个问题有很多种选择，这取决于你的应用需求和你个人的偏好。一般来说，内存容量的一到两倍就是个不错的选择。Redhat官方推荐的swap大小设置如下：

![swap大小](swap大小.png)



## 定义swap文件大小及位置

~~~shell
$ dd if=/dev/zero of=/swapfile bs=1k count=2048000
~~~

- of:创建swap文件分区的名称

- bs:blocksizes，每个块大小为1k

- count=2048000
- 总大小为 bs * count = 2G的文件。

**注意**：此处不使用`fallocate`命令来创建swap文件，`fallocate`可能比快`dd`，但它不适合创建交换文件，并且不受交换相关工具的支持。

## 创建swap

~~~shell
#更改swap文件权限，确保只有root权限才可读。
$ chmod 600 /swapfile
#基于该文件用于创建swap
$ mkswap /swapfile
~~~

## 启动swap

~~~shell
$ swapon /swapfile
~~~

## 系统自启动

~~~shell
#       设备名      地址  格式  参数类型 不备份 不检查
$ echo "/swapfile swap swap defaults 0 0" >>/etc/fstab
~~~

## 删除分区

~~~shell
$ swapoff /swapfile
$ swapoff -a >/dev/null
$ rm -rf /swapfile
~~~

## 优化

### Swappiness

`swappiness`参数决定了系统将数据从内存交换到swap空间的频率，数值设置在0到100之间，代表系统将数据从内存交换到swap空间的力度。

该数值越接近于0，系统越倾向于不进行swap，仅在必要的时候进行swap操作。由于swap要比内存慢很多，因此减少对swap的依赖意味着更高的系统性能。

该数值越接近于100，系统越倾向于多进行swap。有些应用的内存使用习惯更适合于这种情况，这也于服务器的用途有关。输入如下命令查看当前的swappiness数值：

~~~shell
$ cat /proc/sys/vm/swappiness
~~~

CentOS 7默认设置了30的swappiness，这对于大部分桌面系统和本地服务器是比较中庸的数值。对于VPS系统而言，可能接近于0的值是更加合适的。使用`sysctl`命令可以修改swappiness。比如将swappiness设为10：

~~~shell
#重启失效
$ sysctl vm.swappiness=10
#永久有效
$ vim /etc/sysctl.conf
#如果没有，加到文件末尾
 vm.swappiness=10 
~~~

### 缓存压力（Cache Pressure ）

该配置项涉及特殊文件系统元文件条目的存储。对此类信息的频繁读取是非常消耗性能的，所以延长其在缓存的保存时间可以提升系统的性能。通过`proc`文件系统查看缓存压力的当前设定值：

~~~shell
$ cat /proc/sys/vm/vfs_cache_pressure
~~~

这个数值是比较高的，意味着系统从缓存中移除inode信息的速度比较快。一个保守一些的数值是50，使用`sysctl`命令进行设置：

~~~shell
#重启失效
$ sysctl vm.vfs_cache_pressure=50
#永久有效，编辑/etc/sysctl.conf
$ vim /etc/sysctl.conf
#修改配置项vm.vfs_cache_pressure,若果没有，则将其添加到末尾
vm.vfs_cache_pressure=50
~~~

## 总结

至此，我们的系统内存就获得了一些喘气的空间。有了swap空间可以有效避免一些常见的问题。

如果你仍然会遇到内存不足（OOM，out of memory）的错误信息，或者你的系统不能运行你需要的应用，那么最好的方法是优化你的应用配置或者升级你的服务器，不过配置swap空间也不失为一个灵活的节省方案。
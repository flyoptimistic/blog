---
title: Linux 基础命令
date: 2020-08-18 10:04:06
tags: 
	- Linux
	- 命令
categories:
	- 操作系统
	- Linux

description: 常用的linux命令

---



## 基础命令：

~~~shell
ls  ll  alias   

ln -s 	源文件  目标文件				软连接

查看文件权限： 文件属主，添加可执行权限

ll 文件权限、文件属主： 

chown  -R amico:amico  /eig8t  
将eig8t 文件夹下的所有内容，全部修改属主,属组为amico

chmod +x start.sh    给脚本加可执行权限
chmod -x start.sh 	给文件去掉可执行权限

chmod  777   create_amico.sh    给文件加所有权限

-rwx rwx rwx  1 root root       79 8Ղ  22 18:50      - 开头代表文件

chmod  777   create_amico    给文件夹加所有权限

drw xrw xrwx  1 root root       79 8Ղ  22 18:50 	   d 开头代表文件夹
~~~



​	


使用手册：

tab ---补全
man手册查看帮助、
help


例如：


进程查看，端口查看：
lsof -i:port   （yum -y install lsof ）  

ps -ef | grep  nginx

netstat -tunlp | grep  port|进程|进程PID



简单的检测http接口：

curl -I 内网ip:端口/参数     

例如：  curl -I  172.16.1.212:8080      返回是否 200 状态码


不通的情况： 考虑，接口问题、防火墙问题

接口问题：查找项目存放log   （tail -f /xxx/xxx/out.log）

如果是系统的软件问题一般 log 都会在 /var/log/ 下



journalctl -f   or   tailf /var/log/syslog   监听系统日志


tailf /xxx/project/out.log     监听项目日志

tail -200f /xxx/project/out.log  查看日志 后200 行

tail -500f /xxx/project/out.log |grep -C  20  "error"   显示后500行日志中 带error关键字的前后20行 

-A  打印匹配行的后5行
-B  打印匹配行的前5行


防火墙问题：
内网：
systemctl stop firewalld

关闭selinux:
cd /etc/selinux/config
sed -i "s/enforcing/disable/g" /etc/selinux/config
setenforce 0     关闭selinux

关闭 iptables:

iptables -nvL ---查看当前规则
iptables -F --- 关闭

--------------------------------------------------------------------

vim 修改基本操作：

:set nu   显示行数

:set nonu  取消显示行数

:167   到167 行

gg       回到编辑文件最开头
Shift +g   去到编辑文件最末尾



查找：

/source  正向查找，按n下一个

?source  反向查找， 按n上一个



替换：

:%s/ccccaa/amico/g      把文件中全部的 ccccaa 替换成 amico

:%s/39.108.250.194/3.3.3.3/g

:63,73s/old/new/g     

:%s#/swaggerui/usr/local#/opt#g     替换带有歧义的 文本内容的时候 
									使用 # # #, 或 % % % ， 或 "\" 进行转义

sed -i 's%old%new%g'   /path/xxx.file

sed -i 's/ldnew#g'   /path/xxx.file 

sed -i 's#old#new#g'   /path/xxx.file 




批量注释、批量修改

i      插入，进入编辑状态
esc    回到非编辑状态

u   撤销修改
ctrl + r  还原撤销修改
Shift + A    在光标行的最后插入
o 	 	在光标行的下一个进行插入

yy	  复制一行
3yy   复制三行

dd    删除（剪切）光标所在行

3dd   删除（剪切）从光标开始的 三行

p      粘贴 上面剪切或者是 yy 复制的内容

批量注释：
1、  ctrl + v  键盘向下选择需要注释的行

2、  Shift  + i

3、  Shift  + #

4、 连按两次 Esc


取消批量注释：

1、 ctrl + v  键盘向下选择需要注释的行

2、 x    


查看磁盘、查看cpu、内存     vmstat 2 ---> 磁盘io等内容


top (Shift + p、 Shift  + m)    cpu 的 1、5、15分钟使用百分比/core		

Shift + m   按内存使用大小排序

Shift + p   按cpu使用大小排序


history     查看历史执行过的命令

!254    执行第254条历史命令

!!   执行上一条命令


ls /usr/local/src 
cd !$      执行上一条命令的最后一个参数


free -m      查看内存




安装基础软件。 redis、zookeeper、jdk、nginx
安装版本选择原则： 不新不旧

redis为例：


一、
1.源码下载
# wget http://download.redis.io/releases/redis-5.0.3.tar.gz

2.解压
# tar xf redis-4.0.11.tar.gz -C /usr/local/

# cd /usr/local/redis-4.0.11

# mv redis-4.0.11   redis

# make && make install 

编译过程有可能报缺什么编译环境。则缺什么装什么
(yum|apt-get  -y install  make gcc c   )


如果编译时报 “*** 致命错误 *** ”时，使用下面的命令进行编译

# make MALLOC=libc

安装完成


二、
# vim /etc/profile			加入长期环境变量
export PATH=$PATH:/usr/local/redis/src     可在任何地方执行 redis-server 启动命令

echo $PATH      输出当前系统上的 path变量

启动：
redis-server  /usr/local/redis/redis.conf 
	
关闭：
redis-cli  -p 6379 shutdown 

配置文件个别参数......


nginx 配置
nginx -t 、 nginx -s reload 
systemctl restart nginx systemctl stop|start nginx    ---yum或apt安装

环境变量
echo $SHELL ----   脚本 #!/bin/bash 
echo $PATH

编写一个简单的启动脚本。以amico-service.sh 为例-

下面是 amico-service.sh 启动、停止、查看状态脚本。自己项目的对应修改路径即可

--------------------------------------------------------------------
#!/bin/bash

Base_dir=/usr/local/im-service

case $1 in 

     start)
       bash $Base_dir/amico-im-service/bin/amico-im-service $1 
       sleep 1
       bash $Base_dir/amico-im-service1/bin/amico-im-service $1 
    
       echo -e "\033[42;37m all service start done \033[0m"
           ;;
      stop)
       bash $Base_dir/amico-im-service/bin/amico-im-service $1
       sleep 1
       bash $Base_dir/amico-im-service1/bin/amico-im-service $1
    
       echo -e "\033[42;37m all service stop done \033[0m"
       ;;
    status) 
       bash $Base_dir/amico-im-service/bin/amico-im-service $1 
       sleep 1
       bash $Base_dir/amico-im-service1/bin/amico-im-service $1 
       ;;
         *) 
       echo "Usage: $0 { start | stop | status }"
       exit 1
       ;;
esac

----------------------------------------------------------------------
安装软件：
	以Centos7 系列为例：

如果想安装某个知道名字的软件包，例如nginx，lrzsz
则：
	yum -y install nginx lrzsz 

如果想安装指定版本的nginx (添加nginx yum 源后)

    yum --showduplicate list nginx 		---列出可安装的版本
    yum -y install nginx-1:1.16.1-1.el7   ---安装指定版本

知道某条命令，需要安装使用，但不知道要安装的包名：

例如： 知道 rz 上传命令，但  yum -y install rz 无法安装该包

	yum provides "*/rz"     或   yum whatprovides  "*/rz"
	
	apt-file search "*/rz"  (Ubuntu)

列出后，找到对应包后符合的：
	yum -y install lrzsz 



文件上传下载： rz sz 前提安装 yum|apt-get -y install lrzsz 



时间同步：
yum -y install ntpd || apt-get install -y ntpd 

ntpdate  server.org.shanghai   时间服务器可网上找

cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime



***  普通用户借权： sudo 前提 root 配置 /etc/sudoers 

例如：
	 sudo yum -y install lrzsz   




文件压缩：tar 、zip

文件解压： tar -xzf xxx.jdk.tar.gz  -C  /usr/local

文件压缩:  yum -y install unzip  tar 

zip amico.zip -r /amico    或  tar -cvf  test.tar.gz  test/ 


文件传输与登录：ssh、scp

ssh 默认 22 端口远程主机：

	ssh  amico@172.16.1.212 

ssh 非默认 22 端口远程主机：

	ssh -p 2222  amico@172.16.1.212 

scp 文件或文件夹 到远程非 默认端口主机：

	scp -P port -r dir amico@172.16.1.212    (-r 参数 ，scp复制文件夹)
	
	scp -P port xxx.file amico@172.16.1.212 (互相拷贝文件)



阿里云或内网主机查看出口ip：

curl ifconfig.me

查看机器内网ip：

ifconfig
ip addr 


从给定链接下载文件到 /opt 目录下：
wget -P /opt/    https://www.baidu.com/xxx.file   


拉取代码：
git clone  https://github/xxx.project 

更新代码：
git pull  https://github/xxx.project


ss -tnl  查看端口

ss -tnl | grep "8080"

netstat -tunlp | grep "3306"  或   netstat -tunlp | grep "mysql"
查看tcp、udp端口


查找项目pid号。

1. 根据端口找进程pid

pid=lsof -i:8055| awk '{print $2}'

寻找父进程

ps -ef | grep "pid"

ppid=`ps -ef | grep $pid | grep -v 'grep'|  awk '{print $3}'`

杀掉进程：

kill -9 $ppid   若 ppid 为 1 的情况下，则 kill -p $pid


查看进程使用内存：

ps aux |grep "30482"|grep -v "grep"|awk '{sum+=$6}; END{print sum}'

or 

top -p 30482  (RES 单位为 /Kb)


查看磁盘可用空间：

df -h 

查看某个文件大小：

du -sh ./xxx.file


hostname 

hostnamectl   --查看linux服务器信息

cat /etc/redhat-release  
---
title: Git多账号
date: 2022-05-01 10:04:06
tags: 
	- Git
categories:
	- 工具
	- Git


---

# 使用SSH管理Git多账号

## HTTPS 和 SSH 的区别：

- HTTPS可以随意克隆github上的项目，而不管是谁的；而SSH则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。
- https url 在push的时候是需要验证用户名和密码的；而 SSH 在push的时候，是不需要输入用户名的，如果配置SSH key的时候设置了密码，则需要输入密码的，否则直接是不需要输入
  密码的。

<!-- more -->

## 生成ssh公钥

~~~shell
#默认
$ ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  
#指定公钥位置和名称
$ ssh-keygen -t rsa -f ~/.ssh/id_github_rsa -C "xxxxx@xxxxx.com" 
~~~

- `-t` 指定密钥类型，默认是 rsa ，可以省略
- `-C `设置注释文字，比如邮箱
- `-f` 指定密钥文件存储文件名

**提示**：如果本地没有.ssh文件夹，则使用默认创建公钥方式，不配置，直接回车。

## 临时添加验证

~~~shell
$ ssh-agent bash
$ ssh-add id_github_rsa
~~~

## 配置config

在 `~/.ssh `目录下新建一个`config`文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

~~~
# gitee
Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_github_rsa
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_github_rsa
~~~

## 查看所有已添加密钥 

~~~shell
$ ssh-add -l
~~~

## 添加ssh有关信息

~~~shell
$ cat ~/.ssh/id_github_rsa
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
~~~

复制生成后的 ssh key，通过仓库主页 **「管理」->「部署公钥管理」->「添加部署公钥」** ，添加生成的 public key 添加到仓库中。

## 验证是否连接成功

~~~shell
$ ssh -T git@gitee.com
$ ssh -T git@github.com
~~~




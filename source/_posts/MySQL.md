---
title: MySQL
date: 2020-08-18 10:04:06
tags: 
	- 数据库
	- Mysql
categories:
	- 数据库
	- MySQL



---

# MySQL

MySQL 数据库管理系统具有很多的优势，下面总结了其中几种。

## 特点和优势

### MySQL 是开放源代码的数据库

MySQL 是开放源代码的数据库，任何人都可以获取该数据库的源代码。这就使得任何人都可以修正 MySQL 的缺陷，并且任何人都能以任何目的来使用该数据库。MySQL 是一款可以自由使用的数据库。

<!-- more -->

### MySQL 的跨平台性

MySQL 不仅可以在 Windows 系列的操作系统上运行，还可以在 UNIX、Linux 和 Mac OS 等操作系统上运行。因为很多网站都选择 UNIX、Linux 作为网站的服务器，所以 MySQL 的跨平台性保证了其在 Web 应用方面的优势。虽然微软公司的 SQL Server 数据库是一款很优秀的商业数据库，但是其只能在 Windows 系列的操作系统上运行。因此，MySQL 数据库的跨平台性是一个很大的优势。

### 价格优势

MySQL 数据库是一个自由软件，任何人都可以从 MySQL 的官方网站上下载该软件，这些社区版本的 MySQL 都是免费试用的，即使是需要付费的附加功能，其价格也是很便宜的。相对于 Oracle、DB2 和 SQL Server 这些价格昂贵的商业软件，MySQL 具有绝对的价格优势。

### 功能强大且使用方便

MySQL 是一个真正的多用户、 多线程 SQL 数据库服务器。它能够快速、有效和安全的处理大量的数据。相对于 Oracle 等数据库来说，MySQL 的使用是非常简单的。MySQL 主要目标是快速、健壮和易用。

## 下载安装

### Linux 操作系统的 MySQL 软件包一般分为以下 3 类：

- RPM 软件包
- 二进制软件包
- 源码包

#### RPM

RPM 软件包的安装和卸载都很方便，它的服务器端（Server）软件和客户端（Client）软件都需要分开下载和安装。

#### 二进制软件包

二进制软件包是软件发布的时候已经进行过编译的软件包，安装速度比源码包快得多。

#### 源码包

源码包中是 MySQL 数据库的源代码，需要用户编译成二进制文件后才可以使用。

### Linux CentOS卸载MySQL

#### 使用以下命令查看当前系统中是否安装 MySQL

~~~shell
[root@bogon Desktop]# rpm -qa|grep -i mysql
mysql-libs-5.1.71-1.el6.x86_64
~~~

#### 停止 MySQL 服务，卸载 mysql-libs-5.1.71-1.el6.x86_64

~~~shell
service mysql stop
rpm -e --nodeps mysql-libs-5.1.71-1.el6.x86_64
~~~

注意：如果提示错误，可使用命令 `rpm -ev mysql-libs-5.1.71-1.el6.x86_64 --nodeps` 或 `rpm -e --noscripts mysql-libs-5.1.71-1.el6.x86_64` 卸载 MySQL。

#### 查找之前老版本 MySQL 的目录，并且删除老版本的文件和库。

~~~shell
find / -name mysql
#显示 MySQL 目录后，可以使用以下命令删除目录
rm -rf 目录名
~~~

### 安装

 Centos7 系统下使用 yum 命令安装 MySQL，需要注意的是 CentOS 7 版本中 MySQL数据库已从默认的程序列表中移除，所以在安装前我们需要先去官网下载 Yum 资源包，下载地址为：https://dev.mysql.com/downloads/repo/yum/

~~~shell
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
yum install mysql-server
~~~

#### 权限设置：

~~~shell
chown mysql:mysql -R /var/lib/mysql
~~~

#### 初始化 MySQL：

~~~shell
mysqld --initialize
~~~

#### 启动 MySQL：

~~~shell
systemctl start mysqld
~~~

#### 查看 MySQL 运行状态：

~~~shell
systemctl status mysqld
~~~

#### 登录

~~~~shell
mysql -h 主机名 -u 用户名 -p
~~~~

​											Linux平台MySQL的安装目录

| 文件夹             | 文件夹内容                                               |
| ------------------ | -------------------------------------------------------- |
| /usr/bin           | 客户端和脚本（mysqladmin、mysqldump 等命令）             |
| /usr/sbin          | mysqld 服务器                                            |
| /var/lib/mysql     | 日志文件、socket 文件和数据库                            |
| /usr/share/info    | 信息格式的手册                                           |
| /usr/share/man     | UNIX 帮助页                                              |
| /usr/include/mysql | 头文件                                                   |
| /usr/lib/mysql     | 库                                                       |
| /usr/share/mysql   | 错误消息、字符集、安装文件和配置文件等                   |
| /etc/rc.d/init.d/  | 启动脚本文件的 mysql 目录，可以用来启动和停止 MySQL 服务 |

### MySQL my.cnf配置文件详解

~~~properties
[client]
port=3306
socket=/var/run/mysql/mysql.sock
[mysqldump]
quick
max_allowed_packet = 16M

~~~

以上参数会被 MySQL 客户端应用读取，参数说明如下：

- port：MySQL 客户端连接服务器端时使用的端口号，默认为 3306
- socket：套接字文件所在目录
- quick：支持较大的数据库转储，导出非常巨大的表时需要此项 。
- max_allowed_packet：服务所能处理的请求包的最大大小以及服务所能处理的最大的请求大小（当与大的BLOB字段一起工作时相当必要），每个连接独立的大小，大小动态增加。

~~~properties
[mysqld]

user = mysql
basedir = /usr/local/mysql
datadir = /mydata/mysql/data
port=3306
server-id = 1
socket=/var/run/mysql/mysql.sock
~~~

上述参数说明如下：

- user：mysqld 程序在启动后将在给定 UNIX/Linux 账户下执行。mysqld 必须从 root 账户启动才能在启动后切换到另一个账户下执行。mysqld_safe 脚本将默认使用 user=mysql 选项来启动 mysqld 程序。
- basedir：指定 MySQL 安装的绝对路径；
- datadir：指定 MySQL 数据存放的绝对路径；
- port：服务端口号，默认为 3306
- server-id：MySQL 服务的唯一编号，每个 MySQL 服务的 id 需唯一。
- socket：socket 文件所在目录

~~~properties
character-set-server = utf8mb4
collation-server = utf8mb4_general_ci
init_connect='SET NAMES utf8mb4'
lower_case_table_names = 1

key_buffer_size=16M
max_allowed_packet=8M
no-auto-rehash
sql_mode=TRADITIONAL
~~~

- character-set-server：数据库默认字符集，主流字符集支持一些特殊表情符号（特殊表情符占用 4 个字节）
- collation-server：数据库字符集对应一些排序等规则，注意要和 character-set-server 对应
- init_connect：设置 client 连接 mysql 时的字符集，防止乱码
- lower_case_table_names：是否对 sql 语句大小写敏感，1 表示不敏感
- key_buffer_size：用于指定索引缓冲区的大小
- max_allowed_packet：设置一次消息传输的最大值
- no-auto-rehash：仅仅允许使用键值的 UPDATES 和 DELETES
- sql_mode：表示 SQL 模式的参数，通过这个参数可以设置检验 SQL 语句的严格程度

### MySQL配置文件（my.ini）详解

~~~properties
[client]
port=3306
[mysql]
default-character-set=gbk
~~~

上面显示的是客户端的参数，[client] 和 [mysql] 都是客户端，参数说明如下：

- port：表示 MySQL 客户端连接服务器端时使用的端口号，默认的端口号为 3306。如果需要更改端口号的话，可以直接在这里修改。
- default-character-set：表示 MySQL 客户端默认的字符集。

~~~properties
[mysqld]

port=3306
basedir=C:/Program Files/MySQL/MySQL Server 5.7/
datadir=C:/ProgramData/MySQL/MySQL Server 5.7/Data

character-set-server=gb2312
default-storage-engine=INNODB
sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"

max_connections=100
query_cache_size=0
table_cache=256
tmp_table_size=35M
thread_cache_size=8
myisam_max_sort_file_size=100G
myisam_sort_buffer_size=69M
key_buffer_size=55M
read_buffer_size=64K
read_rnd_buffer_size=256K
sort_buffer_size=256K
~~~

以上是服务器的参数，参数说明如下表所示：

| 参数名称                  | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| port                      | 表示 MySQL 服务器的端口号                                    |
| basedir                   | 表示 MySQL 的安装路径                                        |
| datadir                   | 表示 MySQL 数据文件的存储位置，也是数据表的存放位置          |
| default-character-set     | 表示服务器端默认的字符集                                     |
| default-storage-engine    | 创建数据表时，默认使用的存储引擎                             |
| sql-mode                  | 表示 SQL 模式的参数，通过这个参数可以设置检验 SQL 语句的严格程度 |
| max_connections           | 表示允许同时访问 MySQL 服务器的最大连接数。其中一个连接是保留的，留给管理员专用的 |
| query_cache_size          | 表示查询时的缓存大小，缓存中可以存储以前通过 SELECT 语句查询过的信息，再次查询时就可以直接从缓存中拿出信息，可以改善查询效率 |
| table_open_cache          | 表示所有进程打开表的总数                                     |
| tmp_table_size            | 表示内存中每个临时表允许的最大大小                           |
| thread_cache_size         | 表示缓存的最大线程数                                         |
| myisam_max_sort_file_size | 表示 MySQL 重建索引时所允许的最大临时文件的大小              |
| myisam_sort_buffer_size   | 表示重建索引时的缓存大小                                     |
| key_buffer_size           | 表示关键词的缓存大小                                         |
| read_buffer_size          | 表示 MyISAM 表全表扫描的缓存大小                             |
| read_rnd_buffer_size      | 表示将排序好的数据存入该缓存中                               |
| sort_buffer_size          | 表示用于排序的缓存大小                                       |

 ~~~~properties
innodb_additional_mem_pool_size=3M
innodb_flush_log_at_trx_commit=1
innodb_log_buffer_size=2M
innodb_buffer_pool_size=107M
innodb_log_file_size=54M
innodb_thread_concurrency=18
 ~~~~

以上是 InnoDB 存储引擎使用的参数，参数说明如下：

- innodb_additional_mem_pool_size：表示附加的内存池，用来存储 InnoDB 表的内容。
- innodb_flush_log_at_trx_commit：是设置提交日志的时机，若设置为 1，InnoDB 会在每次提交后将事务日志写到磁盘上。
- innodb_log_buffer_size：表示用来存储日志数据的缓存区的大小。
- innodb_buffer_pool_size：表示缓存的大小，InnoDB 使用一个缓冲池类保存索引和原始数据。
- innodb_log_file_size：表示日志文件的大小。
- innodb_thread_concurrency：表示在 InnoDB 存储引擎允许的线程最大数。


注意：每次修改 my.ini 文件中的参数后，必须重新启动 MySQL 服务才会有效。

## SQL是什么？它能做什么？

**SQL 是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。**

SQL 具有如下优点。

1. 一体化：SQL 集数据定义、数据操作和数据控制于一体，可以完成数据库中的全部工作。
2. 使用方式灵活：SQL 具有两种使用方式，可以直接以命令方式交互使用；也可以嵌入使用，嵌入C、C++、Fortran、COBOL、Java 等语言中使用。
3. 非过程化：只提操作要求，不必描述操作步骤，也不需要导航。使用时只需要告诉计算机“做什么”，而不需要告诉它“怎么做”，存储路径的选择和操作的执行由数据库管理系统自动完成。
4. 语言简洁、语法简单：该语言的语句都是由描述性很强的英语单词组成，而且这些单词的数目不多。

### SQL 包含以下 4 部分：

#### 数据定义语言（Data Definition Language，DDL）

用来创建或删除数据库以及表等对象，主要包含以下几种命令：

- DROP：删除数据库和表等对象
- CREATE：创建数据库和表等对象
- ALTER：修改数据库和表等对象的结构

#### 数据操作语言（Data Manipulation Language，DML）

用来变更表中的记录，主要包含以下几种命令：

- SELECT：查询表中的数据
- INSERT：向表中插入新数据
- UPDATE：更新表中的数据
- DELETE：删除表中的数据

#### 数据查询语言（Data Query Language，DQL）

用来查询表中的记录，主要包含 SELECT 命令，来查询表中的数据。

#### 数据控制语言（Data Control Language，DCL）

用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对数据库中的用户设定权限。主要包含以下几种命令：

- GRANT：赋予用户操作权限
- REVOKE：取消用户的操作权限
- COMMIT：确认对数据库中的数据进行的变更
- ROLLBACK：取消对数据库中的数据进行的变更

## MySQL数据类型

数据类型（data_type）是指系统中所允许的数据的类型。

###  数值类型

整数类型包括 TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT，浮点数类型包括 FLOAT 和 DOUBLE，定点数类型为 DECIMAL。

| 类型名称      | 说明                                      | 存储需求                | 字节 |
| ------------- | ----------------------------------------- | ----------------------- | ---- |
| TINYINT       | -128〜127                                 | 0 〜255                 | 1    |
| SMALLINT      | -32768〜32767                             | 0〜65535                | 2    |
| MEDIUMINT     | -8388608〜8388607                         | 0〜16777215             | 3    |
| INT (INTEGER) | -2147483648〜2147483647                   | 0〜4294967295           | 4    |
| BIGINT        | -9223372036854775808〜9223372036854775807 | 0〜18446744073709551615 | 8    |



| 类型名称            | 说明               | 存储需求   |
| ------------------- | ------------------ | ---------- |
| FLOAT               | 单精度浮点数       | 4 个字节   |
| DOUBLE              | 双精度浮点数       | 8 个字节   |
| DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

DECIMAL 类型不同于 FLOAT 和 DOUBLE。DOUBLE 实际上是以字符串的形式存放的，DECIMAL 可能的最大取值范围与 DOUBLE 相同，但是有效的取值范围由 M 和 D 决定。如果改变 M 而固定 D，则取值范围将随 M 的变大而变大。

### 日期/时间类型

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

### 字符串类型

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

### 二进制类型

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

## MySQL存储引擎有哪些？

数据库存储引擎是数据库底层软件组件，数据库管理系统使用数据引擎进行创建、查询、更新和删除数据操作。

| 存储引擎  | 文件                     | 描述                                                         |
| --------- | ------------------------ | ------------------------------------------------------------ |
| ARCHIVE   | .ARZ .frm                | 用于数据存档的引擎，数据被插入后就不能在修改了，且不支持索引。 |
| CSV       | .CSM                     | 在存储数据时，会以逗号作为数据项之间的分隔符。               |
| BLACKHOLE | .frm                     | 会丢弃写操作，该操作会返回空内容。                           |
| FEDERATED |                          | 将数据存储在远程数据库中，用来访问远程表的存储引擎。         |
| InnoDB    | .frm<br />.ibd           | 具备外键支持功能的事务处理引擎                               |
| MEMORY    | .frm                     | 置于内存的表                                                 |
| MERGE     | .frm<br />.mrg           | 用来管理由多个 MyISAM 表构成的表集合                         |
| MyISAM    | .frm<br />.MYD<br />.MYI | 主要的非事务处理存储引擎                                     |
| NDB       |                          | MySQL 集群专用存储引擎                                       |

### MySQL存储引擎特性汇总和对比

| 特性         | MyISAM | InnoDB | MEMORY |
| ------------ | ------ | ------ | ------ |
| 存储限制     | 有     | 支持   | 有     |
| 事务安全     | 不支持 | 支持   | 不支持 |
| 锁机制       | 表锁   | 行锁   | 表锁   |
| B树索引      | 支持   | 支持   | 支持   |
| 哈希索引     | 不支持 | 不支持 | 支持   |
| 全文索引     | 支持   | 不支持 | 不支持 |
| 集群索引     | 不支持 | 支持   | 不支持 |
| 数据缓存     |        | 支持   | 支持   |
| 索引缓存     | 支持   | 支持   | 支持   |
| 数据可压缩   | 支持   | 不支持 | 不支持 |
| 空间使用     | 低     | 高     | N/A    |
| 内存使用     | 低     | 高     | 中等   |
| 批量插入速度 | 高     | 低     | 高     |
| 支持外键     | 不支持 | 支持   | 不支持 |

## MySQL索引（Index）

索引是一种特殊的数据库结构，由数据表中的一列或多列组合而成，可以用来快速查询数据表中有某一特定值的记录。

通过索引，查询数据时不用读完记录的所有信息，而只是查询索引列。否则，数据库系统将读取每条记录的所有信息进行匹配。

### 为什么要使用索引

索引就是根据表中的一列或若干列按照一定顺序建立的列值与记录行之间的对应关系表，实质上是一张描述索引列的列值与原表中记录行之间一 一对应关系的有序表。

索引是 MySQL 中十分重要的数据库对象，是数据库性能调优技术的基础，常用于实现数据的快速检索。

在 MySQL 中，通常有以下两种方式访问数据库表的行数据：

####  顺序访问

顺序访问是在表中实行全表扫描，从头到尾逐行遍历，直到在无序的行数据中找到符合条件的目标数据。

顺序访问实现比较简单，但是当表中有大量数据的时候，效率非常低下。例如，在几千万条数据中查找少量的数据时，使用顺序访问方式将会遍历所有的数据，花费大量的时间，显然会影响数据库的处理性能。

#### 索引访问

索引访问是通过遍历索引来直接访问表中记录行的方式。

使用这种方式的前提是对表建立一个索引，在列上创建了索引之后，查找数据时可以直接根据该列上的索引找到对应记录行的位置，从而快捷地查找到数据。索引存储了指定列数据值的指针，根据指定的排序顺序对这些指针排序。

### 索引的优缺点

索引有其明显的优势，也有其不可避免的缺点。

#### 优点

索引的优点如下：

- 通过创建唯一索引可以保证数据库表中每一行数据的唯一性。
- 可以给所有的 MySQL 列类型设置索引。
- 可以大大加快数据的查询速度，这是使用索引最主要的原因。
- 在实现数据的参考完整性方面可以加速表与表之间的连接。
- 在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间

#### 缺点

增加索引也有许多不利的方面，主要如下：

- 创建和维护索引组要耗费时间，并且随着数据量的增加所耗费的时间也会增加。
- 索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间。如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。
- 当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，这样就降低了数据的维护速度。


使用索引时，需要综合考虑索引的优点和缺点。

索引可以提高查询速度，但是会影响插入记录的速度。因为，向有索引的表中插入记录时，数据库系统会按照索引进行排序，这样就降低了插入记录的速度，插入大量记录时的速度影响会更加明显。这种情况下，最好的办法是先删除表中的索引，然后插入数据，插入完成后，再创建索引。

### 索引类型详解

索引的类型和存储引擎有关，每种存储引擎所支持的索引类型不一定完全相同。MySQL 索引可以从存储方式、逻辑角度和实际使用的角度来进行分类。

#### 存储方式区分

根据存储方式的不同，MySQL 中常用的索引在物理上分为 B-树索引和 HASH 索引两类，两种不同类型的索引各有其不同的适用范围。

##### B-树索引

B-树索引又称为 BTREE 索引，目前大部分的索引都是采用 B-树索引来存储的。

B-树索引是一个典型的数据结构，其包含的组件主要有以下几个：

- 叶子节点：包含的条目直接指向表里的数据行。叶子节点之间彼此相连，一个叶子节点有一个指向下一个叶子节点的指针。
- 分支节点：包含的条目指向索引里其他的分支节点或者叶子节点。
- 根节点：一个 B-树索引只有一个根节点，实际上就是位于树的最顶端的分支节点。


基于这种树形数据结构，表中的每一行都会在索引上有一个对应值。因此，在表中进行数据查询时，可以根据索引值一步一步定位到数据所在的行。

B-树索引可以进行全键值、键值范围和键值前缀查询，也可以对查询结果进行 ORDER BY 排序。但 B-树索引必须遵循左边前缀原则，要考虑以下几点约束：

- 查询必须从索引的最左边的列开始。
- 查询不能跳过某一索引列，必须按照从左到右的顺序进行匹配。
- 存储引擎不能使用索引中范围条件右边的列。

##### 哈希索引

哈希（Hash）一般翻译为“散列”，也有直接音译成“哈希”的，就是把任意长度的输入（又叫作预映射，pre-image）通过散列算法变换成固定长度的输出，该输出就是散列值。

哈希索引也称为散列索引或 HASH 索引。MySQL 目前仅有 MEMORY 存储引擎和 HEAP 存储引擎支持这类索引。其中，MEMORY 存储引擎可以支持 B-树索引和 HASH 索引，且将 HASH 当成默认索引。

HASH 索引不是基于树形的数据结构查找数据，而是根据索引列对应的哈希值的方法获取表的记录行。哈希索引的最大特点是访问速度快，但也存在下面的一些缺点：

- MySQL 需要读取表中索引列的值来参与散列计算，散列计算是一个比较耗时的操作。也就是说，相对于 B-树索引来说，建立哈希索引会耗费更多的时间。
- 不能使用 HASH 索引排序。
- HASH 索引只支持等值比较，如“=”“IN()”或“<=>”。
- HASH 索引不支持键的部分匹配，因为在计算 HASH 值的时候是通过整个索引值来计算的。

#### 逻辑区分

##### 普通索引

普通索引是 MySQL 中最基本的索引类型，它没有任何限制，唯一任务就是加快系统对数据的访问速度。

普通索引允许在定义索引的列中插入重复值和空值。

创建普通索引时，通常使用的关键字是 INDEX 或 KEY。

##### 唯一索引

唯一索引与普通索引类似，不同的是创建唯一性索引的目的不是为了提高访问速度，而是为了避免数据出现重复。

唯一索引列的值必须唯一，允许有空值。如果是组合索引，则列值的组合必须唯一。

创建唯一索引通常使用 UNIQUE 关键字。

##### 主键索引

主键索引就是专门为主键字段创建的索引，也属于索引的一种。

主键索引是一种特殊的唯一索引，不允许值重复或者值为空。

创建主键索引通常使用 PRIMARY KEY 关键字。不能使用 CREATE INDEX 语句创建主键索引。

##### 空间索引

空间索引是对空间数据类型的字段建立的索引，使用 SPATIAL 关键字进行扩展。

创建空间索引的列必须将其声明为 NOT NULL，空间索引只能在存储引擎为 MyISAM 的表中创建。

空间索引主要用于地理空间数据类型 GEOMETRY。

##### 全文索引

全文索引主要用来查找文本中的关键字，只能在 CHAR、VARCHAR 或 TEXT 类型的列上创建。在 MySQL 中只有 MyISAM 存储引擎支持全文索引。

全文索引允许在索引列中插入重复值和空值。

不过对于大容量的数据表，生成全文索引非常消耗时间和硬盘空间。

创建全文索引使用 FULLTEXT 关键字。

#### 实际使用区分

##### 单列索引

单列索引就是索引只包含原表的一个列。在表中的单个字段上创建索引，单列索引只根据该字段进行索引。

单列索引可以是普通索引，也可以是唯一性索引，还可以是全文索引。只要保证该索引只对应一个字段即可

##### 多列索引

组合索引也称为复合索引或多列索引。相对于单列索引来说，组合索引是将原表的多个列共同组成一个索引。多列索引是在表的多个字段上创建一个索引。该索引指向创建时对应的多个字段，可以通过这几个字段进行查询。但是，只有查询条件中使用了这些字段中第一个字段时，索引才会被使用。

`提示：一个表可以有多个单列索引，但这些索引不是组合索引。一个组合索引实质上为表的查询提供了多个索引，以此来加快查询速度。比如，在一个表中创建了一个组合索引(c1，c2，c3)，在实际查询中，系统用来实际加速的索引有三个：单个索引(c1)、双列索引(c1，c2)和多列索引(c1，c2，c3)。`

## 数据库事务

数据库的**事务（Transaction）**是一种机制、一个操作序列，包含了一组数据库操作命令。事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么都执行，要么都不执行，因此事务是一个不可分割的工作逻辑单元。

### 特性

事务具有 4 个特性，即原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability），这 4 个特性通常简称为 ACID。

#### 原子性

事务是一个完整的操作。事务的各元素是不可分的（原子的）。事务中的所有元素必须作为一个整体提交或回滚。如果事务中的任何元素失败，则整个事务将失败。

#### 一致性

当事务完成时，数据必须处于一致状态。也就是说，在事务开始之前，数据库中存储的数据处于一致状态。在正在进行的事务中. 数据可能处于不一致的状态，如数据可能有部分被修改。然而，当事务成功完成时，数据必须再次回到已知的一致状态。通过事务对数据所做的修改不能损坏数据，或者说事务不能使数据存储处于不稳定的状态。

#### 隔离性

对数据进行修改的所有并发事务是彼此隔离的，这表明事务必须是独立的，它不应以任何方式依赖于或影响其他事务。修改数据的事务可以在另一个使用相同数据的事务开始之前访问这些数据，或者在另一个使用相同数据的事务结束之后访问这些数据。

#### 持久性

事务的持久性指不管系统是否发生了故障，事务处理的结果都是永久的。

一个事务成功完成之后，它对数据库所作的改变是永久性的，即使系统出现故障也是如此。也就是说，一旦事务被提交，事务对数据所做的任何变动都会被永久地保留在数据库中。

### MySQL执行事务的语法和流程

MySQL 提供了多种存储引擎来支持事务。支持事务的存储引擎有 InnoDB 和 BDB，其中，InnoDB 存储引擎事务主要通过 UNDO 日志和 REDO 日志实现，MyISAM 存储引擎不支持事务。

拓展：任何一种数据库，都会拥有各种各样的日志，用来记录数据库的运行情况、日常操作、错误信息等，MySQL 也不例外。例如，当用户 root 登录到 MySQL 服务器，就会在日志文件里记录该用户的登录时间、执行操作等。

为了维护 MySQL 服务器，经常需要在 MySQL 数据库中进行日志操作：

- **UNDO 日志**：复制事务执行前的数据，用于在事务发生异常时回滚数据。
- **REDO 日志**：记录在事务执行中，每条对数据进行更新的操作，当事务提交时，该内容将被刷新到磁盘。


默认设置下，每条 SQL 语句就是一个事务，即执行 SQL 语句后自动提交。为了达到将几个操作做为一个整体的目的，需要使用 BEGIN 或 START TRANSACTION 开启一个事务，或者禁止当前会话的自动提交。

#### 开始事务

~~~sql
BEGIN;
#或者
START TRANSACTION;
~~~

#### 提交事务

~~~sql
COMMIT;
~~~

COMMIT 表示提交事务，即提交事务的所有操作，具体地说，就是将事务中所有对数据库的更新都写到磁盘上的物理数据库中，事务正常结束。

提交事务，意味着将事务开始以来所执行的所有数据都修改成为数据库的永久部分，因此也标志着一个事务的结束。一旦执行了该命令，将不能回滚事务。只有在所有修改都准备好提交给数据库时，才执行这一操作。

#### 回滚（撤销）事务

~~~sql
ROLLBACK;
~~~

ROLLBACK 表示撤销事务，即在事务运行的过程中发生了某种故障，事务不能继续执行，系统将事务中对数据库的所有已完成的操作全部撤销，回滚到事务开始时的状态。这里的操作指对数据库的更新操作。

当事务执行过程中遇到错误时，使用 ROLLBACK 语句使事务回滚到起点或指定的保持点处。同时，系统将清除自事务起点或到某个保存点所做的所有的数据修改，并且释放由事务控制的资源。因此，这条语句也标志着事务的结束。

#### 总结

BEGIN 或 START TRANSACTION 语句后面的 SQL 语句对数据库数据的更新操作都将记录在事务日志中，直至遇到 ROLLBACK 语句或 COMMIT 语句。如果事务中某一操作失败且执行了 ROLLBACK 语句，那么在开启事务语句之后所有更新的数据都能回滚到事务开始前的状态。如果事务中的所有操作都全部正确完成，并且使用了 COMMIT 语句向数据库提交更新数据，则此时的数据又处在新的一致状态。

## MySQL日志

任何一种数据库，都会拥有各种各样的日志，用来记录数据库的运行情况、日常操作和错误等信息，可以帮助我们诊断数据库出现的各种问题。

### 错误日志（Error Log）

该日志文件会记录 MySQL 服务器的启动、关闭和运行错误等信息。



~~~~mysql
mysql> show variables like 'log_error';
+---------------+---------------------+
| Variable_name | Value               |
+---------------+---------------------+
| log_error     | /var/log/mysqld.log |
+---------------+---------------------+
1 row in set (0.03 sec)
~~~~

### 慢查询日志（Slow Query Log）

记录执行事件超过指定时间的操作，通过工具分析慢查询日志可以定位 MySQL 服务器性能瓶颈所在。

慢查询日志用来记录响应时间超过阈值的SQL语句，所以我们可以设置一个阈值，将运行时间超过该值的所有SQL语句都记录到慢查询日志文件中。该阈值可以通过参数 `long_query_time` 来设置，默认为10秒。

#### 启动慢查询日志

**默认情况下，MySQL数据库并不启动慢查询日志，需要手动将这个参数设为ON，然后启动**

~~~mysql
mysql> show variables like "%slow%";
+---------------------------+-------------------------------------------------+
| Variable_name             | Value                                           |
+---------------------------+-------------------------------------------------+
| log_slow_admin_statements | OFF                                             |
| log_slow_slave_statements | OFF                                             |
| slow_launch_time          | 2                                               |
| slow_query_log            | OFF                                             |
| slow_query_log_file       | /var/lib/mysql/iz2zeaf3cg1099kiidi06mz-slow.log |
+---------------------------+-------------------------------------------------+
5 rows in set (0.00 sec)

mysql> set global slow_query_log='ON';
Query OK, 0 rows affected (0.00 sec)


mysql> show variables like "slow_query_log";
+---------------------------+-------------------------------------------------+
| Variable_name             | Value                                           |
+---------------------------+-------------------------------------------------+                                        |
| slow_query_log            | ON                                              |
| slow_query_log_file       | /var/lib/mysql/iz2zeaf3cg1099kiidi06mz-slow.log |
+---------------------------+-------------------------------------------------+
2   rows in set (0.00 sec)
~~~

**但是使用 `set global slow_query_log='ON'` 开启慢查询日志，只是对当前数据库有效，如果MySQL数据库重启后就会失效。所以如果要永久生效，就要修改配置文件 `my.cnf` (其他系统变量也是如此)**，如下：

~~~properties
[mysqld]
slow_query_log=1
~~~

然后重启MySQL就可以让慢查询日志记录开启了,至于日志文件的路径就是上面`slow_query_log_file`对应的路径。

#### 设置阈值

~~~mysql
mysql> show variables like 'long_query_time';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set (0.00 sec)

mysql> set global long_query_time=0.05;
Query OK, 0 rows affected (0.00 sec)

~~~

设置`long_query_time`这个阈值之后，MySQL数据库会记录运行时间超过该值的所有SQL语句，但对于运行时间正好等于 `long_query_time` 的情况，并不会被记录下。而设置 `long_query_time`为0来捕获所有的查询

#### 参数log_queries_not_using_indexes

**另一个和慢查询日志有关的参数是 `log_queries_not_using_indexes`**, 如果运行的SQL语句没有使用索引，则MySQL数据库同样会将这条SQL语句记录到慢查询日志文件。首先确认打开了log_queries_not_using_indexes;

~~~mysql
mysql> show variables like 'log_queries_not_using_indexes';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| log_queries_not_using_indexes | ON    |
+-------------------------------+-------+
1 row in set (0.12 sec)

~~~

### 通用查询日志（Genaral Query Log）

该日志记录 MySQL 服务器的启动和关闭信息、客户端的连接信息、更新、查询数据记录的 SQL 语句等。默认为 `主机名.log`

~~~mysql
mysql> show variables like "general_log%";
+------------------+--------------------------------------------+
| Variable_name    | Value                                      |
+------------------+--------------------------------------------+
| general_log      | OFF                                        |
| general_log_file | /var/lib/mysql/iz2zeaf3cg1099kiidi06mz.log |
+------------------+--------------------------------------------+
2 rows in set (0.24 sec)   

#默认为不开启
mysql> set global general_log='ON';
Query OK, 0 rows affected (0.05 sec)
~~~

### 二进制日志（Binary Log）

该日志文件会以二进制的形式记录数据库的各种操作，但不记录查询语句。

此外，二进制还包括了执行数据库更改操作的时间和执行时间等信息。**二进制日志主要有以下几种作用**:

- **恢复(recovery)**： 某些数据的恢复需要二进制日志，如当一个数据库全备文件恢复后，我们可以通过二进制的日志进行 `point-in-time`的恢复
- **复制(replication)** : 通过复制和执行二进制日志使得一台远程的 MySQL 数据库(一般是slave 或者 standby) 与一台MySQL数据库(一般为master或者primary) 进行实时同步
- **审计(audit)**：用户可以通过二进制日志中的信息来进行审计，判断是否有对数据库进行注入攻击

#### 开启二进制日志

通过配置参数 `log-bin[=name]` 可以启动二进制日志。如果不指定name,则默认二进制日志文件名为主机名，后缀名为二进制日志的序列号

~~~mysql
[mysqld]
log-bin
~~~

~~~mysql
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.00 sec)
~~~



mysqld-bin.000001即为二进制日志文件，而mysqld-bin.index为二进制的索引文件，为了管理所有的binlog文件，MySQL额外创建了一个index文件，它按顺序记录了MySQL使用的所有binlog文件。如果你想自定义index文件的名称，可以设置 `log_bin_index=file`参数。

#### 查看二进制日志文件

对于二进制日志文件来说，不像错误日志文件，慢查询日志文件那样用cat，head, tail等命令可以查看，它需要通过 MySQL 提供的工具 mysqlbinlog。如:

~~~mysql
[root@iz2zeaf3cg1099kiidi06mz mysql]# mysqlbinlog mysqld-bin.000001
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!40019 SET @@session.max_insert_delayed_threads=0*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#180821 16:42:53 server id 1  end_log_pos 120 CRC32 0x3e55be40 	Start: binlog v 4, server v 5.6.39-log created 180821 16:42:53 at startup
# Warning: this binlog is either in use or was not closed properly.
ROLLBACK/*!*/;
BINLOG '
jdB7Ww8BAAAAdAAAAHgAAAABAAQANS42LjM5LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAACN0HtbEzgNAAgAEgAEBAQEEgAAXAAEGggAAAAICAgCAAAACgoKGRkAAUC+
VT4=
'/*!*/;
DELIMITER ;
# End of log file
ROLLBACK /* added by mysqlbinlog */;
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
~~~

#### 二进制日志文件配置参数

**max_binlog_size**

可以通过max_binlog_size参数来限定单个binlog文件的大小(默认1G)

**binlog_cache_size**

当使用事务的表存储引擎(如InnoDB存储引擎)时，所有未提交(uncommitted)的二进制日志会被记录到一个缓冲中去，等该事务提交(committed)时，直接将缓存中的二进制日志写入二进制日志文件中，而该缓冲的大小由`binlog_cache_size`决定，默认大小为32K。此外，`binlog_cache_size` 是基于会话(session)的，当每一个线程开启一个事务时，MySQL会自动分配一个大小为 `binlog_cache_size` 的缓存

```mysql
mysql> show variables like 'binlog_cache_size';
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| binlog_cache_size | 32768 |
+-------------------+-------+
1 row in set (0.00 sec)
```

**sync_binlog**

在默认情况下，二进制日志并不是在每次写的时候同步到磁盘。参数 `sync_binlog` = [N] 表示每写缓冲多少次就同步到磁盘。如果将N设置为1，即 `sync_binlog = 1`表示采用同步写磁盘的方式来写二进制日志，这时写操作就不用向上面所说的使用操作系统的缓冲来写二进制日志

**binlog_format**

`binlog_format` 参数十分重要，它影响了记录二进制日志的格式,分为三种格式:

- statement : 记录的是日志的逻辑SQL语句
- row: 记录表的行更改情况
- mixed: 在此格式下，mysql默认采用statement格式进行二进制日志文件的记录，但是有些情况下使用ROW格式，有以下几种情况:

```
表的存储引擎为NDB，这时对表的DML操作都会以ROW格式记录。
使用了UUID()、USER()、CURRENT_USER()、FOUND_ROW()、ROW_COUNT()等不确定函数。
使用了INSERT DELAY语句。
使用了用户定义函数（UDF）。
使用了临时表（temporary table）。
```

### 重做日志（redo log）

#### 作用：确保事务的持久性。

　　防止在发生故障的时间点，尚有脏页未写入磁盘，在重启mysql服务的时候，根据redo log进行重做，从而达到事务的持久性这一特性。

#### 内容：

　　物理格式的日志，记录的是物理数据页面的修改的信息，其redo log是顺序写入redo log file的物理文件中去的。

#### 什么时候产生：

　　事务开始之后就产生redo log，redo log的落盘并不是随着事务的提交才写入的，而是在事务的执行过程中，便开始写入redo log文件中。

#### 什么时候释放：

　　当对应事务的脏页写入到磁盘之后，redo log的使命也就完成了，重做日志占用的空间就可以重用（被覆盖）。

#### 对应的物理文件：

　　默认情况下，对应的物理文件位于数据库的data目录下的`ib_logfile1`和`ib_logfile2`
　　innodb_log_group_home_dir 指定日志文件组所在的路径，默认./ ，表示在数据库的数据目录下。
　　innodb_log_files_in_group 指定重做日志文件组中文件的数量，默认2
　　关于文件的大小和数量，由一下两个参数配置
　　innodb_log_file_size 重做日志文件的大小。
　　innodb_mirrored_log_groups 指定了日志镜像文件组的数量，默认1

#### 其他：

　　很重要一点，redo log是什么时候写盘的？前面说了是在事物开始之后逐步写盘的。
　　之所以说重做日志是在事务开始之后逐步写入重做日志文件，而不一定是事务提交才写入重做日志缓存，
　　原因就是，重做日志有一个缓存区Innodb_log_buffer，Innodb_log_buffer的默认大小为8M(这里设置的16M),Innodb存储引擎先将重做日志写入innodb_log_buffer中。

~~~shell
mysql> show variables like '%innodb_log_buffer_size%';
+-----------------------+-----------------------+
| Variable_name     	| Value 				|
+-----------------------+-----------------------+
|innodb_log_buffer_size | 16777216 				|
+-----------------------+-----------------------+
1 row in set (0.00 sec)
~~~

　　然后会通过以下三种方式将innodb日志缓冲区的日志刷新到磁盘
　　1，Master Thread 每秒一次执行刷新Innodb_log_buffer到重做日志文件。
　　2，每个事务提交时会将重做日志刷新到重做日志文件。
　　3，当重做日志缓存可用空间 少于一半时，重做日志缓存被刷新到重做日志文件
　　由此可以看出，重做日志通过不止一种方式写入到磁盘，尤其是对于第一种方式，Innodb_log_buffer到重做日志文件是Master Thread线程的定时任务。
　　因此重做日志的写盘，并不一定是随着事务的提交才写入重做日志文件的，而是随着事务的开始，逐步开始的。
　　另外引用《MySQL技术内幕 Innodb 存储引擎》（page37）上的原话：
　　即使某个事务还没有提交，Innodb存储引擎仍然每秒会将重做日志缓存刷新到重做日志文件。
　　这一点是必须要知道的，因为这可以很好地解释再大的事务的提交（commit）的时间也是很短暂的。

### 回滚日志（undo log）

#### 作用：

　　保存了事务发生之前的数据的一个版本，可以用于回滚，同时可以提供多版本并发控制下的读（MVCC），也即非锁定读

#### 内容：

　　逻辑格式的日志，在执行undo的时候，仅仅是将数据从逻辑上恢复至事务之前的状态，而不是从物理页面上操作实现的，这一点是不同于redo log的。

#### 什么时候产生：

　　事务开始之前，将当前是的版本生成undo log，undo 也会产生 redo 来保证undo log的可靠性

#### 什么时候释放：

　　当事务提交之后，undo log并不能立马被删除，
　　而是放入待清理的链表，由purge线程判断是否由其他事务在使用undo段中表的上一个事务之前的版本信息，决定是否可以清理undo log的日志空间。

#### 对应的物理文件：

MySQL5.6之前，undo表空间位于共享表空间的回滚段中，共享表空间的默认的名称是ibdata，位于数据文件目录中。
　　MySQL5.6之后，undo表空间可以配置成独立的文件，但是提前需要在配置文件中配置，完成数据库初始化后生效且不可改变undo log文件的个数
　　如果初始化数据库之前没有进行相关配置，那么就无法配置成独立的表空间了。
　　关于MySQL5.7之后的独立undo 表空间配置参数如下
　　innodb_undo_directory = /data/undospace/ --undo独立表空间的存放目录
　　innodb_undo_logs = 128 --回滚段为128KB
　　innodb_undo_tablespaces = 4 --指定有4个undo log文件

　　如果undo使用的共享表空间，这个共享表空间中又不仅仅是存储了undo的信息，共享表空间的默认为与MySQL的数据目录下面，其属性由参数innodb_data_file_path配置。

~~~
mysql> show variables like '%innodb_data_file_path%';
+-----------------------+------------------------+
| Variable_name     	| Value 				 |
+-----------------------+------------------------+
| innodb_data_file_path | ibdata1:12M:autoextend |
+-----------------------+------------------------+
1 row in set (0.00 sec)
~~~

#### 其他：

　　undo是在事务开始之前保存的被修改数据的一个版本，产生undo日志的时候，同样会伴随类似于保护事务持久化机制的redolog的产生。
　　默认情况下undo文件是保持在共享表空间的，也即ibdatafile文件中，当数据库中发生一些大的事务性操作的时候，要生成大量的undo信息，全部保存在共享表空间中的。
　　因此共享表空间可能会变的很大，默认情况下，也就是undo 日志使用共享表空间的时候，被“撑大”的共享表空间是不会也不能自动收缩的。
　　因此，mysql5.7之后的“独立undo 表空间”的配置就显得很有必要了。
## 16.2 MySQL数据库管理系统

### 16.2.1 数据库安装与配置

社区版MySQL Community Edition是免费的，社区版本比较适合中小企业数据库。  

社区版下载地址为：

    https://dev.mysql.com/downloads/windows/installer/5.7.html  

可以选择不同的平台版本，MySQL可以运行在Windows、Linux和UNIX等操作系统上安装和运行。  

### 16.2.2 连接MySQL服务器

连接MySQL服务器命令：

    Mysql –h localhost –u root –p 
    -h ：要连接的服务器主机名或IP地址，可以是远程的一个服务器主机，也可以是-hlocalhost方式没有空格。
    -u ：服务器要验证的用户名，这个用户一定是数据库中存在的，并且具有连接服务器的权限，也可以是-uroot方式没有空格。
    -p ：是与上面用户对应的密码，也可以直接输入密码-p123456，123456是root密码。

所以“mysql –h localhost –u root -p”命令也可以替换为“mysql –hlocalhost –uroot –p123456”。

### 16.2.3 常见的管理命令

1：help  
help：帮助命令

2：退出命令  
exit和quit

3：数据库管理  

show databases：查看数据库列表。  
create database + 要创建的数据库名字：创建数据库  
drop database + 要删除的数据库名字：删除数据库  
use + 数据库名字：进入数据库  

4：数据库表的管理  
show tables：查看数据库中所有的表  
create table +所要创建的表名：创建表  
drop table +所要删除的表名：删除表  
desc +已创建表明：查看表的结构  

例如：  

    create table user(
        Name varchar(20) not null,
        Userid int primary key);

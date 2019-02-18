---
title: >-
  主机无法连接远程数据库，显示：ERROR 2003 (HY000): Can't connect to MySQL server on
  '192.168.0.128' (10060)
date: 2019-02-18 12:59:36
updated: 2019-02-18 12:59:36
tags: mysql
categories: mysql
---

1.网络不通？
检查能不能ping通！
2.防火墙设置？
防火墙是否放过mysql的进程，是否屏蔽了mysql的3306端口！！
3.端口设置？
检查mysql的端口是不是3306！！
4.mysql的账户设置？
mysql账户是否不允许远程连接。如果无法连接可以尝试以下方法：
（1）如果你想root使用密码12345678从任何主机连接到mysql服务器的话：
``` bash
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '12345678' WITH GRANT OPTION; 

mysql>FLUSH PRIVILEGES; 
```
（2）如果你想允许用户root从ip为192.168.1.6的主机连接到mysql服务器，并使用12345678作为密码：
``` bash
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.3' IDENTIFIED BY '12345678' WITH GRANT OPTION; 

mysql>FLUSH PRIVILEGES; 
```
（3）如果你想允许用户root从ip为192.168.1.6的主机连接到mysql服务器的dk数据库，并使用12345678作为密码：
``` bash
mysql>GRANT ALL PRIVILEGES ON dk.* TO 'root'@'192.168.1.3' IDENTIFIED BY '12345678' WITH GRANT OPTION; 

mysql>FLUSH PRIVILEGES; 
```
5.通过修改表来实现远程：
``` bash
mysql>use mysql;
mysql>update user set host = '%' where user = 'root';
```
6.修改my.cnf配置文件：
这个是mysql的配置文件，如果找不到在哪里的话，可以输入find /* -name my.cnf 找到，通过vi/vim编辑该文件，找到 bind-address    = 127.0.0.1 这一句，然后在前面加个#号注释掉，保存退出。（没有这一句就算了）

7.重启服务： 
``` bash
service mysql restart
```
8.在本地远程连接
在命令行或终端输入：
``` bash
mysql -h 服务器ip地址 -P 3306 -u root -p
```
然后输入密码即可。

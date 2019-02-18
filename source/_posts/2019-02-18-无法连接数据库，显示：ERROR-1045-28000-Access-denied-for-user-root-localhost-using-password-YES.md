---
title: >-
  无法连接数据库，显示：ERROR 1045 (28000): Access denied for user 'root'@'localhost'
  (using password: YES)
date: 2019-02-18 14:05:03
updated: 2019-02-18 14:05:03
tags: mysql
categories: mysql
---

1.打开MySQL目录下的my.ini文件，在文件的 [mysqld] 下面添加一行“skip-grant-tables”，保存并关闭文件。（WIN7默认安装my.ini在C:\ProgramData\MySQL\MySQL Server 5.6，Linux则在shell命令中搜索 whereis my ， 可找到my.cnf，再用vi/vim编辑。）
2.重启MySQL服务。
3.通过命令行进入MySQL的BIN目录，输入“mysql -u root -p”(不输入密码)，回车即可进入数据库。（WIN7默认安装，BIN目录为：C:\Program Files\MySQL\MySQL Server 5.6\bin）
4.执行“ use mysql; ”，使用mysql数据库。
5.执行“ update user set password=PASSWORD("123456") where user='root'; ”（修改root的密码）
6.打开MySQL目录下的my.ini文件，删除或注释 [mysqld] 下面的“ skip-grant-tables ”，保存并关闭文件。
7.重启MySQL服务。
8.在命令行中输入“ mysql -u root -p ”，输入密码后即可成功连接数据库。


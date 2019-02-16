---
title: Windows 10 与CentOS 7 安装配置Samba服务器
date: 2019-02-16 20:00:58
updated: 2019-02-16 20:00:58
tags: samba, win10, centos7
categories: Linux
---

搭建和配置samba服务，实现windows与Linux之间的文件共享
1.查看版本：
CentOS系统：
``` bash
# cat /etc/RedHat-release
或者
# rpm -q centos-release
```
Samba服务器：
``` bash
# rpm -qi samba
```
2.安装：
``` bash
# yum -y install samba samba-client
```
3.配置：
进入samba配置目录：
``` bash
# cd /etc/samba/
```

备份smb.conf
``` bash
[root@base samba]# cp smb.conf smb.conf.origin
```

新建smb.conf
``` bash
[root@base samba]# vim smb.conf
```
内容如下，保存并退出

``` bash
[global]
        workgroup = WORKGROUP
        security = user
        passdb backend = tdbsam
        server string = CSJ Samba Server %v
        netbios name = CSJSamba
        map to guest = Bad User

[FileShare]
        comment = share some files
        path = /smb/fileshare
        public = yes
        writeable = yes
        create mask = 0644
        directory mask = 0755

[WebDev]
        comment = project development directory
        path = /smb/webdev
        valid users = ted
        write list = ted
        printable = no
        create mask = 0644
        directory mask = 0755
```
### 【*注意*】
如果只是为个人开发，可以把系统的整个 / 目录都共享出来：
``` bash
[global]
        workgroup = WORKGROUP
        security = user
        passdb backend = tdbsam
        server string = CSJ Samba Server %v
        netbios name = CSJSamba
        map to guest = Bad User

[root]
        comment = share whole files
        path = /
#       public = Yes
        write list = root
        valid users = root
        writeable = Yes
        create mask = 0644
        browseable = Yes
        directory mask = 0755

# wwwroot这一节点可以不需要
[wwwroot]
        comment = Project dev directory
        path = /
        write list = root
        valid users = root
        writeable = Yes
        printable = No
        create mask = 0664
        directory mask = 0775

```

注释：
workgroup 项应与 Windows 主机保持一致，这里是WORKGROUP
security、map to guest项设置为允许匿名用户访问
再下面有两个section，实际为两个目录，section名就是目录名（映射到Windows上可以看见）。
第一个目录名是FileShare，匿名、公开、可写
第二个目录名是WebDev，限定ted用户访问
默认文件属性644/755（不然的话，Windows上在这个目录下新建的文件会有“可执行”属性）

创建用户
``` bash
[root@base samba]# groupadd co3
[root@base samba]# useradd ted -g co3 -s /sbin/nologin
[root@base samba]# smbpasswd -a ted
New SMB password:
Retype new SMB password:
Added user ted.
[root@base samba]# 
注意这里smbpasswd将使用系统用户，设置密码
```
### 【*注意*】
若只是个人开发，可为samba设置用户为root，即
``` bash
[root@base samba]# useradd root -g root -s /sbin/nologin
[root@base samba]# smbpasswd -a root
New SMB password:
Retype new SMB password:
Added user root.
[root@base samba]# 
```

4.创建共享目录
``` bash
[root@base samba]# mkdir -p /smb/{fileshare,webdev}
[root@base samba]# chown nobody:nobody /smb/fileshare/
[root@base samba]# chown ted:co3 /smb/webdev/
```
注意设置属性，不然访问不了。
### 【*注意*】
同样接上，若是个人开发，可不需要mkdir，
``` bash
[root@base samba]# chown nobody:nobody /
[root@base samba]# chown root:root /
```

启动Samba服务，设置开机启动
``` bash
[root@base samba]# systemctl start smb
[root@base samba]# systemctl enable smb
Created symlink from /etc/systemd/system/multi-user.target.wants/smb.service to /usr/lib/systemd/system/smb.service.
[root@base samba]# 
```
开放端口
``` bash
[root@base samba]# firewall-cmd --permanent --add-port=139/tcp
success
[root@base samba]# firewall-cmd --permanent --add-port=445/tcp
success
[root@base samba]# systemctl restart firewalld
[root@base samba]# 
```
或者直接把防火墙关了也行。

5.测试使用
可以使用testparm测试samba配置是否正确
``` bash
[root@base samba]# testparm 
Load smb config files from /etc/samba/smb.conf
rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
Processing section "[FileShare]"
Processing section "[WebDev]"
Loaded services file OK.
Server role: ROLE_STANDALONE

Press enter to see a dump of your service definitions

# Global parameters
[global]
    netbios name = TEDSAMBA
    server string = Ted Samba Server %v
    map to guest = Bad User
    security = USER
    idmap config * : backend = tdb

[FileShare]
    comment = share some files
    path = /smb/fileshare
    guest ok = Yes
    read only = No

[WebDev]
    comment = project development directory
    path = /smb/webdev
    create mask = 0644

    valid users = ted
    write list = ted
[root@base samba]# 
```
root用户的话，不用密码回车即可直接查看samba服务器情况
``` bash
[root@base samba]# smbclient -L localhost 
Enter root's password: 
```
6.在win10访问
打开【库】或【此电脑】，添加【映射网络驱动器】，随便指定一个驱动器名，
文件夹填：\\\192.168.0.128\WebDev （此处的 WebDev 是上面配置的方括号的名称），
勾选【使用其他凭据连接】，【完成】后输入用户名和密码，【确定】。
### 【*注意*】
文件夹填：\\\192.168.0.128\root

转载自https://www.linuxidc.com/Linux/2017-03/141390.htm


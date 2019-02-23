---
title: CENTOS7安装各种桌面，CENTOS安装桌面图形化GUI GNOME/KDE/Cinnamon/MATE/Xfce
date: 2019-02-23 19:06:52
updated: 2019-02-23 19:06:52
tags: centos7
categories: Linux
---


如果你的CENTOS是最小化安装的那默认都是不带X WINDOWS的，所以在安装这些桌面之前得先安装一下X WINDOWS，这个控制功能。
``` bash
yum upgrade
yum install yum-axelget
yum -y groupinstall "X Window System" "Fonts"
```
检查一下我们已经安装的软件以及可以安装的软件，用命令:
``` bash
yum grouplist <回车>
```
然后安装我们需要的图形界面软件，比如GNOME(GNOME Desktop)
这里需要特别注意！！！！
一定要注意，名称必须对应，不同版本的centOS的软件名可能不同，其他Linux系统类似，
否则会出现No packages in any requested group available to install or update 的错误。

1.安装cinnamon桌面
``` bash
 yum -y groupinstall "X Window system" "Fonts"
 yum -y install lightdm
 yum -y install cinnamon
 ```
2.安装终端软件
安装完成后会发现没有终端软件，执行下面的指令，安装就可以了
``` bash
yum install gnome-terminal
```
3.启动桌面
``` bash
systemctl isolate graphical.target
```
4.用startx命令启动（可选）
执行下面的指令，生成.xinitrc
``` bash
echo "systemctl isolate graphical.target" >> ~/.xinitrc
startx
```
5.设置开机启动桌面（可选）
``` bash
systemctl set-default graphical.target
rm '/etc/systemd/system/default.target'
ln -s '/usr/lib/systemd/system/graphical.target' '/etc/systemd/system/default.target'
```
6.安装windows字体
``` bash
yum -y groupinstall "Fonts"
```
或者：
把windows下的一些字体使用tftp复制到了~/myfonts，这个目录要自己建（#mkdir ~/myfonts） 
进行/usr/share/fonts目录，建立~/myfonts目录软连接，
``` bash
cd /usr/share/fonts
ln -s ~/fonts myfonts
mk fontscale
mk fontdir
fc-cache
```


来源参考：
https://www.bnxb.com/linuxserver/27457.html
https://blog.csdn.net/jameshadoop/article/details/54881893
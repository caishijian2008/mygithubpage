---
title: Linux(CentOS 7)命令行模式安装VMware Tools 详解
date: 2019-02-23 18:59:57
updated: 2019-02-23 18:59:57
tags: centos7
categories: Linux
---

1.首先启动CentOS 7,在VMware中点击上方“VM”，点击“Install VMware Tools...”（如已安装则显示“Reinstall VMware Tools...”）。

2.在命令行输入“ls /dev”查看。

3.输入“mkdir /mnt/cdrom”在/mnt目录下新建一个名为cdrom的文件夹。

4.输入“mount -t iso9660 /dev/cdrom /mnt/cdrom”将光盘挂载到/mnt/cdrom目录下。

5.输入“ls /mnt/cdrom/”查看内容，输入“cp /mnt/cdrom/VMwareTools-9.2.0-799703.tar.gz /root/vm.tar.gz”,将名为“VMwareTools-9.2.0-799703.tar.gz”复制到/root目录下，并重新命名为vm.tar.gz。

6.在根目录下输入“ls”查看文件，输入“tar -xzf vm.tar.gz”将文件解压，输入“ls”查看文件，可发现新增目录“vmware-tools-distrib”。

7.输入“cd vmware-tools-distrib/”进入名为“vmware-tools-distrib”的目录，输入“./vmware-install.pl”尝试安装，出现错误“-bash: ./vmware-install.pl: /usr/bin/per: bad interpreter: No such file or directory”，表明未安装编译环境。

8.输入“yum -y install perl gcc make kernel-headers kernel-devel”开始安装。

9.提示已经安装完毕。

10.在“vmware-tools-distrib”目录下重新输入“./vmware-install.pl”开始安装，基本上按回车键即可。

11.VMware Tools已经安装完毕，提示可以运行“/usr/bin/vmware-uninstall-tools.pl”命令卸载VMware Tools。第一次运行时需运行“/usr/bin/vmware-config-tools.pl”命令配置VMware Tools，按回车键直接运行。

12.提示已经安装完毕，可以开始使用。

13.输入"/usr/bin/vmware-user"启动vmware用户进程，并输入”startx“启动图形界面。

14.选择文件（本例中为"测试文档.docx"）并摁住鼠标左键不放，尝试拖动文件到虚拟机。

15.放开鼠标左键，发现文件已经复制到虚拟机。

16.如需卸载VMware Tools，输入“/usr/bin/vmware-uninstall-tools.pl”即可。

17.如需在Gnome GUI图形界面下安装，则只需将文件解压，然后再文件夹里点鼠标右键，选择“Open in Terminal”,在Terminal里面输入“./vmware-install.pl”即可。

安装故障

18.如安装时出现类似下图错误，提示无法删除open-vm-tools，则可能是因为上次安装失败造成。

19.如尝试输入“/usr/bin/vmware-uninstall-tools.pl”仍无法卸载，则输入“rpm -e open-vm-tools-desktop”卸载并重新安装。


来源于：https://www.linuxidc.com/Linux/2017-05/143323.htm

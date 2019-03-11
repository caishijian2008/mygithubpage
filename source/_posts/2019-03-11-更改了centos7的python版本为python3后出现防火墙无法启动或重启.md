---
title: 更改了centos7的python版本为python3后出现防火墙无法启动或重启
date: 2019-03-11 19:48:57
updated: 2019-03-11 19:48:57
tags: python
categories: python
---

更改了centos7的python版本为python3后出现防火墙无法启动或重启，报错如下：
``` bash
[root@localhost ~]# systemctl restart firewalld.service
Job for firewalld.service failed because the control process exited with error code. See "systemctl status firewalld.service" and "journalctl -xe" for details.
[root@localhost ~]# journalctl -xe
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit firewalld.service has begun shutting down.
8月 10 17:27:03 localhost.localdomain kernel: Ebtables v2.0 unregistered
8月 10 17:27:03 localhost.localdomain systemd[1]: Starting firewalld - dynamic f
-- Subject: Unit firewalld.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit firewalld.service has begun starting up.
8月 10 17:27:03 localhost.localdomain systemd[1]: firewalld.service: main proces
8月 10 17:27:03 localhost.localdomain systemd[1]: Failed to start firewalld - dy
-- Subject: Unit firewalld.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit firewalld.service has failed.
--
-- The result is failed.
8月 10 17:27:03 localhost.localdomain systemd[1]: Unit firewalld.service entered
8月 10 17:27:03 localhost.localdomain systemd[1]: firewalld.service failed.
8月 10 17:27:03 localhost.localdomain polkitd[650]: Unregistered Authentication
lines 2533-2555/2555 (END)
```

解决方法：
使用 #vi /usr/sbin/firewalld 查看，并把最开头的 /usr/bin/python 改为 /usr/bin/python2 ，保存即可

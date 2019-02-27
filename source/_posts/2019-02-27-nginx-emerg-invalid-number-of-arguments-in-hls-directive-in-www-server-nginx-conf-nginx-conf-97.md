---
title: >-
  nginx: [emerg] invalid number of arguments in hls directive in
  /www/server/nginx/conf/nginx.conf:97
date: 2019-02-27 15:36:14
updated: 2019-02-27 15:36:14
tags: nginx+hls
categories: nginx
---

nginx报错：
nginx: [emerg] invalid number of arguments in "hls" directive in /www/server/nginx/conf/nginx.conf:97

解决：
1.命名文件夹时一定要注意这两个雷区：中文和空格。
2.仔细查看添加的参数的key或者value是否有空格！

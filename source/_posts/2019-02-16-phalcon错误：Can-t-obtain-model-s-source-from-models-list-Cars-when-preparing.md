---
title: 'phalcon错误：Can''t obtain model''s source from models list: ''Cars'', when preparing'
date: 2019-02-16 19:12:38
updated: 2019-02-16 19:12:38
tags: phalcon
categories: php
---

php使用phalcon查询数据时出现错误如下：
Can't obtain model's source from models list: 'Cars', when preparing: select * from cars where name = :name:
源码：
``` bash
$query = $this->modelsManager->createQuery("select * from cars where name = :name:");
$cars = $query->execute(["name" => "Audi001"]); 
echo json_encode($cars);
```

解决方法：修改sql语句！
``` bash
方法1：
createQuery("select * from Cars where name = :name:");
方法2：
createQuery("select * from cars where cars.name = :name:");
```
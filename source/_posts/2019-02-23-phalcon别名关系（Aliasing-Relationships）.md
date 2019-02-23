---
title: phalcon别名关系（Aliasing Relationships）
date: 2019-02-23 18:53:56
updated: 2019-02-23 18:53:56
tags: phalcon
categories: php
---

当一个数据库表是具有关联数组型的表（即级联型，省市区型，二级分类、三级分类等），或者做内联查询时要起别名。
https://docs.phalconphp.com/zh/latest/db-models-relationships#relationships-between-models

例如，有表如下：
``` sql
mysql> desc robots_similar;
+-------------------+------------------+------+-----+---------+----------------+
| Field             | Type             | Null | Key | Default | Extra          |
+-------------------+------------------+------+-----+---------+----------------+
| id                | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| robots_id         | int(10) unsigned | NO   | MUL | NULL    |                |
| similar_robots_id | int(10) unsigned | NO   |     | NULL    |                |
+-------------------+------------------+------+-----+---------+----------------+
```
其中，robots_id和similar_robots_id都与模型Robots是有关系的，
映射此表及其关系的模型如下所示：
``` php
<?php
class RobotsSimilar extends Phalcon\Mvc\Model {
    public function initialize() {
        $this->belongsTo(
            'robots_id',
            'Store\Toys\Robots',
            'id'
        );
        $this->belongsTo(
            'similar_robots_id',
            'Store\Toys\Robots',
            'id'
        );
    }
}
```

由于两种关系都指向同一模型（Robots），因此获得与关系相关的记录无法清楚：

``` php
<?php
$robotsSimilar = RobotsSimilar::findFirst();
// 返回基于列的相关记录（robots_id）
// 同样作为belongsTo它只返回一条记录
// 但是名称'getRobots'似乎意味着返回多个
$robot = $robotsSimilar->getRobots();
// 但是，如果两个关系具有相同的名称，如何根据列（similar_robots_id）获取相关记录？
```

别名允许我们重命名这两个关系来解决这些问题：

``` php
<?php
use Phalcon\Mvc\Model;
class RobotsSimilar extends Model {
    public function initialize() {
        $this->belongsTo(
            'robots_id',
            'Store\Toys\Robots',
            'id',
            [ 'alias' => 'Robot', ]
        );
        $this->belongsTo(
            'similar_robots_id',
            'Store\Toys\Robots',
            'id',
            [ 'alias' => 'SimilarRobot', ]
        );
    }
}
```

通过别名，我们可以轻松获得相关记录。您还可以使用getRelated()方法使用别名来访问关系:

``` php
<?php
$robotsSimilar = RobotsSimilar::findFirst();

// 返回基于列的相关记录（robots_id）
$robot = $robotsSimilar->getRobot();
$robot = $robotsSimilar->robot;
$robot = $robotsSimilar->getRelated('Robot');

// 返回基于列的相关记录（similar_robots_id）
$similarRobot = $robotsSimilar->getSimilarRobot();
$similarRobot = $robotsSimilar->similarRobot;
$similarRobot = $robotsSimilar->getRelated('SimilarRobot');
```

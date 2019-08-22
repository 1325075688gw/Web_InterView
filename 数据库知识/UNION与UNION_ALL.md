### MySQL UNION 操作符

#### 描述

MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。(union叫做联合查询)

```mysql
SELECT ...
UNION [ALL | DISTINCT]
SELECT ...
[UNION [ALL | DISTINCT]
SELECT ...]
```

**DISTINCT:** 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。

##### 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```http
mysql> SELECT * FROM Websites;
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "apps" APP 的数据：

```http
mysql> SELECT * FROM apps;
+----+------------+-------------------------+---------+
| id | app_name   | url                     | country |
+----+------------+-------------------------+---------+
|  1 | QQ APP     | http://im.qq.com/       | CN      |
|  2 | 微博 APP | http://weibo.com/       | CN      |
|  3 | 淘宝 APP | https://www.taobao.com/ | CN      |
+----+------------+-------------------------+---------+
3 rows in set (0.00 sec)
```

------

#### SQL UNION 实例

下面的 SQL 语句从 "Websites" 和 "apps" 表中选取所有**不同的**country（只有不同的值）：

##### 实例

```mysql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```



执行以上 SQL 输出结果如下：

![img](UNION与UNION_ALL.assets/union1.jpg)

**注释：**UNION 不能用于列出两个表中所有的country。如果一些网站和APP来自同一个国家，每个国家只会列出一次。UNION 只会选取不同的值。请使用 UNION ALL 来选取重复的值！

------

#### SQL UNION ALL 实例

下面的 SQL 语句使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**country（也有重复的值）：

##### 实例

```mysql
SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```



执行以上 SQL 输出结果如下：

![img](UNION与UNION_ALL.assets/union2.jpg)

------

#### 带有 WHERE 的 SQL UNION ALL

下面的 SQL 语句使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**中国(CN)的数据（也有重复的值）：

##### 实例

```mysql
SELECT country, name FROM Websites
WHERE country='CN'
UNION ALL
SELECT country, app_name FROM apps
WHERE country='CN'
ORDER BY country;
```



**这里可以看出第一个SELECT语句中的字段名称被用作最后结果的字段名**

执行以上 SQL 输出结果如下：

![img](UNION与UNION_ALL.assets/AAA99C7B-36A5-43FB-B489-F8CE63B62C71.jpg)





### 数据准备

student表

```mysql
-- ----------------------------
-- Table structure for `student`
-- ----------------------------
DROP TABLE IF EXISTS `student`;
CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(10) DEFAULT NULL,
  `age` tinyint(4) DEFAULT NULL,
  `classId` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
 
-- ----------------------------
-- Records of student
-- ----------------------------
INSERT INTO `student` VALUES ('1', 's1', '20', '1');
INSERT INTO `student` VALUES ('2', 's2', '22', '1');
INSERT INTO `student` VALUES ('3', 's3', '22', '2');
INSERT INTO `student` VALUES ('4', 's4', '25', '2');
```



教师表

```mysql
-- ----------------------------
-- Table structure for `teacher`
-- ----------------------------
DROP TABLE IF EXISTS `teacher`;
CREATE TABLE `teacher` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(10) DEFAULT NULL,
  `age` tinyint(4) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
 
-- ----------------------------
-- Records of teacher
-- ----------------------------
INSERT INTO `teacher` VALUES ('1', 't1', '36');
INSERT INTO `teacher` VALUES ('2', 't2', '33');
INSERT INTO `teacher` VALUES ('3', 's3', '22');
```



查询数据如下

```mysql
mysql> SELECT * FROM student;
+----+------+-----+---------+
| id | name | age | classId |
+----+------+-----+---------+
|  1 | s1   |  20 |       1 |
|  2 | s2   |  22 |       1 |
|  3 | s3   |  22 |       2 |
|  4 | s4   |  25 |       2 |
+----+------+-----+---------+
4 rows in set
 
mysql> SELECT * FROM teacher;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | t1   |  36 |
|  2 | t2   |  33 |
|  3 | s3   |  22 |
+----+------+-----+
3 rows in set
```



```mysql
mysql> SELECT id, name, age FROM student -- 这里可以看出第一个SELECT语句中的字段名称被用作最后结果的字段名
    -> UNION
    -> SELECT age, name, id FROM teacher;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | s1   |  20 |
|  2 | s2   |  22 |
|  3 | s3   |  22 |
|  4 | s4   |  25 |
| 36 | t1   |   1 |
| 33 | t2   |   2 |
| 22 | s3   |   3 |
+----+------+-----+
7 rows in set
```



使用 UNION的结果

```mysql
mysql> SELECT id, name, age FROM student
    -> UNION　　-- 与UNION DISTINCT相同
    -> SELECT id, name, age FROM teacher;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | s1   |  20 |
|  2 | s2   |  22 |
|  3 | s3   |  22 |
|  4 | s4   |  25 |
|  1 | t1   |  36 |
|  2 | t2   |  33 |
+----+------+-----+
6 rows in set
```



使用 UNION ALL的结果

```mysql
mysql> SELECT id, name, age FROM student
    -> UNION ALL
    -> SELECT id, name, age FROM teacher;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | s1   |  20 |
|  2 | s2   |  22 |
|  3 | s3   |  22 |
|  4 | s4   |  25 |
|  1 | t1   |  36 |
|  2 | t2   |  33 |
|  3 | s3   |  22 |
+----+------+-----+
7 rows in set
```



其实联合查询跟字段的类型无关，只要求每个SELECT查询的字段数一样，能对应即可，如

```mysql
mysql> SELECT id, name, age FROM student -- 这里可以看出第一个SELECT语句中的字段名称被用作最后结果的字段名
    -> UNION
    -> SELECT age, name, id FROM teacher;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | s1   |  20 |
|  2 | s2   |  22 |
|  3 | s3   |  22 |
|  4 | s4   |  25 |
| 36 | t1   |   1 |
| 33 | t2   |   2 |
| 22 | s3   |   3 |
+----+------+-----+
7 rows in set
```



在联合查询中，当使用ORDER BY的时候，需要对SELECT语句添加括号，并且与LIMIT结合使用才生效，如

```mysql
mysql> (SELECT classId, id, name, age FROM student WHERE classId = 1 ORDER BY age DESC)
    -> UNION
    -> (SELECT classId, id, name, age FROM student WHERE classId = 2 ORDER BY age);
+---------+----+------+-----+
| classId | id | name | age |
+---------+----+------+-----+
|       1 |  1 | s1   |  20 |
|       1 |  2 | s2   |  22 |
|       2 |  3 | s3   |  22 |
|       2 |  4 | s4   |  25 |
+---------+----+------+-----+
4 rows in set
此时classId为1的学生并没有按照年龄进行降序，结合LIMIT后
```



此时classId为1的学生并没有按照年龄进行降序，结合LIMIT后

```mysql
mysql> (SELECT classId, id, name, age FROM student WHERE classId = 1 ORDER BY age DESC LIMIT 2)
    -> UNION
    -> (SELECT classId, id, name, age FROM student WHERE classId = 2 ORDER BY age);
+---------+----+------+-----+
| classId | id | name | age |
+---------+----+------+-----+
|       1 |  2 | s2   |  22 |
|       1 |  1 | s1   |  20 |
|       2 |  3 | s3   |  22 |
|       2 |  4 | s4   |  25 |
+---------+----+------+-----+
4 rows in set
 
```




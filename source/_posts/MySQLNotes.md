title: MySQL Summary
date: 2017-10-03 21:24:57
tags: [MySQL, notes]
categories: summary
description: MySQL summary。
---

## MySQL
Mysql 是最流行的关系型数据库管理系统，由瑞典MySQL公司开发，目前属于Oracle。MySQL使用标准的SQL数据语言形式。

## Concept In MySQL

- 主键： 主键是唯一的，一个数据表只能有一个主键， 可以用主键来查询数据。
- 外键：用于关联两个数据表。
- 复合键(组合键)：将多个列作为一个索引键，一般用于复合索引。 

## MySQL Install and Setup

- `service mysqld start` 启动mysql
- `mysql -h localhost -u root -p` 进入mysql client端进行执行简单的SQL命令
- `quit` OR `Ctrl + D` 退出mysql。
- `mysqladmin -u root password "new_password";` to set new password.
- `SHOW DATABASES;` to list all databases

## MySQL 数据类型
MySQL中主要有三种数据类型：熟悉、日期|时间、字符串，MySQL支持所有标准SQL数值数据类型 [See more info.](http://www.cnblogs.com/zbseoag/archive/2013/03/19/2970004.html)。

## MySQL 数据库操作命令

- `SHOW DATABASES;` To list all databases
- `use XXX;` To choose the XXX database
- `SHOW TABLES;` 列出该数据库中所有表
- `SHOW COLUMNS FROM XX` OR `DESCRIBE XX;` 显示数据表的属性信息
- `SHOW INDEX FROM XX` 显示数据表的详细索引信息
- `SHOW TABLE STATUS FROM XXX` 显示数据库管理系统的性能及统计信息
- `SHOW TABLE STATUS FROM XXX LIKE 'runoob%'` 显示以runoob开头的表的信息
- `SHOW TABLE STATUS FROM XXX LIKE 'runoob%'\G` 显示以runoob开头的表的信息, 结果按照列打印
- `select version(),current_date();` 显示版本和日期， 可见mysql对大小写结果一致。也可多行语句， 直到见到";"为止。

## MySQL 常用操作命令

- `create database XXX;` 创建数据库XXX
- `deop database XXX;` 删除数据库XXX
- `use XXX;` 选择数据库XXX
- `CREATE TABLE table_name(column_name column_type);` 创建数据表

例如：
```
mysql> create table test_table( id INT NOT NULL AUTO_INCREMENT, name VARCHAR(20) NOT NULL, sex CHAR(1) NOT NULL, birth DATE, birth_addr VARCHAR(20), PRIMARY KEY (id)); 
```
- `drop table XXX;` 删除数据表；
- `insert into table_name (filed1, ...fieldn 可省略) values (value1, value2, ...valuen)` 插入数据；

例如：

```
insert into test_table values (1, 'john', 'm', '1992-01-29', 'shanghai');
```
- `select * from table_name;`显示该表下的全部数据；

MySQL 查询数据语句：

```
SELECT column_name,column_name
FROM table_name1, table_name2
[WHERE Clause]
[OFFSET M ][LIMIT N]
```
例如：

```
select name,sex from test_table where id > 1 limit 1;
```
- `update table_name set field1=new-value1, field2=new-value2` 更新数据

例如：

```
update test_table set name='mike' where id=1;
```
- `delete from table_name [where clause];`  删除表中数据，若没有指定条件，则删除整个表。

- `WHERE FIELD LIKE '%XXX';` Like子句 + %，起到查询包含XXX的数据的左右， 例如： `select name from test_table where birth like '1992%';`
- `WHERE NAME REGEXP 'regex expression'`, 例如： `SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$'; `

### ALTER 使用

- `ALTER TABLE testalter_tbl  DROP i;` 删除一列
- `ALTER TABLE testalter_tbl ADD i INT;` 增加一列
- `ALTER TABLE testalter_tbl ADD i INT FIRST;` 在指定位置增加一列
- `ALTER TABLE testalter_tbl ADD i INT AFTER c;` 在指定位置增加一列
- `ALTER TABLE testalter_tbl MODIFY c CHAR(10);` 修改字段类型
- `ALTER TABLE testalter_tbl CHANGE j j INT;` 修改字段名以及类型
- `ALTER TABLE testalter_tbl 
    -> MODIFY j BIGINT NOT NULL DEFAULT 100;` 设置默认值和NOT NULL
- `ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;` 修改字段默认值
- `ALTER TABLE testalter_tbl ALTER i DROP DEFAULT;` 删除字段默认值
- `ALTER TABLE testalter_tbl RENAME TO alter_tbl;` 更改表名

## Reference

- http://blog.csdn.net/chinacodec/article/details/5797127/
- http://www.runoob.com/mysql/mysql-create-database.html
- http://www.runoob.com/java/java-mysql-connect.html

---
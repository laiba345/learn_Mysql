**mysql约束**
```
-- 主键使用
/*
	细节
	1. PRIMARY KEY不能重复而且不能为null
	2. 一张表最多只能🈶一个主键,但可以是复合主键; 
	3. 主键的定义方式有两种: 
		- 直接在字段名后指定: 字段名 PRIMARY KEY
		- 在表定义最后写 PRIMARY KEY(列名)
	4. 使用desc 表名,可以看到
	5. 在实际开发中,每个表往往都会设计一个主键
*/
CREATE TABLE t10(
	id INT PRIMARY KEY, -- 表示id列是主键
	`name` VARCHAR(32), 
	email VARCHAR(32)); 
-- !!!主键列的值是不可以重复的
INSERT INTO t10 VALUES(1, 'jack', 'hello@kang.com'); 
INSERT INTO t10 VALUES(1, 'kang', 'hello@kang.com'); -- 报错

DESC t10; 

-- UNIQUE 唯一; 
/*
	细节
	1. 如果没有指定not null, 则unique字段可以有多个null
	2. 一张表中可以有多个unique; 
*/
CREATE TABLE t11(
	id INT UNIQUE, -- 表示id列是不可以重复的
	`name` VARCHAR(32), 
	email VARCHAR(32)); 
INSERT INTO t11 VALUES(1, 'jack', 'hello@kang.com'); 
INSERT INTO t11 VALUES(1, 'kang', 'hello@kang.com'); -- 报错
```
**外键约束**
```
-- 外键的使用
-- 创建 主表 my_class
CREATE TABLE my_class(
	id INT PRIMARY KEY, 
  `name` VARCHAR(32) NOT NULL DEFAULT ''); 
-- 创建从表 my_stu
CREATE TABLE my_stu(
	id INT PRIMARY KEY, -- 学生编号
	`name` VARCHAR(32) NOT NULL DEFAULT '',
	class_id INT, 
	-- 下面指定外键关系
	FOREIGN KEY(class_id) REFERENCES my_class(id)); 

-- 测试数据
INSERT INTO my_class VALUES(100, 'java'), (200, 'web'); 
SELECT * FROM my_class; 
INSERT INTO my_stu VALUES(1, 'tom', 100); 
INSERT INTO my_stu VALUES(2, 'jack', 200);
INSERT INTO my_stu VALUES(3, 'hsp', 300);  -- 失败,因为300班级不存在;  
SELECT * FROM my_stu;
-- 一旦建立主外键的关系, 数据不能随意删除了
DELETE FROM my_class WHERE id = 100; -- 删除失败; 
```

**check约束**
```
-- check约束
-- 测试
CREATE TABLE t12(
	id INT PRIMARY KEY, 
	`name` VARCHAR(32),
	sex VARCHAR(6) CHECK (sex IN('man', 'woman')), 
	sal DOUBLE CHECK (sal > 1000 AND sal < 2000)
	); 

INSERT INTO t12 VALUES(1, 'hk', 'man', 1500); 
SELECT * FROM t12; 
```
**check()在括号里面写的是表达式**

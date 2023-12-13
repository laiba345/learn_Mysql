Mysql的列类型
1. 数值类型; 在满足需求的情况下,尽量选择占用空间小的类型;
2. 文本类型(字符串类型)
3. 二进制数据类型
4. 日期类型


数据类型的演示操作TINYINT
```
# 如果没有指定unsinged, 则表示有符号, 否则表示无符号;
# 演示整形的使用
CREATE TABLE t3 (
		id TINYINT); 
		
CREATE TABLE t4 (
		id TINYINT UNSIGNED);  
		
INSERT INTO t3 VALUES(-128); 
INSERT INTO t4 VALUES(-128); 
```

数据类型的演示操作; 小数型号
```
# 演示decimal、float、double的使用
# 创建表; DECIMAL可以存放非常大的数据;
CREATE TABLE t6 (
		num1 FLOAT, 
		num2 DOUBLE, 
		num3 DECIMAL(30, 20));
	
# 添加数据
INSERT INTO t6 VALUES(88.1234567891234, 88.1234567891234, 88.1234567891234); 
```

字符串数据类型的基本使用
```
# 字符串的基本使用 CHAR、VARCHAR
CREATE TABLE t7 (
		`name` CHAR(255));
		
CREATE TABLE t8 (
		`name` VARCHAR(10000));
		
DROP TABLE t8; 
```
注意: 如果varchar不够用,可以尝试使用mediumtext,或者longtext, 
如果想简单点,可以直接使用text


日期类型的基本使用
```
# 演示时间相关的类型
CREATE TABLE t9(
		birthday DATE, -- 生日
		job_time DATETIME, -- 记录年月日,时分秒
		login_time TIMESTAMP 
		NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP); 
		
SELECT * FROM t9;

INSERT INTO t9(birthday, job_time) VALUES('2022-11-11', '2022-11-11 10:10:10'); 
```

创建表练习
```
# 创建表练习
CREATE table `emp` (
		id INT, 
		`name` VARCHAR(32), 
		sex CHAR(1), 
		birthday DATE, 
		entry_data DATETIME, 
		job VARCHAR(32),
		salary DOUBLE, 
		`resume` TEXT) CHARSET utf8 COLLATE utf8_bin ENGINE INNODB; 

# 添加一条
INSERT INTO `emp` VALUES(100, 'hk', '男', '2000-11-11', '2010-11-10 11:11:11', '开心的', 20000, '自信');
```

修改表的相关操作
```
# 修改表的操作练习
ALTER TABLE emp ADD image VARCHAR(32) NOT NULL DEFAULT '' AFTER resume; 
# 查看表的所有列
DESC emp;
# 修改job列, 使其长度为60
ALTER TABLE emp MODIFY job VARCHAR(60) NOT NULL DEFAULT ''; 
# 删除sex列
ALTER TABLE emp DROP sex; 
# 表名改为employee
RENAME TABLE emp TO employee; 
#  修改表的字符集为 utf8
ALTER TABLE employee CHARACTER SET utf8; 
-- 将列名name修改为user_name
ALTER TABLE employee
	CHANGE `name` `user_name` VARCHAR(64) NOT NULL DEFAULT ''; 
	
DESC employee; 
```

**修改表的相关操作**
1. 添加某一列; ALTER + ADD
2. 修改某一列; ALTER + MODIFY
3. 删除某一列; ALTER + DROP
4. 修改表名; RENAME
5. 修改表里面的列属性; ALTER + CHANGE


**插入操作的话,可以写具体的列也可以不写具体的列**


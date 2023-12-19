**Mysql管理**
```
/*
	Mysql用户管理
	原因: 1、当我们做项目开发时, 可以根据不同的开发人员, 赋给它们相应的Mysql操作权限
	2. Mysql数据库管理人员(root),根据需要创建不同的用户,赋给相应的权限,供人员使用
*/
-- 1. 创建新的用户
/*
	解释; CREATE USER 'hsp_edu'@'localhost' IDENTIFIED BY '123456'; 
	1. 'hsp_edu'@'localhost' ; 
		- 表示用户的完整信息; 
		- 'hsp_edu'; 用户名, 'localhost' 登陆的IP
	2. 123456; 密码; 但是注意 存放到mysql.user表时, 是经过password(‘123456’)加密后的密码; 
*/
CREATE USER 'hsp_edu'@'localhost' IDENTIFIED BY '123456';
SELECT * FROM mysql.user;  
SELECT `host`, `user` FROM mysql.user; 
-- 2. 删除用户
DROP USER 'hsp_edu'@'localhost'; 
-- 3. 登录; 参照sqlyog登录

-- 修改自己密码; 没问题
SET PASSWORD = PASSWORD('abcdef'); 
-- 修改其他人的密码, 需要权限; 
-- root用户修改 'hsp_edu'@'localhost'密码,是可以成功的; 权限够; 
```

**演示用户权限管理**
```
-- 演示 用户权限的管理
-- 创建用户 shunping 密码 123
CREATE USER 'shunping'@'localhost' IDENTIFIED BY '123'; 
-- 使用root用户创建testdb, 表news
CREATE DATABASE testdb; 
CREATE TABLE news02(
	id INT, 
	content VARCHAR(32)); 
-- 添加一条测试数据
INSERT INTO news02 VALUES(100, '北京新闻'); 
SELECT * FROM news02; 

-- 给 shunping 分配查看news表 和 添加news的权限
GRANT SELECT, INSERT ON hsp_db01.news02 TO 'shunping'@'localhost'; 
-- 添加update权限
GRANT UPDATE ON hsp_db01.news02 TO 'shunping'@'localhost'; 
-- 修改shunping的密码为abc
SET PASSWORD FOR  'shunping'@'localhost' = PASSWORD('abc'); 
-- 回收shunping用户在testdb.news表的所有权限; 
REVOKE SELECT, UPDATE, INSERT ON hsp_db01.news02 FROM 'shunping'@'localhost'; 
REVOKE ALL ON hsp_db01.news02 FROM 'shunping'@'localhost'; 
-- 删除 shunping
DROP USER 'shunping'@'localhost'; 
```
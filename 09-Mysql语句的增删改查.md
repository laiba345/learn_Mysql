**Insert语句; 插入**
```
CREATE TABLE `goods` (
		id INT, 
		goods_name VARCHAR(10), 
		price DOUBLE NOT NULL DEFAULT 100);  -- 给该列一个固定的字段,并给予默认值,如果没有设置列值的话就使用默认值

DROP TABLE goods; 
		
-- 添加数据
INSERT INTO `goods` (id, goods_name, price)
		VALUES(10, '华为手机', 2000);
		
INSERT INTO `goods` (id, goods_name, price)
		VALUES(20, '苹果手机', 5999);

SELECT * FROM goods;
```

**update语句; 更新**
```
-- 1. 将所有员工薪水修改为5000元[如果没有带where条件,会修改所有记录,因此要小心]
UPDATE employee SET salary = 5000; 
-- 2. 将姓名为小妖怪的员工薪水修改为3000
UPDATE employee SET salary = 3000
		WHERE user_name = 'hk'; 
-- 3. 将kb的薪水在原有基础上增加1000元
INSERT INTO employee 
	VALUES(2, 'kb', '1990-11-11', '2023-11-11 11:11:11', '打球的', 5000, 'hello', '图片路径'); 

UPDATE employee SET salary = salary + 1000 WHERE user_name = 'kb';

SELECT * FROM employee; 
```

**delete删除语句;**
```
-- 删除语句
-- 1. 删除表中名称为 'kb'的记录
DELETE FROM employee WHERE user_name = 'kb'; 
-- 2. 删除表中所有记录,提醒: 一定要小心; 并非删除表
DELETE FROM employee; 
-- 3. DELETE语句不能删除某一列的值(可以使用update设为null或者'')
UPDATE employee SET job = '' WHERE user_name = 'hk'; 
-- 4. 要删除某张表, 而不是某张记录
DROP TABLE employee; 

SELECT * FROM employee; 
```

**select语句, 查询**
```
-- 创建一张新的学生表
CREATE TABLE student(
	id INT NOT NULL DEFAULT 1, 
	`name` VARCHAR(20) NOT NULL DEFAULT '', 
	chinese FLOAT NOT NULL DEFAULT 0.0, 
	english FLOAT NOT NULL DEFAULT 0.0, 
	math FLOAT NOT NULL DEFAULT 0.0
); 
INSERT INTO student(id, name, chinese, english, math) VALUES(1, 'hk', 99, 99, 99); 
INSERT INTO student(id, name, chinese, english, math) VALUES(2, 'hkk', 11, 22, 33); 
INSERT INTO student(id, name, chinese, english, math) VALUES(3, 'kk', 44, 55, 66); 
INSERT INTO student(id, name, chinese, english, math) VALUES(4, 'mm', 77, 88, 99); 
INSERT INTO student(id, name, chinese, english, math) VALUES(5, 'nn', 67, 68, 69); 
INSERT INTO student(id, name, chinese, english, math) VALUES(6, 'jj', 19, 39, 99); 
INSERT INTO student(id, name, chinese, english, math) VALUES(7, 'zz', 79, 99, 99); 

DELETE FROM student; 

SELECT * FROM student; 
-- SELECT语句的使用1
-- 1. 查询表中所有学生的信息
SELECT * FROM student; 
-- 2. 查询表中所有学生的姓名和对应的英语成绩
SELECT `name`, english from student; 
-- 3. 过滤掉表中重复数据distinct
SELECT DISTINCT english FROM student; 
-- 4. 要查询的记录,每个字段都相同,才会去重
SELECT DISTINCT `name`, english FROM student; 

-- SELECT语句的使用2
-- 1. 统计每个学生的总分
SELECT `name`, (chinese+english+math) FROM student;
-- 2. 在所有学生总分加10分的情况
SELECT `name`, (chinese+english+math+10) FROM student;
-- 3. 使用别名表示学生分数
SELECT `name`, (chinese+english+math+10) AS total_score FROM student;

-- SELECT语句的使用3
-- 1. 查询姓名为hk的学生成绩
SELECT * FROM student WHERE `name` = 'hk'; 
-- 2. 查询英语成绩大于90分的同学
SELECT * FROM student WHERE english > 90;
-- 3. 查询总分大于200分的所有同学
SELECT * FROM student WHERE (chinese + english + math) > 200; 

-- SELECT语句的使用4
-- 1. 查询math大于60并且(and)id大于4的学生成绩
SELECT * FROM student WHERE math > 60 AND id > 4; 
-- 2. 查询英语成绩大于语文成绩的同学
SELECT * FROM student WHERE english > chinese;
-- 3. 查询总分大于200分并且数学成绩大于语文成绩的姓n的学生
SELECT * FROM student WHERE (chinese + english + math) > 200 AND (math > chinese) AND `name` LIKE 'n%'; 


-- SELECT语句的使用5
-- 1. 查询英语分数在80-90之间的学生成绩
SELECT * FROM student WHERE english >= 80 AND english <= 90; 
SELECT * FROM student WHERE english BETWEEN 80 AND 90;  -- BETWEEN...AND...是闭区间
-- 2. 查询数学分数为89,90,91的的同学
SELECT * FROM student WHERE math = 89 OR math = 90 OR math = 91;
SELECT * FROM student WHERE math IN (89, 90, 91);
-- 3. 查询所有姓h的同学
SELECT * FROM student WHERE `name` LIKE 'h%'; 

-- SELECT语句的使用6
-- ORDER BY 的使用
-- 1. 对数学成绩排序后输出[升序]
SELECT * FROM student ORDER BY math; 
-- 2. 对总分按从高到低的顺序输出[降序]; 还可以通过别名排序
SELECT `name`, (chinese+english+math) AS total_score FROM student ORDER BY total_score DESC; 
-- 3. 对姓h的学生成绩[总分]排序输出(升序) WHERE + ORDER BY
SELECT `name`, (chinese+english+math) AS total_score FROM student
	WHERE `name` LIKE 'h%' ORDER BY total_score; 

-- SELECT语句的使用7
-- 统计函数的使用
-- 1. 统计一个班级有多少学生
SELECT COUNT(*) FROM student; 
-- 2. 统计数学成绩大于90的学生有多少个?
SELECT COUNT(*) FROM student WHERE math > 90; 
-- 3. 统计总分大于250的人数有多少?
SELECT COUNT(*) FROM student WHERE (math + english + chinese) > 250; 
-- 4. 解释count(*) 和 count(列)的区别	
	-- count(*)返回满足条件的记录的行数, count(列)统计满足条件的列有多少个;
-- sum函数的使用
-- 1. 统计一个班级数学总成绩?
SELECT SUM(math) FROM student; 
-- 2. 统计一个班级语文、数学、英语各科的总成绩
SELECT SUM(math) AS math_total_score, SUM(english), SUM(chinese) FROM student; 
-- 3. 统计一个班语文、英语、数学的成绩总和
SELECT SUM(math + english + chinese) FROM student; 
-- 4. 统计一个班级语文成绩平均分
SELECT SUM(chinese) / COUNT(*) FROM student; 

-- Avg函数的使用
-- 1. 求一个班级数学平均分
SELECT AVG(math) from student; 
-- 2. 求一个班级总分平均分
SELECT AVG(math + english + chinese) FROM student; 

-- max和min的使用
-- 求班级最高分和最低分
SELECT MAX(math + english + chinese), MIN(math + english + chinese) FROM student; 

-- 部门表 
CREATE TABLE dept(
    deptno MEDIUMINT NOT NULL DEFAULT 0,
    dname VARCHAR(20) NOT NULL DEFAULT '',
    loc VARCHAR(13) NOT NULL DEFAULT ''
);
INSERT INTO dept VALUES(10,'ACCOUNTING','NEW YORK'),
    (20,'RESEARCH','DALLAS'),
    (30,'SALES','CHICAGO'),
    (40,'OPERATIONS','BOSHTON');  
SELECT * FROM dept;

-- 员工表
CREATE TABLE `emp` (
  `empno` mediumint(8) unsigned NOT NULL DEFAULT '0',
  `ename` varchar(20) COLLATE utf8_bin NOT NULL DEFAULT '""',
  `job` varchar(9) COLLATE utf8_bin NOT NULL DEFAULT '""',
  `mgr` mediumint(8) unsigned DEFAULT NULL,
  `hiredate` date NOT NULL,
  `sal` decimal(7,2) NOT NULL,
  `comm` decimal(7,2) DEFAULT NULL,
  `deptno` mediumint(8) unsigned NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

-- 添加测试数据
INSERT INTO emp VALUES(7369,'SMITH','CLERK',7902,'1990-12-17',800.00,NULL,20),
    (7499,'ALLEN','SALESMAN',7698,'1991-2-20',1600.00,300.00,30),
    (7521,'WARD','SALESMAN',7968,'1991-2-22',1250.00,500.00,30),
    (7566,'JONES','MANAGER',7839,'1991-4-2',2975.00,NULL,20),
    (7654,'MARTIN','SALESMAN',7968,'1991-9-28',1250.00,1400.00,30),
    (7698,'BLAKE','MANAGER',7839,'1991-5-1',2850.00,NULL,30),
    (7782,'CLARK','MANAGER',7839,'1991-6-9',2450.00,NULL,10),
    (7788,'SCOTT','ANALYST',7566,'1991-4-19',3000.00,NULL,20),
    (7839,'KING','PRESIDENT',NULL,'1991-11-17',5000.00,NULL,10),
    (7844,'TURNER','SALESMAN',7698,'1991-9-8',1500.00,NULL,30),
    (7900,'JAMES','CLERK',7698,'1991-12-3',950.00,NULL,30),
    (7902,'FORD','ANALYST',7566,'1991-12-3',3000.00,NULL,20),
    (7934,'MILLER','CLERK',7782,'1991-1-23',1300.00,NULL,10);
		
SELECT * FROM emp;

CREATE TABLE salgrade(
grade MEDIUMINT UNSIGNED NOT NULL DEFAULT 0, -- 工资级别
losal DECIMAL(17, 2) NOT NULL, -- 该级别的最低工资
hisal DECIMAL(17, 2) NOT NULL); -- 该级别最高工资

INSERT INTO salgrade VALUES(1, 700, 1200),
(2, 1201, 1400),
(3, 1401, 2000),
(4, 2001, 3000),
(5, 3001, 9999)

-- 演示GROUP BY + HAVING
-- 1. 如何显示每个部门的平均工资和最高工资
-- 2. 按照部分来分组查询
SELECT AVG(sal), MAX(sal), deptno FROM emp GROUP BY deptno; 

-- 显示每个部门的每种岗位的平均工资和最低工资; 
	-- 1. 显示每个部门的平均工资和最低工资
	-- 2. 显示每个部门的每种岗位的平均工资和最低工资
SELECT AVG(sal), MIN(sal), deptno, job FROM emp GROUP BY deptno, job; 
-- 显示平均工资低于2000的部门号和它的平均工资 思路: 化繁为简, 各个击破
	-- 1. 显示各个部门的平均工资和部门号
	-- 2. 在1的结果基础上, 进行过滤, 保留AVG(sal) < 2000
-- SELECT AVG(sal), deptno FROM emp GROUP BY deptno; 
-- SELECT AVG(sal), deptno FROM emp GROUP BY deptno HAVING AVG(sal) < 2000; 
-- 取别名
SELECT AVG(sal) AS avg_sal, deptno FROM emp GROUP BY deptno HAVING avg_sal < 2000; 
```
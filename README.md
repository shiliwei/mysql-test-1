17061725 石力玮
## 题目1：建立一个mysql database, 包含以下两张表：

```sql
mysql> set names gbk;
Query OK, 0 rows affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| slw                |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```

![](https://github.com/shiliwei/mysql-test-1/blob/master/1.png) 

## 题目2：建表 查表 改表 改表字段类型
```
create table t_dept (
    deptno INT,
	dname  VARCHAR(20),
	loc    VARCHAR(40)
);
describe t_dept;
show create table t_dept;
drop table t_dept;
ALTER TABLE t_dept  
    ADD descri VARCHAR(20);	
ALTER TABLE t_dept  
    ADD descri_head VARCHAR(20) FIRST;	
ALTER TABLE t_dept
     MODIFY deptno VARCHAR(20);
ALTER TABLE t_dept
     MODIFY deptno INT;
     -- Now create a new table
create table t_dept (
    deptno INT,
	dname  VARCHAR(20),
	loc    VARCHAR(40)
);


-- show the table
describe t_dept;
-- another way to show the table
show create table t_dept;

-- delete the table
drop table t_dept;

-- alter table to add a column
ALTER TABLE t_dept  
    ADD descri VARCHAR(20);
	
-- alter table to add a column in the first column
ALTER TABLE t_dept  
    ADD descri_head VARCHAR(20) FIRST;	
	
-- modify deptno from INT to VARCHAR
ALTER TABLE t_dept
     MODIFY deptno VARCHAR(20);
-- from VARCHAR back to INT
ALTER TABLE t_dept
     MODIFY deptno INT;
	 
-- Not NULL constrains
CREATE TABLE t_dept_2(
	deptno INT(20) NOT NULL,
	dname VARCHAR(20),
	loc VARCHAR(40)
);

-- INSERT a record to both tables
INSERT INTO t_dept (descri_head, deptno, dname, loc, descri) VALUES ("head", 1, "myName", "Hangzhou", "waha");
INSERT INTO t_dept (descri_head, deptno, dname, loc, descri) VALUES ("head2", NULL, "myName2", "Shanghai", "newPlace");

INSERT INTO t_dept_2 (deptno, dname, loc) VALUES (1, "myName_t2", "Hangzhou_t2");
INSERT INTO t_dept_2 (deptno, dname, loc) VALUES (NULL, "myName_t2", "Hangzhou_t2");


-- Auto increment
CREATE TABLE t_dept(
	deptno INT(20) PRIMARY KEY AUTO_INCREMENT,
	dname VARCHAR(20),
	loc VARCHAR(40)
);
-- insert records;
INSERT INTO t_dept (dname, loc) VALUES ("myName_1", "Hangzhou_1");
INSERT INTO t_dept (dname, loc) VALUES ("myName_2", "Hangzhou_2");
-- show records
SELECT * FROM t_dept;


--insert data
INSERT INTO t_dept (deptno, dname, loc)
    VALUES (1, "cjg", 'shangxi1');
-- omit field name	
INSERT INTO t_dept 
    VALUES (1, "cjg", 'shangxi1');
-- ANY ORDER
INSERT INTO t_dept (deptno,loc,dname)
    VALUES (3, 'shanghai1', 'wka');
-- NULL
INSERT INTO t_dept (deptno, loc)
    VALUES (4, 'zj1');
	
-- INSERT multi records
INSERT INTO t_dept (deptno, dname, loc)
    VALUES (11, "x1", 'l1'), 
	       (12, "x2", '12'),
		   (13, "x3", '13');
		   
-- read an old table and write into new table
-- frist create a new table;
create table t_dept2 (
    deptno INT AUTO_INCREMENT PRIMARY KEY,
	dname  VARCHAR(20),
	loc    VARCHAR(40)
);		 
-- insert into new table; f(g())  /*View*/ vs /*Data copy*/
INSERT INTO t_dept2 (dname, loc)
	SELECT dname, loc FROM t_dept;
     
     ```
![](https://github.com/shiliwei/mysql-test-1/blob/master/2.png) 
![](https://github.com/shiliwei/mysql-test-1/blob/master/3.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/4.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/5.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/6.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/7.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/8.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/9.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/10.png)
![](https://github.com/shiliwei/mysql-test-1/blob/master/11.png)
```



## 题目3： inner join 笛卡尔积 行列消减
```
DROP TABLE IF EXISTS t_employee;

CREATE TABLE t_employee2 (
    deptno INT NOT NULL,
	empno INT  PRIMARY KEY,
	ename VARCHAR(20),
	job  VARCHAR(20),
    MGR  INT,
	Hiredate Date,
	sal    float,
	comm   float
);



INSERT INTO t_employee2 (empno, ename, job, MGR, Hiredate, sal, comm, deptno) VALUES 
    (7369, "SMITH", "CLERK", 7902, "1981-03-12", 800.00, NULL, 20),
	(7499, "ALLEN", "SALESMAN", 7698, "1982-03-12", 1600, 300, 30),
	(7521, "WARD", "SALESMAN", 7698, "1838-03-12", 1250, 500, 30),
	(7566, "JONES", "MANAGER", 7839, "1981-03-12", 2975, NULL, 20),
	(7654, "MARTIN", "SALESMAN", 7698, "1981-01-12", 1250, 1400, 30),
	(7698, "BLAKE", "MANAGER", 7839, "1985-03-12", 2450, NULL, 10),
	(7788, "SCOTT", "ANALYST", 7566, "1981-03-12", 3000, NULL, 20),
	(7839, "KING", "PRESIDENT", NULL, "1981-03-12", 5000, NULL, 10),
	(7844, "TURNER", "SALESMAN", 7689, "1981-03-12", 1500, 0, 30),
	(7878, "ADAMS", "CLERK", 7788, "1981-03-12", 1100, NULL,20),
	(7900, "JAMES", "CLERK", 7698,"1981-03-12",  950, NULL, 30),
	(7902, "FORD", "ANALYST", 7566, "1981-03-12", 3000, NULL, 20),
	(7934, "MILLER", "CLERK", 7782, "1981-03-12", 1300, NULL, 10)
	;
	
DROP TABLE IF EXISTS t_dept;

CREATE TABLE t_dept (
    deptno INT PRIMARY KEY,
	dname VARCHAR(30),
	loc VARCHAR(50)
);

INSERT INTO t_dept VALUES 
(10, "ACCOUNTING", "NEW YORK"),
(20, "RESEARCH", "DALLAS"),
(30, "SALES", "CHICAGO"),
(40, "OPERATIONS", "BOSTON")
;

select *
from t_employee2 t1 inner join t_employee2 t2 
on t1.empno = t2.mgr; 
![](https://github.com/shiliwei/mysql-test-1/blob/master/12.png) 
```
 
## 题目4：运行上述存储过程和函数
```
DELIMITER $$
CREATE PROCEDURE proce_employee_sal1 ()
BEGIN
    SELECT sal
	FROM t_employee;
END$$
DELIMITER ;

CALL proce_employee_sal1();

DELIMITER $$
CREATE FUNCTION func_employee_sal (empno INT)
RETURNS DOUBLE
BEGIN
    RETURN (SELECT sal FROM t_employee WHERE t_employee.empno = empno);
END$$
DELIMITER ;

SELECT func_employee_sal(7369);
![](https://github.com/shiliwei/mysql-test-1/blob/master/13.png) 
```


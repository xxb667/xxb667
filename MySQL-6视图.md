# MySQL-6视图

## 介绍

- 视图（view）是一个**虚拟表**，非真实存在
  - 其本质是根据SQL语句获取动态的数据集，并为其命名
  - 用户使用时只需使用**视图名称**即可获取结果集，
  - 并可以将其当作表来使用。
- 数据库中只存放了视图的定义，而并没有存放视图中的数据。
  - 这些**数据存放在原来的表**中。
- 使用视图查询数据时，数据库系统会从原来的表中取出对应的数据。
  - 因此，视图中的数据是依赖于原来的表中的数据的。
  - 一旦原表中的数据发生改变，显示在视图中的数据也会发生改变。

## 作用

- **简化代码**
  - 可以把重复使用的查询封装成视图重复使用
  - 同时可以使复杂的查询易于理解和使用。
- **安全原因**
  - 如果一张表中有很多数据，很多信息不希望让所有人看到
  - 此时可以使用视图
  - 如：
    - 社会保险基金表，可以用视图只显示姓名，地址
    - 而不显示社会保险号和工资数等，
    - 可以对不同的用户，设定不同的视图。

## 视图的创建

### 语法格式

```
create [or replace] [algorithm = {undefined | merge | temptable}]
 
view view_name [(column_list)]
 
as select_statement
 
[with [cascaded | local] check option]

```

参数说明：

```
参数说明：
（1）algorithm：可选项，表示视图选择的算法。
（2）view_name ：表示要创建的视图名称。
（3）column_list：可选项，指定视图中各个属性的名词，默认情况下与SELECT语句中的查询的属性相同。
（4）select_statement
：表示一个完整的查询语句，将查询记录导入视图中。
（5）[with [cascaded | local] check option]：可选项，表示更新视图时要保证在该视图的权限范围之内。
```

## 数据准备

### 创建数据库

- 创建 数据库mydb6_view,
- 然后在该数据库下执行sql脚本view_data.sql 导入数据

```
create database mydb6_view;
```

### 创建表并插入数据

#### 部门表

```
-- 部门表
use mydb6_view;
create table dept(
	deptno int primary key,
  dname varchar(20),
	loc varchar(20)
);

-- 插入数据
insert into dept values(10, '教研部','北京'),
(20, '学工部','上海'),
(30, '销售部','广州'),
(40, '财务部','武汉');

```

#### 员工表

```
-- 员工表
use mydb6_view;
create table emp(
	empno int primary key,
	ename varchar(20),
	job varchar(20),
	mgr int,
	hiredate date,
	sal numeric(8,2),
	comm numeric(8, 2),
	deptno int,
-- 	FOREIGN KEY (mgr) REFERENCES emp(empno),
	FOREIGN KEY (deptno) REFERENCES dept(deptno) ON DELETE SET NULL ON UPDATE CASCADE
);

-- 插入数据
insert into emp values
(1001, '甘宁', '文员', 1013, '2000-12-17', 8000.00, null, 20),
(1002, '黛绮丝', '销售员', 1006, '2001-02-20', 16000.00, 3000.00, 30),
(1003, '殷天正', '销售员', 1006, '2001-02-22', 12500.00, 5000.00, 30),
(1004, '刘备', '经理', 1009, '2001-4-02', 29750.00, null, 20),
(1005, '谢逊', '销售员', 1006, '2001-9-28', 12500.00, 14000.00, 30),
(1006, '关羽', '经理', 1009, '2001-05-01', 28500.00, null, 30),
(1007, '张飞', '经理', 1009, '2001-09-01', 24500.00, null, 10),
(1008, '诸葛亮', '分析师', 1004, '2007-04-19', 30000.00, null, 20),
(1009, '曾阿牛', '董事长', null, '2001-11-17', 50000.00, null, 10),
(1010, '韦一笑', '销售员', 1006, '2001-09-08', 15000.00, 0.00, 30),
(1011, '周泰', '文员', 1008, '2007-05-23', 11000.00, null, 20),
(1012, '程普', '文员', 1006, '2001-12-03', 9500.00, null, 30),
(1013, '庞统', '分析师', 1004, '2001-12-03', 30000.00, null, 20),
(1014, '黄盖', '文员', 1007, '2002-01-23', 13000.00, null, 10);

```

#### 薪资等级表

```
-- 薪资等级表
use mydb6_view;
create table salgrade(
	grade int primary key,
	losal int,
	hisal int
);

-- 插入数据
insert into salgrade values
(1, 7000, 12000),
(2, 12010, 14000),
(3, 14010, 20000),
(4, 20010, 30000),
(5, 30010, 99990);

```

![image-20220116183835820](C:/Users/18393/AppData/Roaming/Typora/typora-user-images/image-20220116183835820.png)

## 操作

```
create or replace view view1_emp
as 
select ename,job from emp; 

-- 查看表和视图 
show full tables;
 
```

![image-20220116191717247](https://gitee.com/xxb667/img/raw/master/img/image-20220116191717247.png)

### 修改视图

- 修改视图是指修改数据库中**已存在的表**的定义。
- 当基本表的某些字段发生改变时，可以通过修改视图来保持视图和基本表之间一致。
- MySQL中通过**CREATE OR REPLACE VIEW**语句和**ALTER VIEW**语句来修改视图。

#### 格式

```
alter view 视图名 as select语句
```

#### 操作

```
alter view view1_emp
as 
select a.deptno,a.dname,a.loc,b.ename,b.sal from dept a, emp b where a.deptno = b.deptno;

```

![image-20220116193512400](https://gitee.com/xxb667/img/raw/master/img/image-20220116193512400.png)

### 更新视图

- 某些视图是可更新的。
- 也就是说，可以在UPDATE、DELETE或INSERT等语句中使用它们，以更新基表的内容。
- 对于可更新的视图，在视图中的行和基表中的行之间必须具有一对一的关系。
- 如果视图包含下述结构中的任何一种，那么它就是不可更新的：
  - 聚合函数（SUM(), MIN(), MAX(), COUNT()等）
  - DISTINCT
  - GROUP BY
  - HAVING
  - UNION或UNION ALL
  - 位于选择列表中的子查询
  - JOIN
  - FROM子句中的不可更新视图
  - WHERE子句中的子查询，引用FROM子句中的表。
  - 仅引用文字值（在该情况下，没有要更新的基本表）

- **注意：**
  - 视图中虽然可以更新数据，但是有很多的限制。
  - 一般情况下，最好将视图作为查询数据的虚拟表，而不要通过视图更新数据。
  - 因为，使用视图更新数据时，如果没有全面考虑在视图中更新数据的限制，就可能会造成数据更新失败。

#### 更新操作

```
-- 更新视图-------
create or replace view view1_emp
as 
select ename,job from emp;
 
update view1_emp set ename = '周瑜' where ename = '黄盖';  -- 可以修改

```

![image-20220116194636939](https://gitee.com/xxb667/img/raw/master/img/image-20220116194636939.png)

```
insert into view1_emp values('孙权','文员');  -- 不可以插入
```

![image-20220116194753500](https://gitee.com/xxb667/img/raw/master/img/image-20220116194753500.png)

#### 不可更新

##### 聚合函数不可更新

```
-- 视图包含聚合函数不可更新--------------
create or replace view view2_emp
as 
select count(*) cnt from emp;
 
insert into view2_emp values(100);

update view2_emp set cnt = 100; 
```

![image-20220116195221743](https://gitee.com/xxb667/img/raw/master/img/image-20220116195221743.png)

##### distinct不可更新

```
-- 视图包含distinct不可更新
create or replace view view3_emp
as 
select distinct job from emp;
 
insert into view3_emp values('财务');
 
```

![image-20220116195415593](https://gitee.com/xxb667/img/raw/master/img/image-20220116195415593.png)

##### goup by 或 having不可更新

```
-- 视图包含goup by 、having不可更新
 
create or replace view view4_emp
as 
select deptno ,count(*) cnt from emp group by deptno having  cnt > 2;
 
insert into view4_emp values(30,100);
```

![image-20220116195614525](https://gitee.com/xxb667/img/raw/master/img/image-20220116195614525.png)

##### union或union all不可更新

```
-- 视图包含union或者union all不可更新
create or replace view view5_emp
as 
select empno,ename from emp where empno <= 1005
union 
select empno,ename from emp where empno > 1005;
 
insert into view5_emp values(1015,'韦小宝');

```

![image-20220116195911793](https://gitee.com/xxb667/img/raw/master/img/image-20220116195911793.png)

##### 子查询不可更新

```
-- 视图包含子查询不可更新
create or replace view view6_emp
as 
select empno,ename,sal from emp where sal = (select max(sal) from emp);
 
insert into view6_emp values(1015,'韦小宝',30000);

```

![image-20220116200056109](https://gitee.com/xxb667/img/raw/master/img/image-20220116200056109.png)

##### join不可更新

```
-- 视图包含join不可更新
create or replace view view7_emp
as 
select dname,ename,sal from emp a join  dept b  on a.deptno = b.deptno;
 
insert into view7_emp(dname,ename,sal) values('行政部','韦小宝',30000);
 
```

![image-20220116200326766](https://gitee.com/xxb667/img/raw/master/img/image-20220116200326766.png)

##### 常量不可更新

```
-- 视图包含常量文字值不可更新
create or replace view view8_emp
as 
select '行政部' dname,'杨过'  ename;
 
insert into view8_emp values('行政部','韦小宝');
```

![image-20220116200525167](https://gitee.com/xxb667/img/raw/master/img/image-20220116200525167.png)

#### 其他操作

##### 重命名视图

```
-- rename table 视图名 to 新视图名;

rename table view1_emp to my_view1


-- 查看
show full tables;
```

![image-20220116210451037](https://gitee.com/xxb667/img/raw/master/img/image-20220116210451037.png)

##### 删除视图

```
-- drop view 视图名[,视图名…];
drop view if exists view_student;

-- 查看
show full tables;
```

![image-20220116210615904](https://gitee.com/xxb667/img/raw/master/img/image-20220116210615904.png)

**注意：**

- 删除视图时
- ==只能删除视图的定义==
- 不会删除数据。

## 练习

1：查询部门平均薪水最高的部门名称

```
-- 1：查询部门平均薪水最高的部门名称
select dname from dept  a ,(select deptno,avg(sal) from emp group by deptno order by avg(sal) desc limit 1) b
where a.deptno = b.deptno;  
 
```

![image-20220116200801859](https://gitee.com/xxb667/img/raw/master/img/image-20220116200801859.png)

2：查询员工比所属领导薪资高的部门名、员工名、员工领导编号

1. 查询员工比领导薪资高的部门号
2. 将第1步查询出来的部门号和部门表进行联表查询

```
-- 2：查询员工比所属领导薪资高的部门名、员工名、员工领导编号
select * from dept x,
(select a.ename aname ,a.sal asal,b.ename bname,b.sal bsal,a.deptno
from emp a, emp b 
where a.mgr = b.empno and a.sal > b.sal) y
where x.deptno = y.deptno;
```

![image-20220116201423656](https://gitee.com/xxb667/img/raw/master/img/image-20220116201423656.png)

3：查询工资等级为4级，2000年以后入职的工作地点为北京的员工编号、姓名和工资，并查询出薪资在前三名的员工信息

```
-- 3：查询工资等级为4级，2000年以后入职的工作地点为北京的员工编号、姓名和工资，并查询出薪资在前三名的员工信息
create view xxx
as       
SELECT e.empno,e.ename,e.sal,e.hiredate
FROM emp e,dept d,salgrade s
WHERE (e.sal BETWEEN  losal AND hisal) AND s.GRADE = 4
AND year(e.hiredate) > '2000'
AND d.loc = '北京';
 
select * from 
(
select 
 *,
 dense_rank() over(order by sal desc ) rn
from xxx
) t
where t.rn <=3;

```

![image-20220116205539308](https://gitee.com/xxb667/img/raw/master/img/image-20220116205539308.png)


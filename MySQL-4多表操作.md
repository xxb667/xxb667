# MySQL-4多表操作

# 多表关系

## 一对一

- 在任一表中添加唯一外键，指向另一方主键，确保一对一关系。
- 一个学生只有一张身份证；一张身份证只能对应一学生。
- 一般一对一关系很少见，遇到一对一关系的表最好是合并表。

![image-20220103203650803](https://gitee.com/xxb667/img/raw/master/img/image-20220103203650803.png)

## 一对多/多对一

- **实现原则**：在多的一方建立外键，指向一的一方的主键

- 部门和员工

  - 分析：
  - 一个部门有多个员工，
  - 一个员工只能对应一个部门

  

![image-20220103203604180](https://gitee.com/xxb667/img/raw/master/img/image-20220103203604180.png)

## 多对多

- **原则**：多对多关系实现需要借助第三张中间表。
  - 中间表至少包含**两个字段**
    - 将多对多的关系
    - 拆成一对多的关系
  - 中间表至少要有**两个外键**
  - 这两个外键分别指向原来的那两张表的主键
- 学生和课程
  - 分析：
  - 一个学生可以选择很多门课程，
  - 一个课程也可以被很多学生选择

![image-20220103204128358](https://gitee.com/xxb667/img/raw/master/img/image-20220103204128358.png)

# 外键约束

## 介绍

- MySQL 外键约束（FOREIGN KEY）是表的一个特殊字段，
  - 经常与主键约束一起使用。
- 对于两个具有关联关系的表而言，
  - 相关联字段中主键所在的表就是主表（父表），
  - 外键所在的表就是从表（子表）。
- **外键**用来建立主表与从表的关联关系，
  - 为两个表的数据建立连接，
  - 约束两个表中数据的**一致性**和**完整性**。
  - 比如：
    - 一个水果摊，只有苹果、桃子、李子、西瓜等 4 种水果，
    - 那么，你来到水果摊要买水果就只能选择苹果、桃子、李子和西瓜，
    - 其它的水果都是不能购买的。

![image-20220103204719420](https://gitee.com/xxb667/img/raw/master/img/image-20220103204719420.png)

## 特点

### 定义外键时，遵守下列规则

- 主表必须已经存在于数据库中
  - 或者是当前正在创建的表。
- 必须为主表定义主键。
-  主键不能包含空值，但允许在外键中出现空值。
  - 也就是说，只要外键的每个非空值出现在指定的主键中
  - 这个外键的内容就是正确的。
-  在主表的表名后面指定列名或列名的组合。
  - 这个列或列的组合必须是主表的主键或候选键。
- 外键中列的数目必须和主表的主键中列的数目相同。
- 外键中列的数据类型必须和主表主键中对应列的数据类型相同。

## 操作-创建外键约束

### 方式1-在创建表时设置外键约束

在 create table 语句中，通过 **foreign key** 关键字来指定外键，具体的语法格式如下：

```
[constraint <外键名>] foreign key 字段名 [，字段名2，…] references <主表名> 主键列1 [，主键列2，…]

```

#### 实现

```
create database mydb3;  -- 创建数据库
use mydb3;
-- 创建部门表
create table if not exists dept(
  deptno varchar(20) primary key ,  -- 部门号
  name varchar(20) -- 部门名字
);

```

![image-20220103210128387](https://gitee.com/xxb667/img/raw/master/img/image-20220103210128387.png)

```
-- 创建员工表
create table if not exists emp(
  eid varchar(20) primary key , -- 员工编号
  ename varchar(20), -- 员工名字
  age int,  -- 员工年龄
  dept_id varchar(20),  -- 员工所属部门
  constraint emp_fk foreign key (dept_id) references dept (deptno) –- 外键约束
);

```

![image-20220103210244685](https://gitee.com/xxb667/img/raw/master/img/image-20220103210244685.png)

两个表的关系如下

点击【文件】--【新架构设计器】--把表拖进来，就可以看到**主外键关系图**

![image-20220103212231263](https://gitee.com/xxb667/img/raw/master/img/image-20220103212231263.png)

### 方式2-在创建后设置外键约束

- 外键约束也可以在修改表时添加，
- 但是添加外键约束的前提是：
  - 从表中外键列中的数据必须与主表中主键列中的数据一致
  - 或者是没有数据。

#### 格式

```
alter table <数据表名> add constraint <外键名> foreign key(<列名>) references 
<主表名> (<列名>);

```

#### 实现

```
-- 创建部门表
create table if not exists dept2(
  deptno varchar(20) primary key ,  -- 部门号
  name varchar(20) -- 部门名字
);
-- 创建员工表
create table if not exists emp2(
  eid varchar(20) primary key , -- 员工编号
  ename varchar(20), -- 员工名字
  age int,  -- 员工年龄
  dept_id varchar(20)  -- 员工所属部门
 
);


```

![image-20220103213518216](https://gitee.com/xxb667/img/raw/master/img/image-20220103213518216.png)

```
-- 创建外键约束
alter table emp2 add constraint dept_id_fk foreign key(dept_id) references dept2 (deptno);
```

![image-20220103213704657](https://gitee.com/xxb667/img/raw/master/img/image-20220103213704657.png)



![image-20220103213741490](https://gitee.com/xxb667/img/raw/master/img/image-20220103213741490.png)

## 数据插入

**添加主表数据**

```
 -- 1、添加主表数据
 -- 注意必须先给主表添加数据
insert into dept values('1001','研发部');
insert into dept values('1002','销售部');
insert into dept values('1003','财务部');
insert into dept values('1004','人事部');

```

![image-20220103214041230](https://gitee.com/xxb667/img/raw/master/img/image-20220103214041230.png)

**添加从表数据**

```
-- 2、添加从表数据
  -- 注意给从表添加数据时，外键列的值不能随便写，必须依赖主表的主键列
insert into emp values('1','乔峰',20, '1001');
insert into emp values('2','段誉',21, '1001');
insert into emp values('3','虚竹',23, '1001');
insert into emp values('4','阿紫',18, '1002');
insert into emp values('5','扫地僧',35, '1002');
insert into emp values('6','李秋水',33, '1003');
insert into emp values('7','鸠摩智',50, '1003'); 
insert into emp values('8','天山童姥',60, '1005');  -- 不可以

```

![image-20220103214144426](https://gitee.com/xxb667/img/raw/master/img/image-20220103214144426.png)

## 删除数据

```
-- 3、删除数据
 /*
   注意：
       1：主表的数据被从表依赖时，不能删除，否则可以删除
       2: 从表的数据可以随便删除
 */
delete from dept where deptno = '1001'; -- 不可以删除

```

![image-20220103214408261](https://gitee.com/xxb667/img/raw/master/img/image-20220103214408261.png)

```
delete from dept where deptno = '1004'; -- 可以删除
delete from emp where eid = '7'; -- 可以删除
```

![image-20220103214506596](https://gitee.com/xxb667/img/raw/master/img/image-20220103214506596.png)

## 删除外键约束

当一个表中不需要外键约束时，就需要从表中将其删除。外键一旦删除，就会解除主表和从表间的关联关系

### 格式

```
alter table <表名> drop foreign key <外键约束名>;
```

### 实现

```
alter table emp2 drop foreign key dept_id_fk;
```

![image-20220103214717683](https://gitee.com/xxb667/img/raw/master/img/image-20220103214717683.png)

# 多表联合查询

## 介绍

多表查询就是同时查询两个或两个以上的表，因为有时候我们在查看数据的时候,需要显示的数据来自多张表.

## 数据准备

### 注

**外键约束**对多表查询并**无影响**

### 创建部门表

```
use mydb3;

-- 创建部门表
create table if not exists dept3(
  deptno varchar(20) primary key ,  -- 部门号
  name varchar(20) -- 部门名字
);
 
```

### 创建员工表

```
use mydb3;

-- 创建员工表
create table if not exists emp3(
  eid varchar(20) primary key , -- 员工编号
  ename varchar(20), -- 员工名字
  age int,  -- 员工年龄
  dept_id varchar(20)  -- 员工所属部门
);
```

### 准备查询数据

#### 向部门表（dept3）中插入数据

```
-- 给dept3表添加数据
insert into dept3 values('1001','研发部');
insert into dept3 values('1002','销售部');
insert into dept3 values('1003','财务部');
insert into dept3 values('1004','人事部');
 
```

![image-20220106205031650](https://gitee.com/xxb667/img/raw/master/img/image-20220106205031650.png)

#### 向员工表（emp3）中插入数据

```
-- 给emp3表添加数据
insert into emp3 values('1','乔峰',20, '1001');
insert into emp3 values('2','段誉',21, '1001');
insert into emp3 values('3','虚竹',23, '1001');
insert into emp3 values('4','阿紫',18, '1001');
insert into emp3 values('5','扫地僧',85, '1002');
insert into emp3 values('6','李秋水',33, '1002');
insert into emp3 values('7','鸠摩智',50, '1002'); 
insert into emp3 values('8','天山童姥',60, '1003');
insert into emp3 values('9','慕容博',58, '1003');
insert into emp3 values('10','丁春秋',71, '1005');

```

![image-20220106205147149](https://gitee.com/xxb667/img/raw/master/img/image-20220106205147149.png)

## 多表查询分类

- 交叉连接查询
- 内连接查询
- 外连接查询
- 子查询
- 表自关联

### 交叉连接查询（产生笛卡儿积）

- 交叉连接查询返回被连接的两个表所有数据行的**笛卡尔积**
- 笛卡尔积可以理解为一张表的每一行去和另外一张表的任意一行进行匹配（**两表行间匹配**）
- 假如A表有m行数据，B表有n行数据，则返回m*n行数据
- 笛卡尔积会产生很多冗余的数据，后期的其他查询可以在该集合的基础上进行条件筛选

#### 语法

```
select * from 表1,表2...;
```

#### 实现

```
-- 交叉连接查询

select * from dept3,emp3;

```

![image-20220106205254727](https://gitee.com/xxb667/img/raw/master/img/image-20220106205254727.png)



### 内连接查询

内连接查询求多张表的交集

![image-20220106205350163](https://gitee.com/xxb667/img/raw/master/img/image-20220106205350163.png)

使用关键字**inner join** -inner可以省略

#### 隐式内连接

```
select * from A,B where 条件;
```

#### 显示内连接

```
select * from A inner join B on 条件;
```

#### 操作

```
-- 查询每个部门的所属员工
-- 隐式内连接
select * from dept3,emp3 where dept3.deptno = emp3.dept_id;

-- 显示内连接
select * from dept3 inner join emp3 on dept3.deptno = emp3.dept_id;

```

![image-20220106205807915](https://gitee.com/xxb667/img/raw/master/img/image-20220106205807915.png)



![image-20220106205927222](https://gitee.com/xxb667/img/raw/master/img/image-20220106205927222.png)

```
-- 查询研发部和销售部的所属员工
-- 隐式内连接
select * from dept3,emp3 where dept3.deptno = emp3.dept_id and name in( '研发部','销售部');

-- 显示内连接
select * from dept3 join emp3 on dept3.deptno = emp3.dept_id and name in( '研发部','销售部');
 

```

![image-20220106210236714](https://gitee.com/xxb667/img/raw/master/img/image-20220106210236714.png)

![image-20220106210357954](https://gitee.com/xxb667/img/raw/master/img/image-20220106210357954.png)

```
-- 查询每个部门的员工数,并升序排序
-- 隐式内连接
select deptno,count(1) as total_cnt from dept3,emp3 where dept3.deptno = emp3.dept_id group by deptno order by total_cnt;

-- 显示内连接
select deptno,count(1) as total_cnt from dept3 join emp3 on dept3.deptno = emp3.dept_id group by deptno order by total_cnt;

注意：count(1)中的【1】可以用name代替，1只是数据库中表的列的顺序
```

![image-20220106210801283](https://gitee.com/xxb667/img/raw/master/img/image-20220106210801283.png)

![image-20220106210826414](https://gitee.com/xxb667/img/raw/master/img/image-20220106210826414.png)



![image-20220106211048145](https://gitee.com/xxb667/img/raw/master/img/image-20220106211048145.png)

```
-- 查询人数大于等于3的部门，并按照人数降序排序
-- 隐式内连接
select deptno,count(1) as total_cnt from dept3,emp3 where dept3.deptno = emp3.dept_id group by deptno having total_cnt >= 3 order by total_cnt desc;

-- 显示内连接
select deptno,count(1) as total_cnt from dept3 join emp3 on dept3.deptno = emp3.dept_id group by deptno having total_cnt >= 3 order by total_cnt desc;

```

![image-20220106211336333](https://gitee.com/xxb667/img/raw/master/img/image-20220106211336333.png)



![image-20220106211541944](https://gitee.com/xxb667/img/raw/master/img/image-20220106211541944.png)

### 外连接查询

#### 分类

- 左外连接
- 右外连接
- 满外连接

使用关键字**outer join** --outer 可以省略

#### 左外连接：left outer join

以左边的表为主，左边表的信息全部都有

```
select * from A left outer join B on 条件;
```

![image-20220106211651203](https://gitee.com/xxb667/img/raw/master/img/image-20220106211651203.png)

##### 操作

```
-- 外连接查询
-- 查询哪些部门有员工，哪些部门没有员工
use mydb3;
select * from dept3 left outer join emp3 on dept3.deptno = emp3.dept_id;
 
```

![image-20220106211946807](https://gitee.com/xxb667/img/raw/master/img/image-20220106211946807.png)





#### 右外连接：right outer join

以右边的表为主，右边表的信息全部都有

```
select * from A right outer join B on 条件;
```

![image-20220106211702768](https://gitee.com/xxb667/img/raw/master/img/image-20220106211702768.png)

##### 操作

```
-- 查询哪些员工有对应的部门，哪些没有
select * from dept3 right outer join emp3 on dept3.deptno = emp3.dept_id;
```

![image-20220106212138343](https://gitee.com/xxb667/img/raw/master/img/image-20220106212138343.png)



#### 满外连接：full outer join

两个表的信息都有

```
select * from A full outer join B on 条件;
```

![image-20220106211713840](https://gitee.com/xxb667/img/raw/master/img/image-20220106211713840.png)

##### 操作

```
-- 使用union关键字实现左外连接和右外连接的并集
select * from dept3 left outer join emp3 on dept3.deptno = emp3.dept_id
union 
select * from dept3 right outer join emp3 on dept3.deptno = emp3.dept_id;
```

![image-20220106212959825](https://gitee.com/xxb667/img/raw/master/img/image-20220106212959825.png)

### 子查询

#### 介绍

- 子查询就是指的在一个完整的查询语句之中，嵌套若干个不同功能的小查询，
- 从而一起完成复杂查询的一种编写形式，
- 通俗一点就是包含select嵌套的查询。

#### 特点

子查询返回数据类型4种

**1.单行单列：**

- 返回的是一个具体列的内容，
- 可以理解为一个单值数据；

**2.单行多列：**

​	返回一行数据中多个列的内容；

**3.多行单列：**

- 返回多行记录之中同一列的内容，
-  相当于给出了一个操作范围；

**4.多行多列：**

​	查询返回的结果是一张临时表

#### 操作

```
-- 查询年龄最大的员工信息，显示信息包含员工号、员工名字，员工年龄
select eid,ename,age from emp3 where age = (select max(age) from emp3);
 
 
-- 查询年研发部和销售部的员工信息，包含员工号、员工名字
select eid,ename,t.name from emp3 where dept_id in (select deptno,name from dept3 where name = '研发部' or name = '销售部') ;
 
 
-- 查询研发部20岁以下的员工信息,包括员工号、员工名字，部门名字
select eid,age,ename,name from (select * from dept where name = '研发部 ')t1,(select * from emp3 where age <20)t2

```

#### 子查询中的关键字

##### ALL关键字

###### 格式

```
select ... from ... where c>all（查询语句）
-- 等价于
select ... from ... where c>result1 and c>result2 and c>result3;
```

###### 特点

- ALL: 
  - 与子查询返回的所有值比较为true 则返回true
- ALL可以与**=**、**>**、**>=**、**<**、**<=**、**<>**结合是来使用，
  - 分别表示等于、大于、大于等于、小于、小于等于、不等于其中的其中的所有数据。
- ALL表示指定列中的值必须要大于子查询集的每一个值，
  - 即必须要大于子查询集的最大值
  - 如果是小于号即小于子查询集的最小值。
  - 同理可以推出其它的比较运算符的情况。

###### 操作

```
-- 查询年龄大于‘1003’部门所有年龄的员工信息
select * from emp3 where age > all(select age from emp3 where dept_id = '1003’);

```

![image-20220107195325885](https://gitee.com/xxb667/img/raw/master/img/image-20220107195325885.png)

```
-- 查询不属于任何一个部门的员工信息 
select * from emp3 where dept_id != all(select deptno from dept3); 

```

![image-20220107195441286](https://gitee.com/xxb667/img/raw/master/img/image-20220107195441286.png)

##### ANY关键字和SOMY关键字

###### 格式

```
select ... from ... where c>any(查询语句)
--等价于
select ... from ... where c>result1 or c>result2 or c>result3;
```



###### 特点

- ANY:与子查询返回的任何值比较为true 则返回true
- ANY可以与=、>、>=、<、<=、<>结合是来使用
  - 分别表示等于、大于、大于等于、小于、小于等于、不等于其中的其中的任何一个数据。
- 表示制定列中的值要大于子查询中的任意一个值
  - 即必须要大于子查询集中的最小值。
  - 同理可以推出其它的比较运算符的情况。
- SOME和ANY的作用一样
  - SOME可以理解为ANY的别名

###### 操作

```
-- 查询年龄大于‘1003’部门任意一个员工年龄的员工信息
select * from emp3 where age > all(select age from emp3 where dept_id = '1003’);

```

![image-20220107195754475](https://gitee.com/xxb667/img/raw/master/img/image-20220107195754475.png)



##### IN关键字

###### 格式

```
select ... from ... where c in(查询语句)
-- 等价于
select ... from ... where c=result1 or c=result2 or c=result3;
```



###### 特点

- IN关键字，用于判断某个记录的值，是否在指定的集合中
- 在IN关键字前边加上not可以将条件反过来

###### 操作

```
-- 查询研发部和销售部的员工信息，包含员工号、员工名字
select eid,ename from emp3 where dept_id in (select deptno from dept3 where name = '研发部' or name = '销售部') ;
 
```

![image-20220107200001805](https://gitee.com/xxb667/img/raw/master/img/image-20220107200001805.png)

##### EXISTS关键字

###### 格式

```
select ... from ... where exists(查询语句)
```



###### 特点

- 该子查询如果“有数据结果”(至少返回一行数据)， 
  - 则该EXISTS() 的结果为“true”
  - 外层查询执行
- 该子查询如果“没有数据结果”（没有任何数据返回），
  - 则该EXISTS()的结果为“false”
  - 外层查询不执行
- EXISTS后面的子查询不返回任何实际数据，
  - 只返回真或假
  - 当返回真时 where条件成立
- **注意**，EXISTS关键字，比IN关键字的运算效率高
  - 因此，在实际开发中，特别是大数据量时，推荐使用EXISTS关键字

###### 操作

```
-- 查询公司是否有大于60岁的员工，有则输出
select * from emp3 a where exists(select * from emp3 b where a.age > 60);
 
```

![image-20220107200407895](https://gitee.com/xxb667/img/raw/master/img/image-20220107200407895.png)

```
-- 查询有所属部门的员工信息
select * from emp3 a where exists(select * from dept3 b where a.dept_id = b.deptno);
```

![image-20220107200730244](https://gitee.com/xxb667/img/raw/master/img/image-20220107200730244.png)

### 表自关联

###### 概念

- MySQL有时在信息查询时需要进行对表自身进行关联查询，
- 即一张表自己和自己关联，一张表当成多张表来用。
- 注意自关联时表必须给表起别名。

###### 格式

```
select 字段列表 from 表1 a, 表1 b where 条件;
或
select 字段列表 from 表1 a [left] join 表1 b on 条件;
```



###### 操作

创建表

```
-- 创建表,并建立自关联约束
use mydb3;
create table t_sanguo(
    eid int primary key ,
    ename varchar(20),
    manager_id int,
 foreign key (manager_id) references t_sanguo (eid)  -- 添加自关联约束
);
```

添加数据

```
-- 添加数据 
insert into t_sanguo values(1,'刘协',NULL);
insert into t_sanguo values(2,'刘备',1);
insert into t_sanguo values(3,'关羽',2);
insert into t_sanguo values(4,'张飞',2);
insert into t_sanguo values(5,'曹操',1);
insert into t_sanguo values(6,'许褚',5);
insert into t_sanguo values(7,'典韦',5);
insert into t_sanguo values(8,'孙权',1);
insert into t_sanguo values(9,'周瑜',8);
insert into t_sanguo values(10,'鲁肃',8);
 
```

进行关联查询

```
-- 进行关联查询
-- 1.查询每个三国人物及他的上级信息，如:  关羽  刘备 
select * from t_sanguo a, t_sanguo b where a.manager_id = b.eid;
```

![image-20220107201629363](https://gitee.com/xxb667/img/raw/master/img/image-20220107201629363.png)

这节我们介绍了多表操作中的各种操作，包括多表关系、外键约束、多表联合查询、子查询和自关联查询等


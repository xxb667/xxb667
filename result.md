# MySql基本操作

@[toc]

## 登录mysql

### windows下的操作

#### 修改登录密码

##### 方法1

-  这个在打开win+R，

- 输入cmd后输入的指令

```
 mysqladmin -uroot -p123456 password root
 #格式为 mysqladmin -u用户名 -p旧密码 password 新密码

```

出现一个警告信息，但是可以使用新密码登录

```
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.
```

```
Mysqladmin: [警告]在命令行界面上使用密码可能不安全。

警告: 由于密码将以纯文本形式发送到服务器，因此使用 ssl 连接确保密码安全
```

![image-20211215204457708](https://gitee.com/xxb667/img/raw/master/img/image-20211215204457708.png)

##### 方法2

首先登录进去数据库

###### 登录数据库

```
mysql -uroot -proot #登陆数据库 我的账户是root 密码是123456
```

修改密码

```
set password for 'root'@'localhost'= password('123456');
 #格式为 set password for '用户名'@'localhost' = password('新密码');
```

出现报错信息，MySQL语法有错误

![image-20211215205142491](https://gitee.com/xxb667/img/raw/master/img/image-20211215205142491.png)

###### 解决方法

进入MySQL后使用下面语句

```
 alter user 'root'@'localhost' identified by '123456';
```

退出数据库重新进行登录，发现密码修改成功

![image-20211215205626591](https://gitee.com/xxb667/img/raw/master/img/image-20211215205626591.png)

登录后第1步操作

### 登录数据库

```
mysql -uroot -proot #登陆数据库 我的账户是root 密码是123456
```



### 查看数据库

```
 show databases; #查看所有的数据库
```

![image-20211215210004180](https://gitee.com/xxb667/img/raw/master/img/image-20211215210004180.png)

### 创建数据库

```
create database test;#创建名字为test的数据库
show databases; #查看当前是否有我们创建的数据库
use test;  #切换到test数据库；
show tables; #显示所有的表，目前还没创建表
drop database book;#删除数据库book
exit  # --退出连接
```

![image-20211215210803193](https://gitee.com/xxb667/img/raw/master/img/image-20211215210803193.png)



### 使用数据库

```
use test; #使用test数据库
```

![image-20211215211141790](https://gitee.com/xxb667/img/raw/master/img/image-20211215211141790.png)

#### 创建一个简单的表：

```
CREATE TABLE 表名( 
字段名称1 字段类型（长度），
字段名称2 字段类型 注意 最后一列不要加逗号 
);
```

```
#单行写
create table users(id int(10) not null,username varchar(50),password varchar(50) );


#多行写
create table users(
    				id int(10) not null,
                   username varchar(50),
                   password varchar(50) 
                  );

```

表创建成功后，查看

#### 查看表

```
show tables; #查看当前数据库下面的所有表
desc users; #显示users表的详细信息
```

![image-20211215211439938](https://gitee.com/xxb667/img/raw/master/img/image-20211215211439938.png)

#### 修改表:(不区分大小写)

##### 修改表名

```mysql
# 修改表名
#rename table 旧表名 to 新表名
RENAME TABLE users TO listname;

show tables; #查看是否修改成功
```

![image-20211215211800692](https://gitee.com/xxb667/img/raw/master/img/image-20211215211800692.png)

##### 向表中添加列

```
#向表中添加列， 关键字 ADD
#alert table 表名 add 字段名称 字段类型

# 为分类表添加一个新的字段为 分类描述 cdesc varchar(20)
ALTER TABLE listname ADD ip_address VARCHAR(20);

desc listname; #查看是否添加成功
```

![image-20211215212123596](https://gitee.com/xxb667/img/raw/master/img/image-20211215212123596.png)

##### 修改表中列的 数据类型或长度 

```
# 修改表中列的 数据类型或长度 ， 关键字 MODIFY
#alter table 表名 modify 字段名称 字段类型
ALTER TABLE listname MODIFY ip_address VARCHAR(50);

desc listname; #查看是否修改成功
```

![image-20211215212416747](https://gitee.com/xxb667/img/raw/master/img/image-20211215212416747.png)

##### 修改列名称

```
# 修改列名称 , 关键字 CHANGE
#alter table 表名 change 旧列名 新列名 类型(长度);
ALTER TABLE listname CHANGE ip_address IP_addre VARCHAR(30);

desc listname; #查看是否修改成功
```

![image-20211215212714863](https://gitee.com/xxb667/img/raw/master/img/image-20211215212714863.png)

##### 删除列

```
# 删除列 ，关键字 DROP
#alter table 表名 drop 列名;
ALTER TABLE listname DROP IP_addre;

desc listname; #查看是否删除成功
```

![image-20211215212933434](https://gitee.com/xxb667/img/raw/master/img/image-20211215212933434.png)

#### 删除表：

```mysql
DROP TABLE test1;  删除名字为 test1的表

DROP TABLE IF EXISTS test2;# 先判断表是否存在，存在的话再删除test2
#加上 if exists 如果没有这个表，会生成一个消息告诉说没有，会继续往下走。   

#不加if exists 的话  那么databases中要是没有的话，就会出错误。不会往下执行。

show tables; #查看是否成功删除
```

![image-20211215213325786](https://gitee.com/xxb667/img/raw/master/img/image-20211215213325786.png)

#### 向表中添加数据

##### 1、插入全部字段

```
# 增加数据 格式为insert into 表名 （字段名1，字段名2...） values(字段值1，字段值2...);

#方式1： 插入全部字段， 将所有字段名都写出来
insert into listname(id,username,password) values(1,'小米','123');

select * from listname; #查询表中所有数据，看是否添加成功
```

![image-20211215213924194](https://gitee.com/xxb667/img/raw/master/img/image-20211215213924194.png)

##### 2、插入全部字段，不写字段名

```
#方式2： 插入全部字段，不写字段名
insert into listname values(2,'小明','234');

select * from listname; #查询表中所有数据，看是否添加成功
```

- 注意：

- 插入的数据一定要和字段的顺序和类型一致才可以

- 否则会报错

![image-20211215214249927](https://gitee.com/xxb667/img/raw/master/img/image-20211215214249927.png)

##### 3、插入指定字段的值

```
#方式3：插入指定字段的值
insert into listname(id) values(4);

select * from listname; #查询表中所有数据，看是否添加成功
```

![image-20211215214701536](https://gitee.com/xxb667/img/raw/master/img/image-20211215214701536.png)

##### 4、**注意事项：**

1）值与字段必须要对应，个数相同&数据类型相同
2）值的数据大小，必须在字段指定的长度范围内
3）varchar char date类型的值必须使用单引号，或者双引号 包裹
4）如果要插入空值，可以忽略不写，或者插入null
5）如果插入指定字段的值，必须要上写列名

#### 查询数据

```
select * from listname;
```

![image-20211215214931345](https://gitee.com/xxb667/img/raw/master/img/image-20211215214931345.png)

## 删除数据库内容

DELETE FROM <表名> WHERE 子句[LIMIT 子句]

```mysql
delete from books;#删除books所有数据   注意不等于 drop table books;

delete from books where bName="黑客攻防";#删除名字为黑客攻防的内容

delete from books where price >100;  #删除价格大于100 的内容

truncate table 表名 或者 truncate 表名  #情况数据库中表的数据
```

## 删除数据库

一般不建议删除！！

```
 drop database book; #结构 drop database 数据库名;
 
 show databases; #查看是否删除成功
```

![image-20211215215642403](https://gitee.com/xxb667/img/raw/master/img/image-20211215215642403.png)

## 导入数据库

```
create database book;  #新建一个数据库
use book; #使用这个数据库 
 source D:/serve/mysql8/book.sql  #把book.sql  导入 名字为book的数据库
show tables; #查看导入的数据库中包含的表
```

注意：路径名的斜杠【\】换成反斜杠【/】

![image-20211215215927812](https://gitee.com/xxb667/img/raw/master/img/image-20211215215927812.png)

![image-20211215220428147](https://gitee.com/xxb667/img/raw/master/img/image-20211215220428147.png)

![image-20211215220516886](https://gitee.com/xxb667/img/raw/master/img/image-20211215220516886.png)

## where 表达式** (按条件查询)

在MySQL的表查询时，往往并不是需要将所有内容全部查出，而是根据实际需求，查询所需数据

### 查询格式：

```
select 查询内容 from 表名 where 表达式;
```



在MySQL语句中，

**条件表达式**是指

- **select语句的查询条件**，

- 在where子句中,可以使用

- **关系运算符**连接操作数
  - 作为查询条件对数据进行选择。

| 逻辑运算符 | 说明               |
| ---------- | ------------------ |
| AND &&     | 多个条件同时成立时 |
| OR \|\|    | 多个条件任一成立   |
| NOT        | 不成立，取反       |

**注意：在查询时，and 的优先级高于 or**

**关系运算符：**
=  等于
<> 不等于
!= 不等于
<  小于
\>  大于
<= 小于等于
\>= 大于等于

在查询**具有一类相同特征**的数据时，需要用到**模糊查询**

### 模糊查询格式

```
select 查询内容 from 表名 where 内容 like ‘匹配的字符串’;
```

### 其他查询语句

```mysql
desc books; #显示表格详细信息
#查询bId为5的内容
select * from books where bId =5;

```

![image-20211215221206198](https://gitee.com/xxb667/img/raw/master/img/image-20211215221206198.png)

```
#查询publishing  出版社为清华大学出版社的内容
select * from books where publishing= "清华大学出版社";  #字符串要加引号
```

![image-20211215221355140](https://gitee.com/xxb667/img/raw/master/img/image-20211215221355140.png)

```
#查询价格小于50元的书
select * from books where price <50;
```

![image-20211215221545082](https://gitee.com/xxb667/img/raw/master/img/image-20211215221545082.png)

```
#查询 价格大于50 且 出版社为 人民邮电出版社 
select * from books where price >50 and publishing= "人民邮电出版社";

```

![image-20211215221634394](https://gitee.com/xxb667/img/raw/master/img/image-20211215221634394.png)

```
#查询 价格大于50 且 出版社为 人民邮电出版社 并且 书名中 有CAD 内容
select * from books where price >50 and publishing= "人民邮电出版社" and  bName like "%CAD%";
```

![image-20211215221709200](https://gitee.com/xxb667/img/raw/master/img/image-20211215221709200.png)

```
#查询出版社为清华大学 或者人民邮电的 书名和价格
select bName,price,publishing from books where publishing= "人民邮电出版社" or publishing= "清华大学出版社";
```

![image-20211215221742365](https://gitee.com/xxb667/img/raw/master/img/image-20211215221742365.png)

**对查询结果进行排序**

### 查询结果排序格式

```
 **select 查询内容 from 表名 order by 排序条件 asc/desc，**asc表示升序 desc表示降序  
 
```

 

### 限制查询：

  在查询时，可能需要只显示部分数据，这是需要限制查出来的数据数量

#### 格式

```
 **select 查询内容 from 表名 limit 偏移量m 记录数n**，表示从第m+1个记录开始查询出n条记录
```

####  查询代码演示

```mysql
#查询所有图书价格按照从高到底排序
select * from books order by price desc ;

```

![image-20211215222144964](https://gitee.com/xxb667/img/raw/master/img/image-20211215222144964.png)

```
#查询所有图书价格按照从高到底排序
select * from books order by price asc ;

```

![image-20211215222250981](https://gitee.com/xxb667/img/raw/master/img/image-20211215222250981.png)

```
#查询所有图书价格按照从低到高排序，限制显示一条  也就是价格最低的一本书
select * from books order by price asc  limit 0,1;

```

![image-20211215222425234](https://gitee.com/xxb667/img/raw/master/img/image-20211215222425234.png)

```
#查询所有图书价格按照从高到底排序，限制显示三条  也就是价格第二高到第四高的内容
select * from books order by price desc  limit 1,3;
```

![image-20211215222515578](https://gitee.com/xxb667/img/raw/master/img/image-20211215222515578.png)

```
select * from books order by 1 ;
select * from books order by 3 ;
select * from books order by 9 ;
#by n，按第n个变量进行排序
```

![image-20211215222631655](https://gitee.com/xxb667/img/raw/master/img/image-20211215222631655.png)

![image-20211215222723305](https://gitee.com/xxb667/img/raw/master/img/image-20211215222723305.png)

## 修改数据库内容

使用 UPDATE 语句修改单个表，语法

### 格式

```
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ][ORDER BY 子句] [LIMIT 子句]

```

演示

```mysql
修改 表为book的 价格为50 当bId 为3的时候
update books set price=50 where bId=3;

select * from books where bId=3;#查看信息
```

![image-20211215223027504](https://gitee.com/xxb667/img/raw/master/img/image-20211215223027504.png)

## 添加数据

```mysql
insert into books values(45,"黑客攻防",3,"中国工业出版社",98,"2021-01-01","齐鹏",7121008947);
select * from books where bId=45;#查看插入的数据
#上面有介绍 不在赘述
```

![image-20211215223208197](https://gitee.com/xxb667/img/raw/master/img/image-20211215223208197.png)

## 删除数据库内容

DELETE FROM <表名> WHERE 子句[LIMIT 子句]

```
delete from books where bName="黑客攻防";#删除名字为黑客攻防的内容
select * from books where bName="黑客攻防"; #查看信息
```

![image-20211215223806959](https://gitee.com/xxb667/img/raw/master/img/image-20211215223806959.png)

```
delete from books where price >100;  #删除价格大于100 的内容
```

![image-20211215224011435](https://gitee.com/xxb667/img/raw/master/img/image-20211215224011435.png)

```mysql

delete from books;#删除books所有数据   注意不等于 drop table books;
 select * from books;#查看表中数据
```

![image-20211215223421943](https://gitee.com/xxb667/img/raw/master/img/image-20211215223421943.png)

## 导出整个数据库

### Windows下

首先打开cmd

WIN+R

输入cmd进入

```
mysqldump -u 用户名 -p 数据库名 > 导出的文件名
#注意这个是在cmd指令界面
```

```
mysqldump -uroot -p book > D:\serve\book.sql
#写入后会提示输入密码，也就是数据库的密码
```

![image-20211216105021363](https://gitee.com/xxb667/img/raw/master/img/image-20211216105021363.png)

查看路径是否有这个book.sql文件

![image-20211216105118414](https://gitee.com/xxb667/img/raw/master/img/image-20211216105118414.png)



简单的mysql基础使用大概就是这些。如果有不了解的内容，记得call me。

[@toc]

# MySql中的约束

从这里开始使用SQLyog工具对MySQL进行操作,下面操作都是在有数据库mydb1下进行建立的

```
create database mydb1; -- 创建mydb1数据库
```



## 作用

- 表在设计的时候加入约束的目的就是为了保证表中的记录完整性和有效性。

- 比如用户表有些列的值（手机号）不能为空，有些列的值（身份证号）不能重复。

  

## 分类

- 主键约束(primary key) PK
- 自增长约束(auto_increment)
- 非空约束(not null)
- 唯一性约束(unique)
- 默认约束(default)
- 零填充约束(zerofill)
- 外键约束(foreign key) FK

# 主键约束(primary key) PK

## 概念

- MySQL主键约束是一个列或者多个列的组合，其值能唯一地标识表中的每一行,方便在RDBMS中尽快的找到某一行。
- 主键约束相当于 **唯一约束 + 非空约束** 的组合，主键约束列不允许重复，也不允许出现空值。
- 每个表最多**只允许一个主键**
- 主键约束的关键字是：**primary key**
- 当创建主键的约束时，系统默认会在所在的列和列组合上建立对应的唯一索引。

## 操作

### 添加单列主键

- 两种方式
  1. 在定义字段时同时指定主键
  2. 定义完字段之后指定主键

#### 方法1-语法

```
-- 在 create table 语句中，通过 PRIMARY KEY 关键字来指定主键。
--在定义字段的同时指定主键，语法格式如下：
create table 表名(
   ...
   <字段名> <数据类型> primary key 
   ...
)
```

##### 方法1-实现

```
use mydb1;
create table emp1(
    eid int primay key, -- 主键
    name VARCHAR(20),
    deptId int,
    salary double
);

```

![image-20211230155343523](https://gitee.com/xxb667/img/raw/master/img/image-20211230155343523.png)

#### 方法2-语法

```
--在定义字段之后再指定主键，语法格式如下：
create table 表名(
   ...
   [constraint <约束名>] primary key [字段名]
);

```

##### 方法2-实现

```
use mydb1;
create table emp2(
    eid INT,
    name VARCHAR(20),
    deptId INT,
    salary double,
    constraint  pk1 primary key(eid)
 );
```

![image-20211230155914169](https://gitee.com/xxb667/img/raw/master/img/image-20211230155914169.png)



![image-20211230160057065](https://gitee.com/xxb667/img/raw/master/img/image-20211230160057065.png)

### 添加多列联合主键

- 联合主键，
  - 主键是由一张表中多个字段组成的。

**注意：**

1. 当主键是由多个字段组成时，不能直接在字段名后面声明主键约束。

2. 一张表只能有一个主键，联合主键也是**一个主键**

   

#### 语法

```
create table 表名(
   ...
   primary key （字段1，字段2，…,字段n)
);

```

#### 实现

```
use mydb1;
create table emp3( 
  name varchar(20), 
  deptId int, 
  salary double, 
  primary key(name,deptId) 
);

```

![image-20211230160520971](https://gitee.com/xxb667/img/raw/master/img/image-20211230160520971.png)

### 修改表结构添加主键

#### 语法

```
create table 表名(
   ...
);
alter table <表名> add primary key（字段列表);

```

#### 实现

```
-- 添加单列主键
use mydb1;
create table emp4(
  eid int, 
  name varchar(20), 
  deptId int, 
  salary double
);


```

首先查看创建的表

![image-20211230161129982](https://gitee.com/xxb667/img/raw/master/img/image-20211230161129982.png)

修改表之后

```
alter table emp4 add primary key(eid);
```

![image-20211230161430603](https://gitee.com/xxb667/img/raw/master/img/image-20211230161430603.png)



### 删除主键

#### 格式

```
alter table <数据表名> drop primary key;
```

#### 实现

```
-- 删除单列主键 
alter table emp1 drop primary key;
```

![image-20211230162343553](https://gitee.com/xxb667/img/raw/master/img/image-20211230162343553.png)

```
-- 删除联合主键 
alter table emp3 drop primary key;
```

![image-20211230162521359](https://gitee.com/xxb667/img/raw/master/img/image-20211230162521359.png)



# 自增长约束(auto_increment)

## 概念

- 在 MySQL 中，当主键定义为自增长后，

  - 这个主键的值就不再需要用户输入数据了，
  - 而由数据库系统根据定义自动赋值。
  - 每增加一条记录，主键会自动以相同的步长进行增长。

- 通过给字段添加 **auto_increment** 属性来实现主键自增长

  

## 语法

```
字段名 数据类型 auto_increment
```

## 操作

```
use mybd1;
create table t_user1( 
  id int primary key auto_increment, 
  name varchar(20) 
);
```

![image-20211230163013136](https://gitee.com/xxb667/img/raw/master/img/image-20211230163013136.png)



## 特点

- 默认情况下，**auto_increment**的初始值是 1，每新增一条记录，字段值自动加 1。

- 一个表中只能有一个字段使用 auto_increment约束，且该字段必须有唯一索引，以避免序号重复（即为主键或主键的一部分）。

- auto_increment约束的字段必须具备 **NOT NULL 属性**。

- auto_increment约束的字段只能是**整数类型**（TINYINT、SMALLINT、INT、BIGINT 等。

- auto_increment约束字段的最大值受该字段的数据类型约束，如果达到上限，auto_increment就会失效。

## 指定自增字段初始值

### 方法1-创建表时指定

```
-- 方式1，创建表时指定
use mydb1;
create table t_user2 ( 
  id int primary key auto_increment, 
  name varchar(20)
)auto_increment=100;
```

![image-20211230163534364](https://gitee.com/xxb667/img/raw/master/img/image-20211230163534364.png)



### 方法2-创建表之后指定

```
-- 方式2，创建表之后指定
use mydb1;
create table t_user3 ( 
  id int primary key auto_increment, 
  name varchar(20)
);


```

修改前

![image-20211230163722929](https://gitee.com/xxb667/img/raw/master/img/image-20211230163722929.png)

修改后

```
alter table t_user3 auto_increment=100;
```

![image-20211230163935946](https://gitee.com/xxb667/img/raw/master/img/image-20211230163935946.png)



### delete和truncate在删除后自增列的变化

- delete数据之后自动增长从断点开始
- truncate数据之后自动增长从默认起始值开始       #直接删除所有行了

![image-20211230164428057](https://gitee.com/xxb667/img/raw/master/img/image-20211230164428057.png)



# 非空约束(not null)

## 概念

- MySQL 非空约束（not null）指字段的值不能为空。
- 对于使用了非空约束的字段，如果用户在添加数据时没有指定值，数据库系统就会报错。

## 语法

```
方式1：<字段名><数据类型> not null;
方式2：alter table 表名 modify 字段 类型 not null;
```

### 添加非空约束-方式1创建时添加

```
-- 方式1，创建表时指定
use mydb1;
create table t_user6 ( 
  id int , 
  name varchar(20) not null, 
  address varchar(20) not null 
);

```

![image-20211230165200355](https://gitee.com/xxb667/img/raw/master/img/image-20211230165200355.png)

### 添加非空约束-方式2创建后添加

```
use mydb1;
create table t_user7 ( 
  id int , 
  name varchar(20) , -- 指定非空约束 
  address varchar(20) -- 指定非空约束 
); 
alter table t_user7 modify name varchar(20) not null; 
alter table t_user7 modify address varchar(20) not null;
```

![image-20211230165401217](https://gitee.com/xxb667/img/raw/master/img/image-20211230165401217.png)



![image-20211230165514210](https://gitee.com/xxb667/img/raw/master/img/image-20211230165514210.png)

## 删除非空约束

```
-- alter table 表名 modify 字段 类型 
alter table t_user7 modify name varchar(20) ; 
alter table t_user7 modify address varchar(20) ;

```

![image-20211230165706770](https://gitee.com/xxb667/img/raw/master/img/image-20211230165706770.png)



# 唯一性约束(unique)

## 概念

- 唯一约束（Unique Key）是指所有记录中字段的值不能重复出现。
- 例如，为 id 字段加上唯一性约束后，每条记录的 id 值都是唯一的，不能出现重复的情况。

## 语法

```
方式1：<字段名> <数据类型> unique
方式2： alter table 表名 add constraint 约束名 unique(列);
```

### 方式1-创建时添加

```
-- 创建表时指定
use mydb1;
create table t_user8 ( 
 id int , 
 name varchar(20) , 
 phone_number varchar(20) unique -- 指定唯一约束 
);

```

![image-20211230170100096](https://gitee.com/xxb667/img/raw/master/img/image-20211230170100096.png)



### 方式2-创建后添加

```
use mydb1;
create table t_user9 ( 
  id int , 
  name varchar(20) , 
  phone_number varchar(20) -- 指定唯一约束 
); 
alter table t_user9 add constraint unique_ph unique(phone_number);

```

![image-20211230170252784](https://gitee.com/xxb667/img/raw/master/img/image-20211230170252784.png)

## 删除唯一约束

```
-- alter table <表名> drop index <唯一约束名>;
alter table t_user9 drop index unique_ph;
```

![image-20211230170444673](https://gitee.com/xxb667/img/raw/master/img/image-20211230170444673.png)



# 默认约束(default)

## 概念

MySQL默认值约束用来指定某列的默认值

## 语法

```
方式1： <字段名> <数据类型> default <默认值>;
方式2: alter table 表名 modify 列名 类型 default 默认值; 
```

### 方式1-在创建时添加

```
use mydb1;
create table t_user10 ( 
  id int , 
  name varchar(20) , 
  address varchar(20) default ‘北京’ -- 指定默认约束 
);

```

![image-20211230170840065](https://gitee.com/xxb667/img/raw/master/img/image-20211230170840065.png)

### 方式2-创建后添加

```
-- alter table 表名 modify 列名 类型 default 默认值; 
use mydb1;
create table t_user11 ( 
  id int , 
  name varchar(20) , 
  address varchar(20)  
);
alter table t_user11 modify address varchar(20) default  ‘北京’;

```

![image-20211230171053688](https://gitee.com/xxb667/img/raw/master/img/image-20211230171053688.png)

![image-20211230171202402](https://gitee.com/xxb667/img/raw/master/img/image-20211230171202402.png)

## 删除默认约束

```
-- alter table <表名> modify column <字段名> <类型> default null; 

alter table t_user11 modify column address varchar(20) default null;

```

![image-20211230171252535](https://gitee.com/xxb667/img/raw/master/img/image-20211230171252535.png)



# 零填充约束(zerofill)

## 概念

- 插入数据时，当该字段的值的长度小于定义的长度时，会在该值的前面**补上相应的0**
- zerofill默认为**int(10)**
- 当使用zerofill 时，默认会自动加unsigned（无符号）属性，
  - 使用unsigned属性后，数值范围是原值的2倍，
  - 例如，有符号为-128~+127，无符号为0~256。

## 操作

```
use mydb1;
create table t_user12 ( 
  id int zerofill , -- 零填充约束
  name varchar(20)   
);
```

![image-20211230171717381](https://gitee.com/xxb667/img/raw/master/img/image-20211230171717381.png)

## 删除

```
alter table t_user12 modify id int;
```

![image-20211230171817857](https://gitee.com/xxb667/img/raw/master/img/image-20211230171817857.png)



@[toc]

# MySQL基本查询

## 数据准备

### 创建数据库和表

```
-- 创建数据库
create database if not exists mydb2;
use mydb2;
-- 创建商品表：
create table product(
 pid int primary key auto_increment, -- 商品编号
 pname varchar(20) not null , -- 商品名字
 price double,  -- 商品价格
 category_id varchar(20) -- 商品所属分类
);

```

![image-20211231133602988](https://gitee.com/xxb667/img/raw/master/img/image-20211231133602988.png)



### 添加数据

```
insert into product values(null,'海尔洗衣机',5000,'c001');
insert into product values(null,'美的冰箱',3000,'c001');
insert into product values(null,'格力空调',5000,'c001');
insert into product values(null,'九阳电饭煲’,200,'c001');

insert into product values(null,'啄木鸟衬衣',300,'c002');
insert into product values(null,'恒源祥西裤',800,'c002');
insert into product values(null,'花花公子夹克',440,'c002');
insert into product values(null,'劲霸休闲裤',266,'c002');
insert into product values(null,'海澜之家卫衣',180,'c002');
insert into product values(null,'杰克琼斯运动裤',430,'c002');
 
insert into product values(null,'兰蔻面霜',300,'c003');
insert into product values(null,'雅诗兰黛精华水',200,'c003');
insert into product values(null,'香奈儿香水',350,'c003');
insert into product values(null,'SK-II神仙水',350,'c003');
insert into product values(null,'资生堂粉底液',180,'c003');
 
insert into product values(null,'老北京方便面',56,'c004');
insert into product values(null,'良品铺子海带丝',17,'c004');
insert into product values(null,'三只松鼠坚果',88,null);

```

![image-20211231133822155](https://gitee.com/xxb667/img/raw/master/img/image-20211231133822155.png)



# 简单查询

## 语法格式

```
select 
  [all|distinct]
  <目标列的表达式1> [别名],
  <目标列的表达式2> [别名]...
from <表名或视图名> [别名],<表名或视图名> [别名]...
[where<条件表达式>]
[group by <列名> 
[having <条件表达式>]]
[order by <列名> [asc|desc]]
[limit <数字或者列表>];

```

### 简化版语法

```
select *| 列名 from 表 where 条件
```

## 进行简单查询

```
-- 1.查询所有的商品.  
select *  from product;
```

![image-20211231134516343](https://gitee.com/xxb667/img/raw/master/img/image-20211231134516343.png)

```
-- 2.查询商品名和商品价格. 
select pname,price from product;
```

![image-20211231134640673](https://gitee.com/xxb667/img/raw/master/img/image-20211231134640673.png)

```
-- 3.别名查询.使用的关键字是as（as可以省略的）.这个在多表时很有用  
-- 3.1表别名: 
select * from product as p;
-- 3.2列别名：
select pname as pn from product; 
```

![image-20211231134802580](https://gitee.com/xxb667/img/raw/master/img/image-20211231134802580.png)



```
-- 4.去掉重复值.  
select distinct price from product;
```

![image-20211231134928674](https://gitee.com/xxb667/img/raw/master/img/image-20211231134928674.png)



```
-- 5.查询结果是表达式（运算查询）：将所有商品的价格+10元进行显示.
select pname,price+10 from product;
```

![image-20211231135120411](https://gitee.com/xxb667/img/raw/master/img/image-20211231135120411.png)

# 运算符操作

## 4种运算符

### 算术运算符

| **算术运算符**       | **说明**           |
| -------------------- | ------------------ |
| **+**                | 加法运算           |
| **-**                | 减法运算           |
| *****                | 乘法运算           |
| **/** **或** **DIV** | 除法运算，返回商   |
| **%** **或** **MOD** | 求余运算，返回余数 |

```
select 6 + 2;
select 6 - 2;
select 6 * 2;
select 6 / 2;
select 6 % 2;

```

![image-20211231135844670](https://gitee.com/xxb667/img/raw/master/img/image-20211231135844670.png)



![image-20211231135924040](https://gitee.com/xxb667/img/raw/master/img/image-20211231135924040.png)



![image-20211231135957197](https://gitee.com/xxb667/img/raw/master/img/image-20211231135957197.png)

```
 -- 将每件商品的价格加10
select pname,price + 10 as new_price from product;
```

![image-20211231140207010](https://gitee.com/xxb667/img/raw/master/img/image-20211231140207010.png)

```
-- 将所有商品的价格上调10%
select pname,price * 1.1 as new_price from product;
```

![image-20211231140242673](https://gitee.com/xxb667/img/raw/master/img/image-20211231140242673.png)



### 比较运算符

| **比较运算符**                | **说明**                                                     |
| ----------------------------- | ------------------------------------------------------------ |
| **=**                         | 等于                                                         |
| **<**  **和**  **<=**         | 小于和小于等于                                               |
| **>**  **和**  **>=**         | 大于和大于等于                                               |
| **<=>**                       | 安全的等于，两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0 |
| **<>** **或** **!=**          | 不等于                                                       |
| **IS NULL** **或** **ISNULL** | 判断一个值是否为  NULL                                       |
| **IS NOT NULL**               | 判断一个值是否不为  NULL                                     |
| **LEAST**                     | 当有两个或多个参数时，返回最小值                             |
| **GREATEST**                  | 当有两个或多个参数时，返回最大值                             |
| **BETWEEN AND**               | 判断一个值是否落在两个值之间                                 |
| **IN**                        | 判断一个值是IN列表中的任意一个值                             |
| **NOT IN**                    | 判断一个值不是IN列表中的任意一个值                           |
| **LIKE**                      | 通配符匹配                                                   |
| **REGEXP**                    | 正则表达式匹配                                               |

### 逻辑运算符

| **逻辑运算符**           | **说明** |
| ------------------------ | -------- |
| **NOT** **或者** **!**   | 逻辑非   |
| **AND** **或者** **&&**  | 逻辑与   |
| **OR** **或者** **\|\|** | 逻辑或   |
| **XOR**                  | 逻辑异或 |

### 位运算符

| **位运算符** | **说明**               |
| ------------ | ---------------------- |
| **\|**       | 按位或                 |
| **&**        | 按位与                 |
| **^**        | 按位异或               |
| **<<**       | 按位左移               |
| **>>**       | 按位右移               |
| **~**        | 按位取反，反转所有比特 |

## 条件查询

```
-- 查询商品名称为“海尔洗衣机”的商品所有信息：
select * from product where pname = '海尔洗衣机';
```

![image-20211231140545330](https://gitee.com/xxb667/img/raw/master/img/image-20211231140545330.png)

```
-- 查询价格为800商品
select * from product where price = 800;
```

![image-20211231140612327](https://gitee.com/xxb667/img/raw/master/img/image-20211231140612327.png)

```
-- 查询价格不是800的所有商品
select * from product where price != 800;
select * from product where price <> 800;
select * from product where not(price = 800);
```

![image-20211231140705186](https://gitee.com/xxb667/img/raw/master/img/image-20211231140705186.png)

```
-- 查询商品价格大于60元的所有商品信息
select * from product where price > 60;
```

![image-20211231140752560](https://gitee.com/xxb667/img/raw/master/img/image-20211231140752560.png)

```
-- 查询商品价格在200到1000之间所有商品
select * from product where price >= 200 and price <=1000;
select * from product where price between 200 and 1000;
```

![image-20211231140838318](https://gitee.com/xxb667/img/raw/master/img/image-20211231140838318.png)

## 比较运算符查询

```
-- 查询商品价格是200或800的所有商品
select * from product where price = 200 or price = 800;
select * from product where price in (200,800);
```

![image-20211231141339981](https://gitee.com/xxb667/img/raw/master/img/image-20211231141339981.png)

```
-- 查询含有‘裤'字的所有商品
select * from product where pname like ‘%裤%';
```

![image-20211231141453898](https://gitee.com/xxb667/img/raw/master/img/image-20211231141453898.png)

```
-- 查询以'海'开头的所有商品
select * from product where pname like '海%';
```

![image-20211231142031452](https://gitee.com/xxb667/img/raw/master/img/image-20211231142031452.png)

```
-- 查询第二个字为'蔻'的所有商品
select * from product where pname like '_蔻%';
```

![image-20211231142154212](https://gitee.com/xxb667/img/raw/master/img/image-20211231142154212.png)

```
-- 查询category_id为null的商品
select * from product where category_id is null;
```

![image-20211231142309826](https://gitee.com/xxb667/img/raw/master/img/image-20211231142309826.png)

```
-- 查询category_id不为null分类的商品
select * from product where category_id is not null;
```

![image-20211231142343355](https://gitee.com/xxb667/img/raw/master/img/image-20211231142343355.png)



```
-- 使用least求最小值
select least(10, 20, 30); -- 10
select least(10, null , 30); -- null
```

![image-20211231142551192](https://gitee.com/xxb667/img/raw/master/img/image-20211231142551192.png)



![image-20211231142710386](https://gitee.com/xxb667/img/raw/master/img/image-20211231142710386.png)

```
-- 使用greatest求最大值
select greatest(10, 20, 30);
select greatest(10, null, 30); -- null
```

![image-20211231142745979](https://gitee.com/xxb667/img/raw/master/img/image-20211231142745979.png)



## 位运算符

- 位运算符是在二进制数上进行计算的运算符。
- 位运算会先将操作数变成二进制数，进行位运算。
  - 然后再将计算结果从二进制数变回十进制数。

```
select 3&5; -- 位与


```

![image-20211231143127706](https://gitee.com/xxb667/img/raw/master/img/image-20211231143127706.png)



```
select 3|5; -- 位或
```

![image-20211231143206287](https://gitee.com/xxb667/img/raw/master/img/image-20211231143206287.png)



```
select 3^5; -- 位异或
```

![image-20211231143419918](https://gitee.com/xxb667/img/raw/master/img/image-20211231143419918.png)

```
select 3>>1; -- 位左移
```

![image-20211231143449549](https://gitee.com/xxb667/img/raw/master/img/image-20211231143449549.png)

```
select 3<<1; -- 位右移
```

![image-20211231143531583](https://gitee.com/xxb667/img/raw/master/img/image-20211231143531583.png)

```
select ~3;   -- 位取反
```

![image-20211231143600917](https://gitee.com/xxb667/img/raw/master/img/image-20211231143600917.png)



# 排序查询

## 介绍

- 如果我们需要对读取的数据进行排序
- 我们就可以使用 MySQL 的 order by 子句来设定你想按哪个字段哪种方式来进行排序
- 再返回搜索结果

### 语法

```
select 
 字段名1，字段名2，……
from 表名
order by 字段名1 [asc|desc]，字段名2[asc|desc]……
```

## 特点

1.**asc**代表**升序**，**desc**代表**降序**，如果不写**默认升序**

2.order by用于子句中可以支持**单个字段**，**多个字段**，**表达式**，**函数**，**别名**

3.order by子句，放在查询语句的最后面。LIMIT子句除外

## 操作

```
-- 1.使用价格排序(降序)
select * from product order by price desc;
```

![image-20211231144940848](https://gitee.com/xxb667/img/raw/master/img/image-20211231144940848.png)

```
-- 2.在价格排序(降序)的基础上，以分类排序(降序)
select * from product order by price desc,category_id asc;
```

![image-20211231145150027](https://gitee.com/xxb667/img/raw/master/img/image-20211231145150027.png)

```
-- 3.显示商品的价格(去重复)，并排序(降序)
select distinct price from product order by price desc;
```

![image-20211231145253605](https://gitee.com/xxb667/img/raw/master/img/image-20211231145253605.png)

# 聚合查询

## 简介

之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，

而使用聚合函数查询是纵向查询，它是对一列的值进行计算，

然后返回一个单一的值；

另外聚合函数会忽略空值。

| **聚合函数** | **作用**                                                     |
| ------------ | ------------------------------------------------------------ |
| **count()**  | 统计指定列不为NULL的记录行数；                               |
| **sum()**    | 计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0 |
| **max()**    | 计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算； |
| **min()**    | 计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算； |
| **avg()**    | 计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0 |

## 操作

```
-- 1 查询商品的总条数
select count(*) from product;
```

![image-20211231150258514](https://gitee.com/xxb667/img/raw/master/img/image-20211231150258514.png)

```
-- 2 查询价格大于200商品的总条数
select count(*) from product where price > 200;
```

![image-20211231150346072](https://gitee.com/xxb667/img/raw/master/img/image-20211231150346072.png)

```
-- 3 查询分类为'c001'的所有商品的总和
select sum(price) from product where category_id = 'c001';
```

![image-20211231203552006](https://gitee.com/xxb667/img/raw/master/img/image-20211231203552006.png)

```
-- 4 查询商品的最大价格
select max(price) from product;
```

![image-20211231203651753](https://gitee.com/xxb667/img/raw/master/img/image-20211231203651753.png)

```
-- 5 查询商品的最小价格
select min(price) from product;
```

![image-20211231203720959](https://gitee.com/xxb667/img/raw/master/img/image-20211231203720959.png)

```
-- 6 查询分类为'c002'所有商品的平均价格
select avg(price) from product where category_id = 'c002';
```

![image-20211231203751068](https://gitee.com/xxb667/img/raw/master/img/image-20211231203751068.png)

## NULL值的处理

### 介绍

- count函数对null值的处理
  - 如果count函数的参数为星号（*），则统计所有记录的个数。
  - 而如果参数为某字段，不统计含null值的记录个数。
- sum和avg函数对null值的处理
  - 这两个函数忽略null值的存在，
  - 就好象该条记录不存在一样。
- max和min函数对null值的处理
  -  max和min两个函数同样忽略null值的存在。

### 操作

```
-- 创建表
create table test_null( 
 c1 varchar(20), 
 c2 int 
);
```

```
-- 插入数据
insert into test_null values('aaa',3);
insert into test_null values('bbb',3);
insert into test_null values('ccc',null);
insert into test_null values('ddd',6);
```

![image-20211231204244821](https://gitee.com/xxb667/img/raw/master/img/image-20211231204244821.png)

```
-- 测试1
select count(*), count(1), count(c2) from test_null;
```

![image-20211231204404828](https://gitee.com/xxb667/img/raw/master/img/image-20211231204404828.png)

```
-- 测试2
select sum(c2),max(c2),min(c2),avg(c2) from test_null;
```

![image-20211231204506117](https://gitee.com/xxb667/img/raw/master/img/image-20211231204506117.png)

# 分组查询（group-by)

## 简介

分组查询是指使用group by字句对查询信息进行分组。

## 格式

```
select 字段1,字段2… from 表名 group by 分组字段 having 分组条件;
```

## 操作

```
-- 1 统计各个分类商品的个数
select category_id ,count(*) from product group by category_id ;
```

![image-20211231204941114](https://gitee.com/xxb667/img/raw/master/img/image-20211231204941114.png)如果要进行分组的话，则SELECT子句之后，只能出现分组的字段和统计函数，其他的字段不能出现。

## 分组查询

### 分组之后的条件筛选-having

- 分组之后对统计结果进行筛选的话必须使用having，不能使用where
- where子句用来筛选 FROM 子句中指定的操作所产生的行
- group by 子句用来分组 WHERE 子句的输出。
- having 子句用来从分组的结果中筛选行

### 格式

```
select 字段1,字段2… from 表名 group by 分组字段 having 分组条件;
```

### 操作

```
-- 2.统计各个分类商品的个数,且只显示个数大于4的信息
select category_id ,count(*) from product group by category_id having count(*) > 1;
```

 ![image-20211231205040167](https://gitee.com/xxb667/img/raw/master/img/image-20211231205040167.png)

# 分页查询-limit

## 简介

- 分页查询在项目开发中常见，由于数据量很大，显示屏长度有限
  - 因此对数据需要采取分页显示方式。
- 例如数据共有30条，每页显示5条，
  - 第一页显示1-5条，第二页显示6-10条。 

## 格式

```
-- 方式1-显示前n条
select 字段1，字段2... from 表明 limit n
```

```
-- 方式2-分页显示
select 字段1，字段2... from 表明 limit m,n

m: 整数，表示从第几条索引开始，计算方式 （当前页-1）*每页显示条数
n: 整数，表示查询多少条数据
```

## 操作

```
-- 查询product表的前5条记录 
select * from product limit 5 
```

![image-20211231205730171](https://gitee.com/xxb667/img/raw/master/img/image-20211231205730171.png)

```
-- 从第4条开始显示，显示5条 
select * from product limit 3,5

--数据库中的数据索引是从0开始的
```

![image-20211231205808106](https://gitee.com/xxb667/img/raw/master/img/image-20211231205808106.png)

# INSERT INTO SELECT语句

## 简介

将一张表的数据导入到另一张表中，可以使用INSERT INTO SELECT语句 。

## 格式

要求目标表Table2必须存在

```
insert into Table2(field1,field2,…) select value1,value2,… from Table1 
```

```
或者：
insert into Table2 select * from Table1
```

## 操作

```
-- 讲product表的pid和price数据添加到test_null中
insert into test_null(c1,c2) 
select pid,price 
from product; 
```

![image-20211231210324129](https://gitee.com/xxb667/img/raw/master/img/image-20211231210324129.png)



# SELECT INTO FROM语句

## 简介

- 将一张表的数据导入到另一张表中，
- 有两种选择 **SELECT INTO** 和 **INSERT INTO SELECT** 。

## 格式

-  要求目标表Table2不存在，
- 因为在插入时会自动创建表Table2，
- 并将Table1中指定字段数据复制到Table2中。

```
SELECT vale1, value2 into Table2 from Table1
```

## 操作

```
-- 创建新表并复制数据
create table test
(
select  c1 
from test_null
);
```

![image-20211231211719273](https://gitee.com/xxb667/img/raw/master/img/image-20211231211719273.png)

# 正则表达式查询

## 介绍

- 正则表达式(regular expression)描述了一种字符串匹配的规则
- 正则表达式本身就是一个字符串，使用这个字符串来描述、用来定义匹配规则，匹配一系列符合某个句法规则的字符串。
- 在开发中，正则表达式通常被用来**检索**、**替换**那些符合某个规则的文本。
- MySQL通过**REGEXP**关键字支持正则表达式进行字符串匹配。

## 格式

| **模式**       | **描述**                                                     |
| -------------- | ------------------------------------------------------------ |
| **^**          | 匹配输入字符串的开始位置。                                   |
| **$**          | 匹配输入字符串的结束位置。                                   |
| **.**          | 匹配除 "\n" 之外的任何单个字符。                             |
| **[...]**      | 字符集合。匹配所包含的任意一个字符。例如，  '[abc]' 可以匹配  "plain" 中的  'a'。 |
| **[^...]**     | 负值字符集合。匹配未包含的任意字符。例如，  '[^abc]' 可以匹配  "plain" 中的'p'。 |
| **p1\|p2\|p3** | 匹配 p1 或 p2  或 p3。例如，'z\|food'  能匹配  "z" 或  "food"。'(z\|f)ood'  则匹配  "zood" 或  "food"。 |

| *****     | **匹配前面的子表达式零次或多次。例如，****zo\*** **能匹配** **"z"** **以及** **"zoo"****。******* **等价于****{0,}****。** |
| --------- | ------------------------------------------------------------ |
| **+**     | 匹配前面的子表达式一次或多次。例如，'zo+'  能匹配  "zo" 以及  "zoo"，但不能匹配  "z"。+ 等价于  {1,}。 |
| **{n}**   | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}'  不能匹配  "Bob" 中的  'o'，但是能匹配  "food"  中的两个 o。 |
| **{n,m}** | m 和 n 均为非负整数，其中n  <= m。最少匹配 n 次且最多匹配 m 次。 |

## 操作

```
-- ^ 在字符串开始处进行匹配
SELECT  'abc' REGEXP '^a';
```

![image-20211231212417267](https://gitee.com/xxb667/img/raw/master/img/image-20211231212417267.png)

```
-- $ 在字符串末尾开始匹配
SELECT  'abc' REGEXP 'a$';
SELECT  'abc' REGEXP 'c$’;
```

![image-20211231212449068](https://gitee.com/xxb667/img/raw/master/img/image-20211231212449068.png)

![image-20211231212548894](https://gitee.com/xxb667/img/raw/master/img/image-20211231212548894.png)



```
-- . 匹配任意字符
SELECT  'abc' REGEXP '.b';
SELECT  'abc' REGEXP '.c';
SELECT  'abc' REGEXP 'a.';
 
```

![image-20211231212617857](https://gitee.com/xxb667/img/raw/master/img/image-20211231212617857.png)

![image-20211231212646001](https://gitee.com/xxb667/img/raw/master/img/image-20211231212646001.png)

![image-20211231212705793](https://gitee.com/xxb667/img/raw/master/img/image-20211231212705793.png)

```
-- [...] 匹配括号内的任意单个字符
SELECT  'abc' REGEXP '[xyz]';
SELECT  'abc' REGEXP '[xaz]';
```

![image-20211231212804540](https://gitee.com/xxb667/img/raw/master/img/image-20211231212804540.png)

![image-20211231212826586](https://gitee.com/xxb667/img/raw/master/img/image-20211231212826586.png)

```
-- [^...] 注意^符合只有在[]内才是取反的意思，在别的地方都是表示开始处匹配
SELECT  'a' REGEXP '[^abc]';
SELECT  'x' REGEXP '[^abc]';
SELECT  'abc' REGEXP '[^a]';
```

![image-20211231212950171](https://gitee.com/xxb667/img/raw/master/img/image-20211231212950171.png)



![image-20211231213018507](https://gitee.com/xxb667/img/raw/master/img/image-20211231213018507.png)

![image-20211231213043978](https://gitee.com/xxb667/img/raw/master/img/image-20211231213043978.png)



```
-- a* 匹配0个或多个a,包括空字符串。 可以作为占位符使用.有没有指定字符都可以匹配到数据
SELECT 'stab' REGEXP '.ta*b';
SELECT 'stb' REGEXP '.ta*b';
SELECT '' REGEXP 'a*';
```

![image-20211231213114232](https://gitee.com/xxb667/img/raw/master/img/image-20211231213114232.png)



![image-20211231213145951](https://gitee.com/xxb667/img/raw/master/img/image-20211231213145951.png)



![image-20211231213206989](https://gitee.com/xxb667/img/raw/master/img/image-20211231213206989.png)

```
-- a+  匹配1个或者多个a,但是不包括空字符
SELECT 'stab' REGEXP '.ta+b';
SELECT 'stb' REGEXP '.ta+b';
```

![image-20211231213258855](https://gitee.com/xxb667/img/raw/master/img/image-20211231213258855.png)

![image-20211231213320740](https://gitee.com/xxb667/img/raw/master/img/image-20211231213320740.png)



```
-- a?  匹配0个或者1个a
SELECT 'stb' REGEXP '.ta?b';
SELECT 'stab' REGEXP '.ta?b';
SELECT 'staab' REGEXP '.ta?b';
```

![image-20211231213403017](https://gitee.com/xxb667/img/raw/master/img/image-20211231213403017.png)

![image-20211231213436604](https://gitee.com/xxb667/img/raw/master/img/image-20211231213436604.png)

![image-20211231213458189](https://gitee.com/xxb667/img/raw/master/img/image-20211231213458189.png)

```
-- a1|a2  匹配a1或者a2，
SELECT 'a' REGEXP 'a|b';
SELECT 'b' REGEXP 'a|b';
SELECT 'b' REGEXP '^(a|b)';
SELECT 'a' REGEXP '^(a|b)';
SELECT 'c' REGEXP '^(a|b)';
```

![image-20211231213525886](https://gitee.com/xxb667/img/raw/master/img/image-20211231213525886.png)

![image-20211231213559762](https://gitee.com/xxb667/img/raw/master/img/image-20211231213559762.png)

![image-20211231213629535](https://gitee.com/xxb667/img/raw/master/img/image-20211231213629535.png)

![image-20211231213650869](https://gitee.com/xxb667/img/raw/master/img/image-20211231213650869.png)

![image-20211231213707559](https://gitee.com/xxb667/img/raw/master/img/image-20211231213707559.png)





```
-- a{m} 匹配m个a
 
SELECT 'auuuuc' REGEXP 'au{4}c';
SELECT 'auuuuc' REGEXP 'au{3}c';
```

![image-20211231213754400](https://gitee.com/xxb667/img/raw/master/img/image-20211231213754400.png)

![image-20211231213812252](https://gitee.com/xxb667/img/raw/master/img/image-20211231213812252.png)



```
 -- a{m,n} 匹配m到n个a,包含m和n
 
SELECT 'auuuuc' REGEXP 'au{3,5}c';
SELECT 'auuuuc' REGEXP 'au{4,5}c';
SELECT 'auuuuc' REGEXP 'au{5,10}c';
```

![image-20211231213856661](https://gitee.com/xxb667/img/raw/master/img/image-20211231213856661.png)

![image-20211231213921820](https://gitee.com/xxb667/img/raw/master/img/image-20211231213921820.png)

![image-20211231213939406](https://gitee.com/xxb667/img/raw/master/img/image-20211231213939406.png)

```
-- (abc) abc作为一个序列匹配，不用括号括起来都是用单个字符去匹配，如果要把多个字符作为一个整体去匹配就需要用到括号，所以括号适合上面的所有情况。
SELECT 'xababy' REGEXP 'x(abab)y';
SELECT 'xababy' REGEXP 'x(ab)*y';
SELECT 'xababy' REGEXP 'x(ab){1,2}y';
```

![image-20211231214026505](https://gitee.com/xxb667/img/raw/master/img/image-20211231214026505.png)

![image-20211231214049006](https://gitee.com/xxb667/img/raw/master/img/image-20211231214049006.png)



![image-20211231214109925](https://gitee.com/xxb667/img/raw/master/img/image-20211231214109925.png)



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


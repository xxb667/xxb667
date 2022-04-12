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




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


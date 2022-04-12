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




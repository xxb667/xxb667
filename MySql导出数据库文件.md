# MySql导出数据库文件

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

![image-20220325215815879](https://gitee.com/xxb667/img/raw/master/img/image-20220325215815879.png)

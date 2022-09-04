---
title: MySQL导出数据到文件
date: 2017-05-13 10:41:18
tags: mysql,数据,文件
---

从数据库导出数据的方式很多，如命令的`mysqldump`可以将整个数据库或某些表或某些数据导出。

整个数据库导出:

```mysql
mysqldump [-h] -u root -p database>back.sql
```

某部分表导出:

```mysql
mysqldump [-h] -u root -p database table1 table2 > back.sql
```

某些数据:

```mysql
mysqldump [-h] -u root -p database table1 --where="id<100" > back.sql
```

上面的方式一般作为备份或数据迁移使用，自己想查看并不是那么直观，结果里都是些`mysql`命令,如何只将查询结果导出来呢？可以使用这个语句:

```mysql
select * into OUTFILE '/path/to/file' from table where conditions
```

这里需要注意的是，如我们使用下面这样的语句：

```mysql
select * into OUTFILE '/data/back/tmp/order.txt' from order where create_date='2017-03-27'
```

如果你已经将order.txt文件建好了，会报错说文件已经存在。那么说明文件不需要我们去建，于是删掉。再次运行命令可以又报错说没有写的权限，暴力点就给目录加上`0777`的权限就可以了。




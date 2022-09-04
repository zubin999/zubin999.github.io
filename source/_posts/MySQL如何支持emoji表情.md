---
title: MySQL如何支持emoji表情
date: 2016-08-28 23:54:41
tags: mysql,emoji
---

emoji表情对于移动端来说，十分常见。可是，最近才发现我们项目还不支持emoji表情。数据库使用的字符集是`UTF-8`,WTF?还有UTF-8搞不定的？那该用什么？该用`utf8mb4`。那么，改吧。

要想支持emoji表情，首先数据库版本必须是5.5.3及其以上。如果你的数据库版本还没有达到要求，那么请[升级](https://dev.mysql.com/downloads/mysql/)。版本符合要求的，开始修改数据库、表、字段信息吧。

- 数据库

```
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

- 表

将要存储emoji表情的表修改如下:

```
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

- 字段

```
ALTER TABLE table_name CHANGE column_name column_name VARCHAR(200) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

对于字段，除了字符集设置，其他信息字段类型，长度等请根据自己项目的实际情况设置。
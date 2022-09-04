---
title: 使用logstash将MySQL数据同步到elasticsearch
date: 2020-09-06 12:24:44
tags: logstash,elasitcsearch,mysql,数据同步
---

# 安装logstash

logstash下载地址：[https://www.elastic.co/downloads/logstash](https://www.elastic.co/downloads/logstash)，这里只显示了最新版本，历史版本地址：[https://www.elastic.co/downloads/past-releases#logstash](https://www.elastic.co/downloads/past-releases#logstash)。

下载好解压后简单配置下`logstash.conf`（解压后在`config`下有一个`logstash-sample.conf`文件，复制为`logstash.conf`，其他命名也行）就可以使用了。如果你的版本是7.6(不包括)以下，还需要安装`logstash-integration-jdbc`插件，7.6以上该插件已经集成到logstash中，无需额外安装。

# 配置

## 输入配置

默认配置文件打开，在input里有个beats的配置删掉，插入jdbc配置

```
jdbc {
    jdbc_driver_library => "mysql-connector-java-5.1.36-bin.jar"
    jdbc_driver_class => "com.mysql.jdbc.Driver"
    jdbc_connection_string => "jdbc:mysql://数据库地址:3306/数据库名字"
    jdbc_user => "数据库账号"
    jdbc_password => "数据库密码"
    schedule => "* * * * *这里和Linux的crontab配置一样"
    statement => "查询出要同步到elasticsearch的数据"
    use_column_value => true
    tracking_column => "update_time"
    tracking_column_type => "timestamp"
  }
```

- use_column_value

值为布尔值，这里影响`sql_last_value`的值，默认为`false`，`sql_last_value`使用上次执行时间,初始值为1970-01-01零点。设置为`true`，那么这里是`tracking_column`配置字段的值，如`tracking_column => "id"`，那么这里就是上次执行的id值。

- tracking_column

数据库字段，sql判断条件。如主键、更新时间等。

- tracking_column_type

上面字段值的类型，有两个值可选：`timestamp`、`numeric`。默认为`numeric`。影响sql_last_value，如上面字段`update_time`假设为时间戳，但是这里设置为`timestamp`，那么`sql_last_value`初始值为`1970-01-01`，这就会使sql_last_value一直停留在初始值。设置为`numeric`，初始值为`0`。

## 输出配置

默认配置：

```
 elasticsearch {
    hosts => ["http://localhost:9200"]
    index => ""
    document_id => "%{elasticsearch使用哪个字段作主键ID}"
    #user => "elastic"
    #password => "changeme"
  }
```

`hosts`需要改为自己的`elasticsearch`地址。虽然，参数`document_type`参数还存在，但是默认是会将数据同步到`_doc`下面。从elastcisearch的6.0开始便不再推荐使用type，7.0以上的版本已经废弃，而8.0版本会直接移除。详细说明：[removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/removal-of-types.html)。

这样已经能满足数据同步了，如有其它需求可根据参数说明文档作调整。

- 启动

启动logstash只需进入到bin目录，执行：

```
./logstash -f /配置文件路径/xxx.conf
```

## logstash配置

logstash配置分为三个部分：input(输入)、filter(过滤)、output(输出)。

- jdbc中last_run_metadata_path设置的路径需有读写权限。如果没有写入权限，变量sql_last_value虽然会记录上一次的值，但在重新启动后，值会从0开始。

- 启动时提示：

> WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need eit
her to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

在jdbc_connection_string配置后面加上参数：useSSL=false&verifyServerCertificate=false即可。

- MySQL中tinyint(1)字段值jdbc读取后会被转换为布尔值。同样在jdbc_connection_string后面加上参数：tinyInt1isBit=false&transformedBitIsBoolean=false可以保持原值不被转换。


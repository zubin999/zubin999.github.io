---
title: Elasticsearch安装
date: 2019-11-14 11:07:20
tags: elasticsearch,安装
---

官文地址：[在这里][1]
1. 下载

```
vagrant@machine2:~$ curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.4.2-linux-x86_64.tar.gz
```

2. 解压

```
vagrant@machine2:~$ tar xzf elasticsearch-7.4.2-linux-x86_64.tar.gz
```

3. 启动

```
vagrant@machine2:~$ cd elasticsearch-7.4.2/bin/
vagrant@machine2:~/elasticsearch-7.4.2/bin$ ./elasticsearch
```

出现问题：

```
could not find java in JAVA_HOME or bundled at /usr/lib/jmv/java-8-openjdk-amd64/bin/java
```

查看环境变量：

```
echo $JAVA_HOME
// 得到：
/usr/lib/jmv/java-8-openjdk-amd64
```

需要注意文档这段话：

> Elasticsearch includes a bundled version of OpenJDK from the JDK maintainers (GPLv2+CE). To use your own version of Java, see the JVM version requirements

也就是说自带了jdk，而我自己之前有安装jdk，需要另外配置。通过链接查看[额外配置文档][3]。

> Elasticsearch is built using Java, and includes a bundled version of OpenJDK from the JDK maintainers (GPLv2+CE) within each distribution. The bundled JVM is the recommended JVM and is located within the jdk directory of the Elasticsearch home directory.
> 
> To use your own version of Java, set the JAVA_HOME environment variable. If you must use a version of Java that is different from the bundled JVM, we recommend using a supported LTS version of Java. Elasticsearch will refuse to start if a known-bad version of Java is used. The bundled JVM directory may be removed when using your own JVM.

还是推荐使用自带的`jdk`，没有细说。也在目录下面看到了`jdk`目录，那重新设置`$JAVA_HOME`环境变量到`Elasticsearch`里面的`jdk`目录:

```
export JAVA_HOME=/home/vagrant/elasticsearch-7.4.2/jdk
```

再次启动，显示了一大堆启动的东西，已经成功了，下面来验证是否启动：

```
curl 127.0.0.1:9200

// 会得到:
{
  "name" : "iZ23pskgys8Z",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "J2SyY5cZRayLwC1lsPaoGA",
  "version" : {
    "number" : "7.4.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "2f90bbf7b93631e52bafb59b3b049cb44ec25e96",
    "build_date" : "2019-10-28T20:40:44.881551Z",
    "build_snapshot" : false,
    "lucene_version" : "8.2.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

得到上面的回应，就说明安装成功了。

- `vagrant` 环境下安装

我本地是`vagrant`+`virtualbox`搭建的环境。在安装成功后，修改`elasticsearch.yml`的`network`将`IP`绑定在`vagrant`的`IP`上，如我的是`192.168.56.102`，修改55行：

```
network.host: 192.168.56.102
```

修改68行：

```
discovery.seed_hosts:[]
```

这样，我在`windows`的浏览器里访问`http://192.168.56.102:9200`同样成功了。



  [1]: https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-install.html
  [2]: https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html
  [3]: https://www.elastic.co/guide/en/elasticsearch/reference/current/setup.html#jvm-version
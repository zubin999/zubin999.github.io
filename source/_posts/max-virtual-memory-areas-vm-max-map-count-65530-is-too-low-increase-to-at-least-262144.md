---
title: 'max virtual memory areas vm.max_map_count [65530] is too low, increase to at
  least [262144]'
date: 2020-10-10 21:23:48
tags: elasitcsearch,memory
---

`Elasticsearch`在启动时出现错误：

```
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

这个问题是虚拟内存设置问题。在[虚拟内存](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)有说明：`Elasticsearch`默认使用`mmapfs`类型存储文档。对`mmapfs`的介绍，Elasticsearch文档也有[说明](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-store.html#mmapfs)。系统默认是65530，对`Elasticsearch`远远不够。

- 解决办法

修改`/etc/sysctl.conf`，如果文件内有`vm.max_map_count`项，那么修改值大于或等于报错值262144。如果没有，在末尾追加一行。

修改完之后再终端执行`sysctl --system`或者`sysctl -p`使修改生效。检查是否生效可以使用`sysctl vm.max_map_count`查看。
---
title: Elasticsearch问题收集
date: 2019-11-19 19:14:15
tags:
---

- No alive nodes found in your cluster

在客户端出现这个问题，检查`Elasticsearch`是不是死掉了。如使用:

```
ps -aux | grep elasticsearch

// 得到：
vagrant  13508  0.0  0.0  14228   928 pts/0    S+   19:10   0:00 grep --color=auto elasitcsearch
```

那么，重新启动服务。另外，以daemon方式启动，如果出错是看不到错误的。只显示这一行：

```
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
```

- mapper [title] of different type, current_type [keyword], merged_type [text]

完整的报错类似于这样的：

```
Array
(
    [error] => Array
        (
            [root_cause] => Array
                (
                    [0] => Array
                        (
                            [type] => illegal_argument_exception
                            [reason] => mapper [title] of different type, current_type [keyword], merged_type [text]
                        )

                )

            [type] => illegal_argument_exception
            [reason] => mapper [title] of different type, current_type [keyword], merged_type [text]
        )

    [status] => 400
)
```

这个是怎么出现的呢？就是先已经将`title`设定为`keyword`类型，然后又去设定`title`为`text`类型，就造成了这个问题。那么，怎么办呢？[在这里有解释][1]：修改已经存在的字段会使之前已经建立索引的数据无效。但遇到之前建立错了，必须要改，怎么办呢？只能使用新的`mappings`重新建立一个新的`index`，然后使用`reindex`接口去对数据建立索引。

- master not discovered yet, this node has not previously joined a boots and [cluster.initial_master_nodes] is empty on this node

首先，这个问题发生在我阅读到`path.data`配置时产生的。在此之前，运行正常，其他没有任何配置改动。那么，可以判断是这个配置造成的，后将其注释，再次启动成功。所以，为什么在配置`path.data`后就失败了呢？从这句话来看，需要设置`cluster.initial_master_nodes`的值。我这里是本地就一个单机，先尝试像`discovery.seed_hosts: []`一样，给个空试试，依然在报这个错误。那先将`Node`部分的`node.name`注释去掉，使用默认的`node-1`，然后将`discovery`的`cluster.initial_master_nodes`注释也去掉，并给值`node-1`，可以启动成功了。

  [1]: https://www.elastic.co/guide/en/elasticsearch/reference/7.x/mapping.html#update-mapping

---
title: Native controller process has stopped - no new native processes can be started
date: 2019-11-15 15:20:00
tags: elasticsearch,nproc
---

昨天安装好，今天启动`Elasticsearch`时失败：

```
...
ERROR: [1] bootstrap checks failed
[1]: max number of threads [2048] for user [vagrant] is too low, increase to at least [4096]
[2]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
[2019-11-15T14:46:51,596][INFO ][o.e.n.Node               ] [machine2] stopping ...
[2019-11-15T14:46:51,640][INFO ][o.e.n.Node               ] [machine2] stopped
[2019-11-15T14:46:51,640][INFO ][o.e.n.Node               ] [machine2] closing ...
[2019-11-15T14:46:51,666][INFO ][o.e.n.Node               ] [machine2] closed
[2019-11-15T14:46:51,669][INFO ][o.e.x.m.p.NativeController] [machine2] Native controller process has stopped - no new native processes can be started
```

遇到这样的错误，不能看最后一句话。看前面的`error`，这里有两个错误。

第一个解决方案可以[看这里][1]给出了两种方法：

1. 使用`Linux`命令`ulimit -u xxxx`设置。
2. 修改`/etc/security/limits.conf`中对应当前用户的`nproc`值

额外说下，`nproc`代表`max number of processes`。这在文件前面有解释

第二个问题[看这里][2]，可以知道，没有做任何网络配置，`Elasticsearch`会绑定在`loopback address(127.0.0.1)`上，并扫描`9300`到`9305`的端口去尝试连接其他节点（这样做是方便了集群）。出现这个问题是因为昨天我将`IP地址`修改了。所以，解决这个问题可以：

1. 将昨天的改动去掉，网络配置的IP地址恢复默认配置
2. 在`$ES_HOME/config/elasticsearch.yml`的`Discovery`模块中配置`discovery.seed_hosts: []`




  [1]: https://www.elastic.co/guide/en/elasticsearch/reference/master/max-number-of-threads.html
  [2]: https://www.elastic.co/guide/en/elasticsearch/reference/master/discovery-settings.html
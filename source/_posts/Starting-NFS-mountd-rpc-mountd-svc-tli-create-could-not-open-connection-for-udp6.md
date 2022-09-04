---
title: 'Starting NFS mountd: rpc.mountd: svc_tli_create: could not open connection for udp6'
date: 2019-09-10 12:55:18
tags: nfs,ipv6
---

在安装好nfs后，重启服务：

```
service nfs restart1
```

得到下面报错：

```
Starting kernel based NFS server: idmapd mountdrpc.mountd: svc_tli_create:
could not open connection for udp6
rpc.mountd: svc_tli_create: could not open connection for tcp6
rpc.mountd: svc_tli_create: could not open connection for udp6
rpc.mountd: svc_tli_create: could not open connection for tcp6
rpc.mountd: svc_tli_create: could not open connection for udp6
rpc.mountd: svc_tli_create: could not open connection for tcp6
statd nfsdrpc.nfsd: address family inet6 not supported by protocol TCP
sm-notify
done
```


udp6是用于IPV6的，所以这里可以在`/etc/netconfig`中IPV6的注释掉：

```
udp        tpi_clts      v     inet     udp     -       -
tcp        tpi_cots_ord  v     inet     tcp     -       -
#udp6       tpi_clts      v     inet6    udp     -       -
#tcp6       tpi_cots_ord  v     inet6    tcp     -       -
rawip      tpi_raw       -     inet      -      -       -
local      tpi_cots_ord  -     loopback  -      -       -
unix       tpi_cots_ord  -     loopback  -      -       -
```


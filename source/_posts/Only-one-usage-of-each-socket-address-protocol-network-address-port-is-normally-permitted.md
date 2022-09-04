---
title: 'Only one usage of each socket address (protocol/network address/port) is
  normally permitted.'
date: 2019-09-28 15:20:12
tags: golang,rpc
---

golang开启一个服务：

```
func main() {
       
        rpc.Register(new(service name))
        listener, err := net.Listen("tcp", ":1215")
        if err != nil {
                log.Fatal("listen error:", err)
        }
        for {
                conn, err := listener.Accept()
                if err != nil {
                        continue
                }
                
                go jsonrpc.ServeConn(conn)
        }
}
```

遇到上面报错，将`listen`的地址写全即可

```
listener, err := net.Listen("tcp", "127.0.0.1:1215")
```
---
title: 'auth failed from unknown (xx.xx.xx.xx) unauthorised user'
date: 2019-09-10 17:33:28
tags: sersync,rsync,auth
---

在部署`sersync + rsync`时，一直不成功，从日志中看到：

```
auth failed from unknown (xx.xx.xx.xx) unauthorised user
```

从报错中可以看出可能是两个原因：

1. unknown (xx.xx.xx.xx)
2. unauthorised user


<!--more-->

在网上有很多人的建议方案是在`rsync`的服务器的`/etc/hosts`中添加主服务器的`IP`及名字。这与第一个原因也相符，先尝试这种解决方案。之后再看到日志中看到错误有一点变化：

```
auth failed from machine_name(xx.xx.xx.xx) unauthorised user
```

`unauthorised user`有可能是账号密码的问题，但是在主服务器通过手动推送检查账号密码是没有问题的。

```
rsync-avzP /data/www/ username@xx.xx.xx.xx::www/--password-file=/etc/rsync.password
```

在同步服务器上可以看到所要同步的内容已经全部同步，日志也显示正常。账号密码没有问题，但是却又和账号密码有关，再检查`sersync`的`config.xml`配置文件，发现：

```
<rsync>
            <commonParamsparams="-artuz"/>
            <auth start="false"users="sync_user" passwordfile="/etc/rsync.pas"/>
             <userDefinedPortstart="false" port="874"/><!-- port=874 -->
             <timeoutstart="true" time="100"/><!-- timeout=100 -->
             <sshstart="false"/>
         </rsync>
```

其中`auth`的`start`仍为`false`。将其修改为`true`后重试正常了。
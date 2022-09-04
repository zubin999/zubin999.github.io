---
title: Volt directory can't be written
date: 2016-03-29 22:14:45
tags: phalcon,Volt
---

上周开始在新公司工作，公司使用的是Phalcon框架。以前只是听说过这个名字....(此处省略1万字).
当我在使用git克隆项目代码到本地时，访问项目出现了此错误：

```
Volt directory can't be written
```

看到不能被写入，首先想到的是文件权限问题，但是，我是在windows下，哪里来的权限问题？！那是什么问题呢？Volt是Phalcon框架的模板引擎，会将`views`下的文件重新编译后放在`cache`目录下。再一看项目第一级文件结构，没有`cache`文件，会不会是放在`app`或其他目录下呢？还是来看配置文件吧。

在`app/config/config.php`文件里找到了`cache`的位置：

```php
'application' => array(
        'controllersDir' => APP_PATH . '/app/controllers/',
        'modelsDir'      => APP_PATH . '/app/models/',
        'migrationsDir'  => APP_PATH . '/app/migrations/',
        'viewsDir'       => APP_PATH . '/app/views/',
        'pluginsDir'     => APP_PATH . '/app/plugins/',
        'libraryDir'     => APP_PATH . '/app/library/',
        'cacheDir'       => APP_PATH . '/app/cache/',
        'baseUri'        => '/',
    )
```

所以，在`app`下建立一个`cache`文件夹，再次在浏览器刷新，项目可以正常访问了。
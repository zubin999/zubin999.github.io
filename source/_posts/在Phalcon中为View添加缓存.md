---
title: 在Phalcon中为View添加缓存
date: 2019-08-02 15:23:22
tags: phalcon,view,缓存
---

我们可以为`Phalcon`的`View`添加事件监听，`View`一共有5个事件：

Event Name| Triggered|Can stop operation ?|
-----------|---------|--------------------|
beforeRender|Triggered before starting the render process|Yes|
beforeRenderView|Triggered before rendering an existing view|Yes|
afterRenderView|Triggered after rendering an existing view|No|
afterRender|Triggered after completing the render process|No|
notFoundView|Triggered when a view was not found|No|

在`di`注入是`view`时添加事件：




```
<?php

use Phalcon\Events\Event;
use Phalcon\Events\Manager as EventsManager;
use Phalcon\Mvc\View;

$di->set(
    'view',
    function () {
        // Create an events manager
        $eventsManager = new EventsManager();

        // Attach a listener for type 'view'
        $eventsManager->attach(
            'view',
            function (Event $event, $view) {
                echo $event->getType(), ' - ', $view->getActiveRenderPath(), PHP_EOL;
            }
        );

        $view = new View();

        $view->setViewsDir('../app/views/');

        // Bind the eventsManager to the view component
        $view->setEventsManager($eventsManager);

        return $view;
    },
    true
);
```

上面是监听了View的所有事件,如果要监听某个具体的,如监听`beforeRender`：

```
$eventsManager->attach('view:beforeRender', function(){

})
```

现在，就可以利用`afterRender`事件来缓存页面，提升性能。

首先，需要向`di`注入一个`viewCache`服务：

```
<?php

use Phalcon\Cache\Frontend\Output as OutputFrontend;
use Phalcon\Cache\Backend\Memcache as MemcacheBackend;

// Set the views cache service
$di->set(
    'viewCache',
    function () {
        // Cache data for one day by default
        $frontCache = new OutputFrontend(
            [
                'lifetime' => 86400,
            ]
        );

        // Memcached connection settings
        $cache = new MemcacheBackend(
            $frontCache,
            [
                'host' => 'localhost',
                'port' => '11211',
            ]
        );

        return $cache;
    }
);
```

这里的`cache`也可以换成`Redis`等你在使用的。`lifetime`为默认缓存时间，当调用该服务时，可以另外给出`lifetime`的值。键为`key`。`Phalcon`会默认使用`MD5`加密当前的`Controller/view`。

定义`afterRender`事件触发时要做的事情:

```
class ViewPlugin extends \Phalcon\Mvc\User\Plugin {

    public function afterRender(\Phalcon\Events\Event $event, $view){
        $request    = \Phalcon\Di::getDefault()->get('request');
        $key        = md5($request->getURI());
        if (!$view->getCache()->exists($key)) {
            $view->getCache()->save($key, $view->getContent(), DEFAULT_VIEW_EXPIRE_TIME);
        }
    }
}
```

这样当访问一个新页面后就会缓存下来。接下来就是第二步，再次访问该页面时，直接向浏览器输出缓存内容，可以定义在父类里：

```
$this->view_cache_key = md5($this->request->getURI());
if ($this->view->getCache()->exists($this->view_cache_key)) {
    echo $this->view->getCache()->get($this->view_cache_key);
    exit;
}
```

这就是利用`View`的事件缓存页面，当然也可以用于其他场景，如生成静态`HTML`页面

参考文档:

[Phalcon view][1]


  [1]: https://docs.phalconphp.com/3.4/en/views
---
title: PHP多个CURL请求异步执行
date: 2020-02-28 10:38:26
tags: php,curl,并行请求
---

先看PHP简单的curl请求:

```
$ch = curl_init('http://example.com');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_exec($ch);
curl_close($ch);
```

如果有多个请求,那么需要多次重复上面的操作:

```
$ch = curl_init('http://example1.com');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_exec($ch);
curl_close($ch);

$ch = curl_init('http://example2.com');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_exec($ch);
curl_close($ch);

// 更多的请求...
```

这样,所需时间是每个请求消耗的时间之和.如果有5个请求,每个请求耗时1秒,那么需要5秒的时间.换作异步执行,所需耗时是最长时间的那个请求的时间,那么久只需要1秒钟完成5个请求.

```
// 同样先初始化curl
$ch1 = curl_init('http://example1.com');
curl_setopt($ch1, CURLOPT_HEADER, 0);
curl_setopt($ch1, CURLOPT_RETURNTRANSFER, 1);
// 这里不再使用curl_exec
$ch2 = curl_init('http://example2.com');
curl_setopt($ch2, CURLOPT_HEADER, 0);
curl_setopt($ch2, CURLOPT_RETURNTRANSFER, 1);
// 初始化异步多请求
$mh = curl_multi_init();
// 添加前面的每个handle
curl_multi_add_handle($mh, $ch1);
curl_multi_add_handle($mh, $ch2);

// 执行请求
$active = null;
do {
    $status = curl_multi_exec($mh, $active);
}while($status === CURLM_CALL_MULTI_PERFORM);
while ($active && $status == CURLM_OK){
    // 即使未还有未执行完的请求这里也可能是-1
    // 参考: https://www.php.net/manual/en/function.curl-multi-select.php#115381
    if(curl_multi_select($mh) === -1){
        usleep(100);
    }
        
    do {
        $status = curl_multi_exec($mh, $active);
    }while ($status === CURLM_CALL_MULTI_PERFORM);
}

// 如果需要返回结果
$res[] = curl_multi_getcontent($ch1);
$res[] = curl_multi_getcontent($ch2);

// 移除handle
curl_multi_remove_handle($ch1);
curl_multi_remove_handle($ch2);

// 关闭
curl_multi_close($mh);
```

在具体业务中,可能就是以数组的形式传入所需请求的URL,参数等.我们可以使用循环来操作.
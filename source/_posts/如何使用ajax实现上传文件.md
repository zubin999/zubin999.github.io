---
title: 如何使用ajax实现异步上传文件(译文)
date: 2015-06-24 08:49:20
tags: ajax,上传文件
---

> 原文: [http://www.sitepoint.com/html5-ajax-file-upload/](http://www.sitepoint.com/html5-ajax-file-upload/)

> 在之前我的文章里，我们讨论了怎么使用`HTML5的拖拽功能`和使用`HTML5和Javascript操作文件`。现在我们有一些文件将要上传到服务器。这个进程将在后台异步进行，因此用户可以在页面上做其他的事情。

#### The HTML

再次检查HTML表单：

``` html5
<form id="upload" action="upload.php" method="POST" enctype="multipart/form-data">
<fieldset>
<legend>HTML File Upload</legend>
<input type="hidden" id="MAX_FILE_SIZE" name="MAX_FILE_SIZE" value="300000" />
<div>
    <label for="fileselect">Files to upload:</label>
    <input type="file" id="fileselect" name="fileselect[]" multiple="multiple" />
    <div id="filedrag">or drop files here</div>
</div>
<div id="submitbutton">
    <button type="submit">Upload Files</button>
</div>
</fieldset>
</form>
```

我们将把文件上传至`upload.php`的`PHP文件`，这个页面将在用户点击上传文件后，处理`AJAX`上传请求和表单`POST`提交。
`Javascript`能够保证仅上传小于`300,000B`的`jpg图片`。

#### The Javascript

首先，我们从新的一行创建一个函数`FileSelectHandler`，用于有文件被选择或者放入时调用。要遍历文件我们调用另一个函数`UploadFile`。

``` JavaScript
// file selection
function FileSelectHandler(e) {

    // cancel event and hover styling
    FileDragHover(e);

    // fetch FileList object
    var files = e.target.files || e.dataTransfer.files;

    // process all File objects
    for (var i = 0, f; f = files[i]; i++) {
        ParseFile(f);
        UploadFile(f);
    }

}
```

文件上传需要`XMLHttpRequest2`对象,这个对象在`FireFox`和`chrome`中支持(译者注：这篇文章发表于2012年,当前各个浏览器的支持请看[这里](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest))。在我们调起`Ajax`之前，我们要确定`upload`方法存在并且有一个小于`MAX_FILE_SIZE`值的`JPG图片`。

``` JavaScript
// upload JPEG files
function UploadFile(file) {

    var xhr = new XMLHttpRequest();
    if (xhr.upload && file.type == "image/jpeg" && file.size <= $id("MAX_FILE_SIZE").value) {
        // start upload
        xhr.open("POST", $id("upload").action, true);
        xhr.setRequestHeader("X_FILENAME", file.name);
        xhr.send(file);

    }

}
```

#### The PHP

``` php
<?php
$fn = (isset($_SERVER['HTTP_X_FILENAME']) ? $_SERVER['HTTP_X_FILENAME'] : false);
if ($fn) {

    // AJAX call
    file_put_contents(
        'uploads/' . $fn,
        file_get_contents('php://input')
    );
    echo "$fn uploaded";
    exit();
    
}else {

    // form submit
    $files = $_FILES['fileselect'];

    foreach ($files['error'] as $id => $err) {
        if ($err == UPLOAD_ERR_OK) {
            $fn = $files['name'][$id];
            move_uploaded_file(
                $files['tmp_name'][$id],
                'uploads/' . $fn
            );
            echo "<p>File $fn uploaded.</p>";
        }
    }

}
```
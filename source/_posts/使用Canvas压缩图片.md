---
title: 使用Canvas压缩图片
date: 2016-08-11 23:25:40
tags: canvas,压缩图片
---

## 为什么要前端压缩？
首先，图片在前端压缩是可以实现的。为什么要在前端压缩呢？有什么好处？在前端压缩会不会更耗时......对于这一系列的问题，可以很肯定的回答，前端压缩是需要的。尤其对于流量大的网站，可以节省很多带宽呢(都是RMB呀)。那么，怎么压缩呢？当然，这项工作就交给`HTML5`和`Javascript`吧。有他俩搭档，很容易就能完成这件事情。

## 原理
当你在网上搜索*`HTML5上传文件`*时，你会找到很多拖拽的demo。不论是拖拽还是按钮上传，原理都这样：
- 对事件(change,drop)的监听
- 通过文件API--`FileReader`获取图片
- 通过canvas绘制图片,在这里可以对图片进行缩放，大小压缩

## 实践
HTML代码:
```
<form>
<input type="file" id="uploadfile"/>
</form>
```

Javascript代码：
```
<script>
document.getElementById('uploadfile').addEventListener('change', function(event){
    //code for resize
}, false);
</script>
```

## 压缩代码

``` javascript
;(function ($,doc) {
    'use strict';

    var defaultOps = {
        url: null,
        quality: 40,
        resize: false,
        scale:{
            maxWidth: 640,
            maxHeight: null
        },
        output: null,
        formData: {},
        uploadProgress: function () {

        },
        uploadComplete: function () {

        },
        uploadFailed: function () {

        },
        uploadCanceled: function () {

        },
        uploading: function () {

        }
    };

    function _compload(evt, options) {
        var self = this;

        this.defaults = $.extend(true, defaultOps, options);
        if(!this.defaults.url){
            throw new Error('url is invalid');
        }

        this.afterSelect = function () {
            var file = evt.target.files[0],
                reg  = new RegExp('^image.*$'),
                type = this.defaults.output || file.type;

            if(!file){
                throw new Error('文件为空');
            }
            if(!reg.test(file.type)){
                throw new Error('只能上传图片');
            }

            var reader = new FileReader(),
                i      = doc.createElement('img');

            reader.onload = function (e) {
                i.src = e.target.result;
            }
            reader.readAsDataURL(file);

            i.onload = function (e) {
                var compressImg = self.compress(i, self.defaults.quality, type),
                    imgFile     = self.convert(compressImg.src);

                self.upload(imgFile);
            };
        }

        this.compress = function (source_img_obj, quality, outputType) {

            var cvs     = doc.createElement('canvas'),
                nWidth  = source_img_obj.naturalWidth,
                nHeight = source_img_obj.naturalHeight,
                sWidth  = self.defaults.scale.maxWidth,
                sHeight = self.defaults.scale.maxHeight,
                scale   = 1;

        if(self.defaults.resize){
                    if(sWidth && sHeight){
                        scale = Math.min(sWidth/nWidth, sHeight/nHeight);
                    }else if (sWidth && nWidth >= sWidth && !sHeight){
                        scale = sWidth/nWidth;
                    }else if (!sWidth && nHeight >= sHeight && sHeight){
                        scale = sHeight/nHeight;
                    }
                }

            cvs.width  = nWidth * scale;
            cvs.height = nHeight * scale;

            var ctx              = cvs.getContext("2d").drawImage(source_img_obj, 0, 0, cvs.width, cvs.height),
                newImageData     = cvs.toDataURL(outputType, quality/100),
                result_image_obj = new Image();

            result_image_obj.src = newImageData;

            return result_image_obj;
        }

        this.convert = function (img_src) {
            var BASE64_MARKER = ';base64,',
                dataURL = img_src;
            if (dataURL.indexOf(BASE64_MARKER) == -1) {
                var parts = dataURL.split(',');
                var contentType = parts[0].split(':')[1];
                var raw = parts[1];

                return new Blob([raw], {type: contentType});
            }

            var parts = dataURL.split(BASE64_MARKER);
            var contentType = parts[0].split(':')[1];
            var raw = window.atob(parts[1]);
            var rawLength = raw.length;

            var uInt8Array = new Uint8Array(rawLength);

            for (var i = 0; i < rawLength; ++i) {
                uInt8Array[i] = raw.charCodeAt(i);
            }

            return new Blob([uInt8Array], {type: contentType});
        }

        this.upload = function (file) {
            self.defaults.uploading();
            setTimeout(function () {
                var fd = new FormData();
                fd.append("file", file);
                for (var k in self.defaults.formData){
                    fd.append(k, self.defaults.formData[k]);
                }
                var xhr = new XMLHttpRequest();
                xhr.upload.addEventListener("progress", self.defaults.uploadProgress, false);
                xhr.addEventListener("load", self.defaults.uploadComplete, false);
                xhr.addEventListener("error", self.defaults.uploadFailed, false);
                xhr.addEventListener("abort", self.defaults.uploadCanceled, false);
                xhr.open("POST", self.defaults.url);
                xhr.send(fd);
            }, 10);
        }

        this.afterSelect();
    }

    window.Compload = _compload;
})(jQuery,document);

```
---
title: SimpleAjaxUploader上传后赋值失败问题
date: 2022-05-16 21:51:55
tags:
---

项目中有这样一段关于上传图片的代码:

```
<label class="label required">频道icon:</label>
            <div class="form-item mb-0">
                <input class="form-control" readonly name="img" value="{{data['img']}}"/>
                <button type="button" class="btn btn-primary" id="uploadImage">上传</button>
            </div>
            <div class="form-text help-block">图片的最佳尺寸：100*100，其他尺寸会影响页面效果，格式png，jpg，gif。大小不超过2M</div>
            <div class="form-item mb-4">
                <div class="col-4">
                    <img src="{{data['img']|default(default_image)}}" id="imgPreview" class="img-responsive"/>
                </div>
            </div>
```

上传插件使用的是[Simple Ajax Uploader][1],在上传完成后对input和img赋值:

```
onComplete: function(filename, res, uploadBtn) {
                if(res.error_code != 0){
                    // error msg
                }else{
                    $('input[name=img]').val(res.thumb[0].url);
                    $('#imgPreview').prop('src', res.thumb[0].url);
                }
            },
```

这时候,问题出现了,input框赋值成功,但是img的src值没有改变,控制台也没有任何错误信息.将两行赋值代码交换顺序:

```
$('#imgPreview').prop('src', res.thumb[0].url);
$('input[name=img]').val(res.thumb[0].url);
```

img的src正常复制,input正常复制.这就非常奇怪了.

经过多次调试,最后在input赋值之前加上debugger调试,发现给input赋值之后又进入了上传插件中:

```
 _finish: function (r, p, q, o, n, s, w, t, v, u) {
      this.log('Server response: ' + q);
      if (this._opts.responseType.toLowerCase() == 'json') {
        q = m.parseJSON(q);
        if (q === false) {
          this._errorFinish(r, p, false, 'parseerror', o, n, s, t, v, u);
          return
        }
      }
      this._opts.onComplete.call(this, o, q, u);
      this._last(n, s, w, t, v);
      r = p = q = o = n = s = w = t = v = u = null
    },
```

执行到了onComplete这一行,进入后看到错误:

```
"Failed to set the 'value' property on 'HTMLInputElement': This input element accepts a filename, which may only be programmatically set to the empty string."
```

上传插件添加OnError回调同样得到错误:

```
An attempt was made to use an object that is not, or is no longer, usable
```

为什么会再次进入上传插件SimpleAjaxUploader.js呢?最后发现,该上传插件所创建input的name也是img:

```
<div style="display: block; position: absolute; overflow: hidden; margin: 0px; padding: 0px; opacity: 0; direction: ltr; z-index: 2147483583;"><input type="file" name="img" multiple="" style="position: absolute; right: 0px; margin: 0px; padding: 0px; font-size: 480px; font-family: sans-serif; cursor: pointer;"></div>
```

所以,在给input赋值时,也给上面这个input赋值了,从而出错!而这个img来源于配置里的name,所以也可以修改配置.


  [1]: https://www.lpology.com/code/ajaxuploader/docs.php
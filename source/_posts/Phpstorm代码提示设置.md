---
title: Phpstorm代码提示phalcon设置
date: 2016-03-21 22:36:53
tags: phpstorm,phalcon
---

Phalcon框架因为是以PHP扩展的形式存在，所以不同于其他的框架，安装完成后，IDE还并不能提示代码，这里就需要用到***Developer Tools***。该工具可在*github*上找到，**[传送门][1]**。

1. 下载项目

使用`git`工具`clone`：

    git clone https://github.com/phalcon/phalcon-devtools.git

2. 设置PhpStorm

在PhpStorm左边项目下面有个`External Libraries`,双击可弹出对话框，对话框里点击'+'，然后选择你的`developer-devtools`目录下`ide`下对应的版本。现在便有提示了。这种方式同时适用于Visual Studio Code.

3. 另一种方式

在PhpStorm插件里直接搜索phalcon,有一个autocomplete插件,安装后即可.

[1]: https://github.com/phalcon/phalcon-devtools
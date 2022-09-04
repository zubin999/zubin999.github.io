---
title: 使用Zephir写PHP扩展
date: 2020-07-31 16:45:40
tags: php,zephir,扩展
---

`PHP`由`c语言`写的，如果要编写`PHP扩展`，我们需要理解`PHP源码`，需要使用`c语言`来编写扩展程序，这样步骤繁琐，入门门槛高。有了`Zephir`之后，编写`PHP扩展`，就和编写`PHP代码`一样简单。

# 安装Zephir

+ 安装Zephir parser

准备需要用到的东西：

```
sudo yum install php-devel gcc make re2c autoconf automake
```

得到下面的结果：

>[root@centos8 zeno]# sudo yum install php-devel gcc make re2c autoconf automake
>Last metadata expiration check: 1:03:17 ago on Fri 31 Jul 2020 04:20:32 PM CST.
>Package gcc-8.3.1-5.el8.0.2.x86_64 is already installed.
>Package make-1:4.2.1-10.el8.x86_64 is already installed.
>No match for argument: re2c
>Package autoconf-2.69-27.el8.noarch is already installed.
>Package automake-1.16.1-6.el8.noarch is already installed.
>Error: Unable to find a match: re2c

re2c没有找到，那自己下载安装：

```
wget https://github.com/skvadrik/re2c/archive/2.0.1.tar.gz
```

本地有一个`2.0.1.tar.gz`的文件，解压re2c并进入目录：

```
tar xzf 2.0.1.tar.gz
cd re2c-2.0.1/
```

得到文件：

>[zeno@centos8 re2c-2.0.1]$ ls
>add-release.txt  benchmarks  build      cmake     configure.ac  examples  include  libre2c_old
>Makefile.am      NO_WARRANTY  release.sh       sf-cheatsheet  test
>autogen.sh       bootstrap   CHANGELOG  CMakeLists.txt  doc           fuzz      lib      LICENSE      >Makefile.lib.am  README.md    run_tests.sh.in  src

使用autoreconf命令：

```
autoreconf -i -W all
```

再次查看目录文件:

>[zeno@centos8 re2c-2.0.1]$ ls
>aclocal.m4       autogen.sh      benchmarks  build      CHANGELOG  CMakeLists.txt  configure     doc       >fuzz     lib          LICENSE  Makefile.am  Makefile.lib.am  README.md   run_tests.sh.in  src
>add-release.txt autom4te.cache bootstrap   build-aux  cmake   config.h.in  configure.ac  examples 
>include  libre2c_old  m4       Makefile.in  NO_WARRANTY      release.sh  sf-cheatsheet    test

可以看到有个`configuer`文件。安装：

```
./configure && sudo make && sudo make install
```

现在需要的东西都已准备好了。安装Zephir-parser：

```
git clone git://github.com/phalcon/php-zephir-parser.git
cd php-zephir-parser
phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make
sudo make install
```

在`php.ini`中开启扩展：

```
extension=zephir_parser.so
```

接下来就是安装zephir了。这一步十分简单，只需从`https://github.com/phalcon/zephir/releases/latest`下载`zephir.phar`文件，然后将其移入/usr/bin/或者/usr/local/bin目录下，再加上可执行权限：

```
mv zephir.phar /usr/bin/zephir
chmod +x /usr/bin/zephir
```

zephir便已经安装上了。测试一下，可以在输入命令`zephir`或`zephir help`，如果出现zephir的使用介绍便证明安装成功了。

# 扩展需求及初始化

在开发api的过程中，经常我们会遇到需要显示`xx秒前`、`xx天前`这样的格式，现在就写一个扩展，将传入进来的时间戳转换为对应的格式后返回。

首先，初始化项目：

```
// 这里zephir会自动将大写转为小写，所以直接用小写
zephir init timehelper
cd timehelper/timehelper
```

在这个目录下建一个文件：`helper.zep`，先写一个测试方法输出`hello world`：

```
namespace TimeHelper;

class Helper {

    public static function say(){
        echo "hello world!";
    }
}

```

然后使用`zephir build`来编译安装扩展程序，如果编译安装成功会得到提示：

```
 Preparing for PHP compilation...
 Preparing configuration file...
 Compiling...
 Installing...

 Extension installed.
 Add "extension=timehelper.so" to your php.ini

 ! [NOTE] Don't forget to restart your web server
```

现在，只需在`php.ini`文件里加上`extension=timehelper.so`就能使用扩展了。

# 测试扩展程序

刚刚安装了`timehelper`这个扩展程序，要怎么使用呢？新建一个PHP文件，加入下面一行代码：

```
<?php
TimeHelper\Helper::say();
```

保存为`test.php`后使用PHP执行：`php test.php`，如果输出了`hello world`，那就成功了。

# 编写扩展

```

namespace TimeHelper;

class Helper {

    public static function say(){
        echo "hello world!";
    }
    
    public function before(int! sec)-> string|bool {
        if sec > time() || sec < 0 {
            return false;
        }
        
        int diff;
        let diff = time() - sec;
        if diff <= 60 {
            return diff . "秒前";
        }

        if diff > 60 && diff < 3600 {
            return this->getMin(diff);
        }

        if diff >= 3600 && diff < 86400 {
            return this->getHour(diff);
        }

        if diff >= 86400 && diff < 2592000 {
            return this->getDay(diff);
        }

        if diff >= 2592000 && diff < 2592000 * 12 {
            return this->getMonth(diff);
        }
        
        return this->getYear(diff);
    }

    private function getMin(int! sec)-> string{
            if sec === 0 {
                return "";
            }

            int min = intval(sec / 60);
            let sec = sec % 60;
            if sec > 0 {
                return min . "分" . sec . "秒前";
            }

            return min . "分前";
    }

    private function getHour(int! sec)-> string {
            int hour = intval(sec / 3600);
            int diff = sec % 3600;
            var min = this->getMin(diff);

            if strlen(min) {
                return hour . "小时" . min;
            }

            return hour . "小时前";
    }

    private function getDay(int! sec)->string {
            int day = intval(sec / 86400);

            return day . "天前";
    }

    private function getMonth(int! sec)-> string {
            int month = intval(sec / 2592000);

            return month . "月前";
    }

    private function getYear(int! sec) -> string {
            int year = intval(sec / 2592000 / 365);

            return year . "年前";
    }
}

```

+ PHP测试程序

修改刚刚的`test.php`为下：

```
<?php
$helper = new TimeHelper\Helper();

// xx秒前
$sec = time() - 50 ;
echo $helper->before($sec), "\n";

// xx分xx秒前
$sec = time() - 73;
echo $helper->before($sec), "\n";

// xx小时前
$sec = time() - 3950;
echo $helper->before($sec), "\n";

// xx天前
$sec = time() - 86600;
echo $helper->before($sec), "\n";

// xx月前
$sec = time() - 2597000;
echo $helper->before($sec), "\n";

// xx年前
$sec = time() - 948672000;
echo $helper->before($sec), "\n";
```

在使用PHP执行该文件后可以看到输出：

```
50秒前
1分13秒前
1小时5分50秒前
1天前
1月前
1年前
```

# 后记

- 在zephir的代码里，可以直接使用PHP的内置函数，如上面用到`time`,`intval`
- zephir里返回值可以多个，用`|`分隔
- zephir里参数可以不指定类型：

```
public function say(str){}
```

也可以指定(直接写类型，不加感叹号)：

```
public function say(string str){}
```

还可以强制指定类型(加感叹号在类型后面)：

```
public function say(string! str){}
```

后面两种差别是前者传入类型不一致会作类型转换，转换不成功会报错，后者不会作类型转换，类型必须一致。

- 之前我是在Windows的虚拟机里使用，代码写在Linux和Windows的共享目录里，在Linux终端运行`zephir build`编译失败，因为底层gcc编译会当做跨平台编译，需要传入--host参数，但是zephir命令还不支持传参给gcc，所有把代码移出共享目录，放Linux下任意目录编译成功。
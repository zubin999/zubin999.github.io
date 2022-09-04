---
title: vagrant增加虚拟机磁盘空间
date: 2020-10-08 19:53:21
tags: vagrant,磁盘空间
---

在`vagrant`中增加磁盘空间，可以使用插件`vagrant-disksize`。

- 插件安装

```
vagrant plugin install vagrant-disksize
```

如果出现超时错误，需要终端设置代理。安装好之后，如果虚拟机已经在运行，先使用`vagrant halt`关闭，在`Vagrantfile`中添加配置：

```
config.disksize.size = '50GB'
```

保存后再使用`vagrant up`启动。启动时，看到一段话：

```
==> machine5: Resized disk: old 10240 MB, req 51200 MB, new 51200 MB
==> machine5: You may need to resize the filesystem from within the guest.
```

- 配置

启动后进入系统，使用`df`查看发现，磁盘空间没有还是变化。需要下面几个步骤：

1. 运行`sudo cfdisk /dev/sda`，这时候能够看到新增的空间了。如我的原本是`10GB`，配置里改为了`50GB`，这里就看到两行，第一行是`/dev/sda1`有`10GB`，第二行是新增的`40GB`。
2. 使用上下箭头选中第一行，再使用左右箭头选中`Resize`，后面的提示`New size: 50G`按回车确认。
3. 再使用左右箭头选择`Write`，显示提示`Are you sure you want to write the partition table to disk?`时输入`yes`按回车确认。
4. 选择`Quit`回车。
5. 运行`sudo resize2fs -p -F /dev/sdaX`，X替换为自己对应的数字。如果是centos 7,8这里会提示：`resize2fs: Bad magic number in super-block while trying to open /dev/sda1. Couldn't find valid filesystem superblock.`换成运行`xfs_grows /dev/sda1`

最后，使用`df`查看，磁盘空间已经增加。

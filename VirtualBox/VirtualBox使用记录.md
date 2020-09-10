# VirtualBox使用记录

## 安装使用

首先是安装，安装直接在官网下载最新的版本，进行安装。

安装完了VirtualBox之后，下一步是去下载ISO镜像文件，我一开始下载了正常的（4G左右），进行安装，但是安装完成之后，重启就会出现问题，使劲反复，后来就吧Everything和Minimal版本都下了，还是会出现问题。

查阅了相关资料后，发现可能是WIndows系统中的Hyper-V引起的问题，所以将其关闭了，重启计算机，最后完成了安装。

**需要注意的是，要安装界面需要在软件中进行选择GNONE**

## 安装了界面之后，不是全屏？

在除此安装之后，并不是全屏，在网上查资料说需要安装增强。

操作了之后还是不行，再查找发现需要更新内核。

```
yum -y install gcc-c++
yum -y install kernel kernel-devel
可能还有gcc
```

之后再重启安装

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200910162420969.png" alt="image-20200910162420969" style="zoom:67%;" />

安装增强一次后，需要弹出再安装。

## 如何使用ssh连接自己本地的虚拟机？

在VirtualBox中进行设置，设置好主机端口和子端口，然后确定虚拟机安装好了ssh `ssh localhost`可以测试

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200910162827272.png" alt="image-20200910162827272" style="zoom:67%;" />

之后在本地就可以使用xshell进行连接了

## 简化连接

使用ssh连接每次都要打开软件然后再启动虚拟机，好麻烦！

可以使用VirtualBox自带的工具，VBoxManage来进行管理，路径是

> C:\Program Files\Oracle\VirtualBox

可以把这个路径添加到PATH里面，然后就可以在dos里面调用了。

两个常用的命令：

- `vbm2 startvm centos --type headless`
- `vbm2 controlvm centos poweroff`

这样可以快速启动和连接


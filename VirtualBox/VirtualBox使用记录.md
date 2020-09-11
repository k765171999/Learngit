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

## 主机不能访问虚拟机发布在9870端口的服务。为啥？

在今下午的实验中，采用同样的配置方法，在虚拟机中进行hadoop的配置，配置完成之后，运行`sbin/start-dfs.sh`也没有问题，但是去localhost:9870查看，却显示不出来。

使用了GNONE之后，在虚拟机上查看可以显示出来，但是在主机上却显示不出来。

**最后发现问题是出在了防火墙上，关闭了防火墙就可以了**

------------------------------------------------------------------------------------------

上面是直接使用的NAT来进行测试的，在一开始时没想到防火墙，以为是NAT连接的天然缺陷，最后换了桥接。

首先在VirtualBox中设置好桥接

设置好了之后，需要进入到Linux系统中进行下一步设置,<a href="https://github.com/heibaiying/BigData-Notes/blob/master/notes/installation/%E8%99%9A%E6%8B%9F%E6%9C%BA%E9%9D%99%E6%80%81IP%E5%8F%8A%E5%A4%9AIP%E9%85%8D%E7%BD%AE.md">参考资料</a>

```
# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
使用上面的命令编辑对应的参数
基本包括：
BOOTPROTO=static
IPADDR=192.168.0.107
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
DNS1=192.168.0.1
ONBOOT=yes
最后重启下网络
# systemctl restart network
```

设置好了上面这些后，虚拟机就可一ping通主机了，但是主机ping不通虚拟机，需要在Windows的入站规则中进行设置才行。

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200911171241185.png" alt="image-20200911171241185" style="zoom:50%;" />

再选择高级设置

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200911171307242.png" alt="image-20200911171307242" style="zoom:50%;" />

就可以了，最后再打开防火墙，就完成了

----

两者最大的区别是，使用桥接，虚拟机是一个单独的ip


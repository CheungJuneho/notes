# Linux 服务器基本运维管理操作

## 实验概述

### 实验简介 

近年来开源技术快速发展，Linux 服务器操作系统在整个服务器操作系统中占据越来越重要的地位。另一方面，云计算提供即买即用、灵活调整的云资源，也让越来越多企业和个人将应用和服务转移到云上。本次实验将展示如何在云上 Linux 服务器中完成基本的运维管理操作。

**实验目标**

完成此实验后，可以掌握的能力有：

1. 熟悉使用命令行管理 Linux 操作系统
2. 掌握对 Linux 系统中的文本编辑、文档基本管理以及权限设置 [:question][doc-define]
3. 掌握使用 rpm 及 yum 等工具实现对 Linux 的软件安装及管理

> <bubble for="doc-define"> Linux 操作系统中文档是文件和目录的总称，即“文档=文件+目录” </bubble>

**学前建议**

1. 对 Linux 操作系统有基础认识
2. 对云服务器有基础认识

**实验步骤**

在本次实验中，一个包含三个主要的实验步骤：

![实验步骤](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E9%AA%8C%E8%AF%BE%E6%94%B9%E9%80%A0/Linux%20%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%9F%BA%E6%9C%AC%E8%BF%90%E7%BB%B4%E7%AE%A1%E7%90%86%E6%93%8D%E4%BD%9C/%E5%9B%BE%E7%89%871.png) 

1. 创建一台 Linux 操作系统的云服务器作为实验练习基础平台
2. 在 Linux 服务器中进行文档管理，其中包括文档基本管理、权限管理和修改文件
3. 在 Linux 服务器中使用 rpm 和 yum 管理软件包

下面，会依次进行每一步的操作说明。

## 1. 创建 Linux 操作系统的云服务器 

### 创建云服务器
本实验已经提前为您准备好一台 Linux 云服务器，在实验过程中直接使用即可。关于在腾讯云中创建 Linux 云服务器的具体配置流程请参考腾讯云官方文档：[快速配置 Linux 云服务器 - 腾讯云][cvm-doc]

> <webpage for="cvm-doc" url="https://cloud.tencent.com/document/product/213/2936"></webpage>

## 2. 管理 Linux 服务器上的文档

### 文档基本管理 

### 查看目录内容及目录属性

查看根目录下内容：

`ls` 命令可以用于显示指定工作目录下的内容，比如后跟参数 `/`，可以查看根目录下的内容

``` shell
ls /
```

显示根目录本身详细属性：

`-ld`选项分别代表查看详细属性和查看目录本身

``` shell
ls -ld /
```

### 切换工作目录

切换工作目录到`/boot`目录：

`cd`命令用于切换到指定的工作目录

``` shell
cd /boot
```


### 查看目录内容的属性和隐藏文档

显示当前目录内容的详细属性，并加上易读的容量单位：

`-h`选项配合`-l`选项可以使得属性中的文件大小以易读的容量单位显示

``` shell
ls -lh
```

显示`/root`的全部内容，包括隐藏文档：

`-A`选项可以显示出目录中包含的隐藏文档

``` shell 
ls -A /root
```

显示`/bin/bash`程序，详细属性：

``` shell
ls -l /bin/bash
```

### 创建文档

在目录`/opt`下创建一个子目录`test`：

`mkdir`命令用于在指定路径下创建空目录

``` shell
mkdir /opt/test
```

> <checker type="output-contains" command="ls /opt" hint="请先完成创建目录/opt/test">
> <keyword regex="test" />
> </checker>

在目录`/opt/test`创建文件`readme.txt`和`testfile`：

`touch`命令用于在指定路径下创建空文件

``` shell 
touch /opt/test/readme.txt
touch /opt/test/testfile
```

> <checker type="output-contains" command="ls /opt/test" hint="请先完成创建文件 readme.txt 与 testfile">
> <keyword regex="readme" />
> <keyword regex="testfile" />
> </checker>


### 复制文件

将文件`/etc/passwd`和`/etc/resolv.conf`同时复制到`/opt/test`目录下：

`cp`命令用于复制文档，当复制多个文档时，逐一列出源文档的路径，并将目标路径作为最后一个参数即可

``` shell
cp /etc/passwd /etc/resolv.conf /opt/test
```

> <checker type="output-contains" command="ls /opt/test" hint="请先完成复制文件/etc/passwd 和/etc/resolv.conf">
> <keyword regex="passwd" />
> <keyword regex="resolv" />
> </checker>

将文件`/etc/redhat-release`复制到`/root`下，同时改名为`version.txt`：

`cp`命令在目标路径中额外指定一个新的文档名称可以为复制过来的“复制品”进行重命名

``` shell
cp /etc/redhat-release  /root/version.txt
```

> <checker type="output-contains" command="ls /root" hint="请先完成复制文件/etc/redhat-release 并改名为 version.txt">
> <keyword regex="version" />
> </checker>

### 复制目录

将`/home`目录复制到`/opt/test`目录下 ：

`cp`命令复制目录时必须加上`-r`选项

``` shell
cp -r /home /opt/test
```

> <checker type="output-contains" command="ls /opt/test" hint="请先完成复制目录/home">
> <keyword regex="home" />
> </checker>

### 移动文档

将文件`/root/version.txt`移动到`/opt/test`目录下：

`mv`命令用于移动（剪切）文档

``` shell
mv /root/version.txt /opt/test
```

> <checker type="output-contains" command="ls /opt/test" hint="请先完成移动文件/root/version.txt">
> <keyword regex="version" />
> </checker>



### 删除文档

删除文件`/etc/samba/smb.conf.example`：

`rm`命令用于删除文档，一般搭配`-rf`两个选项一起使用，Linux 中删除后的文档一般无法找回，执行删除操作需要更加谨慎

``` shell
rm -rf /etc/samba/smb.conf.example
```

> <checker type="output-contains" command="ls /etc/samba/smb.conf.example" hint="请先完成删除文件 smb.conf.example">
> <keyword regex="cannot" />
> </checker>

### 文件的权限管理

### 目录基本权限设置

设置`/opt/test`目录的权限为禁止其它用户访问：

`chmod`命令用于修改文档权限，`o=---`则是指定其它用户的读、写、执行权限均为无

``` shell
chmod o=--- /opt/test
```

> <checker type="output-contains" command="ls -ld /opt/test/ | awk '{print substr($1,8,3)}'" hint="请先完成目录/opt/test 的权限设置">
> <keyword regex="\-\-\-" />
> </checker>

### 文件基本权限设置

设置`/opt/test/resolv.conf`文件为所有用户只读文件：

`ugo=r--`代表将文件的所有身份对应的权限修改为只读

``` shell
chmod ugo=r-- /opt/test/resolv.conf
```

> <checker type="output-contains" command="ls -l /opt/test/resolv.conf | awk '{print substr($1,2,9)}'" hint="请先完成文件/opt/test/resolv.conf 的权限设置">
> <keyword regex="r--r--r--" />
> </checker>

### 文件编辑

### 创建与编辑文件

在目录`/opt/test`创建文件`linux.txt`，写入内容 StudyLinux：

1. 创建文件：

``` shell
touch /opt/test/linux.txt
```

> <checker type="output-contains" command="ls /opt/test" hint="请先完成创建文件 linux.txt">
> <keyword regex="linux" />
> </checker>

2. 编辑文件添加以下内容，然后保存并退出：

编辑 [linux.txt][locate-linux] 文件，并在文件中添加以下内容：

   > <edit for="locate-linux" file="/opt/test/linux.txt" />

```tex
StudyLinux
```

   完成编辑后，记得保存文件[:question][save-info1]：

   > <bubble for="save-info1">保存方法：**Windows** 系统点击 **ctrl+s**，**Mac OS** 点击 **command+s** 保存</bubble>

> <checker type="output-contains" command="cat /opt/test/linux.txt" hint="请先完成修改文件 linux.txt">
> <keyword regex="StudyLinux" />
> </checker>


### 修改配置文件

修改文件`/etc/hostname`将其原有内容全部删除，写入新的内容为 server0.example.com：

Linux 中通常没有图形化操作界面，所以各种系统属性、软件设置都通过修改文件的形式实现

编辑配置文件 [hostname][locate-hostname] 删除原有内容、写入以下内容，然后保存并退出：

> <edit for="locate-hostname" file="/etc/hostname" />

``` tex
server0.example.com
```

完成编辑后，记得保存文件[:question][save-info1]：

   > <bubble for="save-info1">保存方法：**Windows** 系统点击 **ctrl+s**，**Mac OS** 点击 **command+s** 保存</bubble>

> <checker type="output-contains" command="cat /etc/hostname" hint="请先完成修改文件 hostname">
> <keyword regex="server0.example.com" />
> </checker>

## 3.管理 Linux 服务器上的软件包

### 使用 rpm 安装

利用 wget 从腾讯云仓库源下载软件包`vsftpd`：

`wget`命令是 Linux 命令行的下载工具，`-O`选项用于指定下载的保存路径

``` shell
wget -O /root/vsftpd-3.0.3-34.el8.x86_64.rpm http://mirrors.tencentyun.com/centos/8/AppStream/x86_64/os/Packages/vsftpd-3.0.3-34.el8.x86_64.rpm
```

> <checker type="output-contains" command="ls /root" hint="请先下载软件包 vsftpd">
> <keyword regex="vsftpd" />
> </checker>

使用 rpm 本地安装软件包`vsftpd`：

`rpm`命令用于安装软件包（默认不显示安装过程），`-ivh`选项用于指定以进度条的形式显示将安装过程

``` shell
rpm -ivh /root/vsftpd-3.0.3-34.el8.x86_64.rpm
```

> <checker type="output-contains" command="ls /etc/" hint="请先安装软件包 vsftpd">
> <keyword regex="vsftpd" />
> </checker>

### 使用 yum 安装

使用 yum 安装软件包`httpd`：

`yum`命令可以自动解决 Linux 软件包之间的依赖关系，帮助快速安装软件包，一般搭配`-y`选项一起使用代表取消安装之前的询问[:question][yum-rely]

> <bubble for="yum-rely"> 软件包之间存在依赖关系，一般安装一个软件包需要同时安装它的依赖包，**yum**可以自动解决这种依赖关系 </bubble>

``` shell
yum -y install httpd
```

> <checker type="output-contains" command="httpd -v" hint="请先安装软件包 httpd">
> <keyword regex="version" />
> </checker>

## 其它实验
### 管理 Linux 服务器存储

除了以上的实验内容外，还有“管理 Linux 服务器存储”的相关课程实验亦可在腾讯云中进行练习操作。

**创建云硬盘**

通过在腾讯云中创建云硬盘，作为云服务器的额外数据盘，用于存储数据，详情请参考腾讯云官方文档：[创建云硬盘 - 腾讯云 ][cbs-doc1]

> <webpage for="cbs-doc1" url="https://cloud.tencent.com/document/product/362/32401"></webpage>

**挂载云硬盘**

创建的云硬盘需要通过挂载，与云服务器进行关联后，才能被云服务器识别，详情请参考腾讯云官方文档：[挂载云硬盘 - 腾讯云 ][cbs-doc2]

> <webpage for="cbs-doc2" url="https://cloud.tencent.com/document/product/362/32402"></webpage>

**初始化云硬盘**

系统识别到的新硬盘设备，暂时未能直接存储数据，在这之前需要进行分区、格式化等初始化操作，详情请参考腾讯云官方文档：[初始化云硬盘 - 腾讯云][cbs-doc3]

> <webpage for="cbs-doc3" url="https://cloud.tencent.com/document/product/362/32403"></webpage>
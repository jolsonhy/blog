---
title: "华硕 RT-AC86U 刷梅林改版系统"
date: 2018-11-01T11:18:03+08:00
# draft: false
tags: [教程, 技术]
categories: [Tutorial]
---


前几天刚把 K2P 换成 86U，以下是 86U 刷梅林系统的步骤，非常简单：

## 刷机前的准备
- EXT 格式的 1G 以上 U盘一个，如果不知道怎么格式化，可以查看此文章附录。
- 改版梅林安装包，下载地址为：[这里](https://1drv.ms/u/s!ArS0nJ_RMzVYgtwvUNgsl7Q1Z9W75Q)，或直接到Coolshare论坛下载：[这里](http://koolshare.cn/thread-127878-1-1.html)
- 科学上网工具离线安装包，下载地址为[这里](https://1drv.ms/u/s!ArS0nJ_RMzVYgtwuoBmaqxYUM2UrxA)，或直接到[GitHub](https://github.com/hq450/fancyss)上找到与你机器相匹配的
- 一根网线，用于连接电脑与路由器
- 一杯茶，用于等待的时候喝哈哈哈

## 正式开刷

### 系统初始化

1. 拿到机器后，打开机器背后开关键，然后连上电源，开机。

2. 找到一根网线，一头接自己的电脑，另一头接路由器的LAN口。

3. 在浏览器地址栏中输入[router.asus.com](http://router.asus.com)进入管理后台。

4. 依次设置自己的PPPoE、无线WIFI、管理后台账号，设置完之后会进入到官方原版的后台界面。


### 刷改版梅林固件

1. 在后台管理界面中找到`高级设置`-`系统管理`-`固件升级`，选择刚才将已准备好的梅林固件（格式为.w），然后点击上传，等待两三分钟即可，当重启后重新进入后台时有一个 `Powered by Asuswrt-Merlin & Coolshare` 代表成功。

2. 双清路由器。进入`高级设置`-`系统设置`，找到`Presistent JFFS2 parttition`，将`Format JFFS partition at next boot`和`Enable JFFS custom scripts and configs`两项都设置为是，并点击`应用本页面设置`按钮，然后重启路由器即可。

<!-- more -->

### 安装科学上网工具
1. 由于梅林已经取消软件中心的科学上网工具在线安装，所以可采取离线安装的方式。

2. 从[这里](https://1drv.ms/u/s!ArS0nJ_RMzVYgtwuoBmaqxYUM2UrxA)上下载离线安装包，打开软件中心，先点击更新将软件中心升级到最新版本，然后点击`离线安装`，上传刚才下载的安装包，注意格式需要为`.tar.gz`，如果不对，手动将其补全后再上传。

3. 等待安装成功后，再打开科学上网，再配置自己的节点即可。


### 挂载虚拟内存
1. 将已准备好的EXT格式U盘插入路由器背部接口（2.0或3.0都可以）

2. 打开软件中心，将`虚拟内存`这个软件安装上

3. 打开该软件，选择这个磁盘，我这里叫做`sda`，选择虚拟内存大小为`512M`或`1G`，然后点创建虚拟内存，等待一段时间即可。

4. 再次打开软件中心-虚拟内存，确保状态显示为`在/mnt/sda下找到swapfile，且已成功挂载！`，表明已挂载成功。



## 附录：U盘格式化为EXT格式
准备好一个1G以上的U盘（随便的都行，利用率不高），并格式化为EXT2,3,4任意的格式，我这边用的是EXT3。

### Windows系统

- 如果没有Linux基础，可使用软件进行操作，可参考百度经验：[如何在windows下把硬盘格式化成EXT3格式？](https://jingyan.baidu.com/article/fea4511a142846f7bb912522.html)

- 如果有一定Linux基础，可使用SSH连接到路由器通过路由器进行操作，具体步骤如下：

 1. 打开路由器后台`高级设置`-`系统设置`，找到`服务`中的`启用SSH`，选择打开`LAN+WAN`，然后保存设置。
 2. 下载一个叫PuTTY的软件，hostname填`192.168.50.1`（路由器后台地址，可能不一样，自己查看下） 然后选择`SSH`。port端口号为`22`，然后点open。
 3. 用自己前面设置的路由器账号密码登录即可，然后出现命令行界面。
 4. 在U盘插入路由器后面的情况下，依次输入以下命令（两横杠后的是解释说明，不用输入），然后等待显示完成即可。

``` bash
df -h // 找到你的盘符,如：sda1
umount /dev/sda1 // 解除进程的占用
/bin/mkntfs /dev/sda1 // 格式化硬盘为ntfs格式
mkfs.ext4 -T largefile /dev/sda1 // 格式化硬盘为ext4格式
```


### macOS系统

- 确保已经安装了Mac下的包管理软件Homebrew，如果没有，可以用以下命令安装

``` bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

- 安装 e2fsprogs

``` bash
brew install e2fsprogs
```

- 插上U盘，执行以下命令在执行结果中找到U盘盘符，我这里是`/dev/disk2s1`

``` bash
diskutil list
```

- 卸载U盘并将其格式化为 `ext2/3/4`格式，这里以ext3为例

``` bash
diskutil unmountdisk /dev/disk2s1
sudo $(brew --prefix e2fsprogs)/sbin/mkfs.ext3 /dev/disk2s1
```

- 中间可能会有确认提示，按提示操作即可。格式化完成就能直接拔掉U盘了。

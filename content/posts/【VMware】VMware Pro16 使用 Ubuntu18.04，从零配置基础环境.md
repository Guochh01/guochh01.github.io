@[TOC](VMware Pro16 使用 Ubuntu18.04，从零配置基础环境)

# 1 VMware 环境配置
## 1.1 安装 VMware Pro16
- 进入官网（[官网链接](https://www.vmware.com/cn/products/workstation-pro.html)），点击下载试用版
- 安装完成后，进行激活（搜一下激活码，有很多）


## 1.2 下载 Ubuntu18.04.6 LTS 镜像
- 进入阿里镜像源（[链接](https://mirrors.aliyun.com/ubuntu-releases/18.04.6/)）
- 下载 `ubuntu-18.04.6-desktop-amd64.iso`

## 1.3 创建虚拟机
- 选择 **自定义**
- 选择 **稍后安装操作系统**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/760d728a6c44f9a28b80d51b2199c63b.png#pic_center =500x)
- 命名虚拟机
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a70397d300d0121ba61c4f7b2e459db8.png#pic_center =500x)
- 默认 **下一步** 操作
- 指定磁盘容量（`40G+` 比较好）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/51261e1aa741328c736b9e10dc542bbc.png#pic_center =500x)
- 存储磁盘文件
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5f702f0926bcdc69c90541da010b2f63.png#pic_center =500x)
- 选择自定义硬件
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1624e2fbfbe6496fa5fe5b66a5f40b54.png#pic_center =800x)
- 完成后，开启虚拟机
- 先不进行安装，**退出安装** or 选择 **先不安装，进行试用**
- 打开终端，暂时**修改分辨率**（默认`800 * 600`，无法进行安装）
```shell
xrandr -s 1920x1200
```
- 分辨率正常后，开始安装
- 后面唯一复杂的就是 **分区操作**
>看到的一些博客：

1. 第 1 块硬盘称为 `sda`，第 2 块硬盘称为 `sdb`……，依此类推。
2. 一块硬盘最多有 `4` 个主分区，主分区以外的分区称为扩展分区，硬盘可以没有扩展分区，但是一定要有主分区，在扩展分区中可以建立若干个逻辑分区。
3. 在 `Linux` 系统中每一个硬盘总共最多有 `16` 个分区，硬盘上的 `4` 个主分区，分别标识为`sda1`、`sda2`、`sda3` 和 `sda4`，逻辑分区则从 `sda5` 开始标识一直到 `sda16`。




> 建议分区大小：

名称 | 用于 | 大小
--- | --- | ---
 `efi` 系统盘 | `FAT32` | `1G`
 `swap` | 交换空间 | 自定义硬件中内存的大小
 挂载`/`  | `EXT4` | `20G`
 挂载`/home`  | `EXT4` | 剩余全部容量
- 安装完成，重启虚拟机

## 1.4 查看虚拟磁盘占用
```shell
df -h # human-readable
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/72177a8e2734a22cb61a4f0d4940ad4a.png#pic_center =700x)
## 1.5 安装 VMware Tools
> **目的：**
> - 为了实现虚拟机与主机之间的复制粘贴、文件拖拽

- 先试一下虚拟机软件中安装 `VMware Tools`
1. 重启虚拟机，点击菜单栏中 **虚拟机 --- 安装 WMware Tools**
2. 按照官方文档[教程](https://docs.vmware.com/cn/VMware-Workstation-Pro/16.0/com.vmware.ws.using.doc/GUID-08BB9465-D40A-4E16-9E15-8C016CC8166F.html)安装
```shell
cd /tmp
tar zxpf '/media/gch/VMware Tools/VMwareTools-10.3.23-16594550.tar.gz' 
cd vmware-tools-distrib/
sudo ./vmware-install.pl # 一直 enter 就可以
```
3. 安装完成后，终端信息会提示你需要做什么

**我在使用这种安装方式的时候，不管是执行 `/usr/bin/vmware-user`，还是重启设置时间同步等等，都无法实现复制粘贴和文件拖拽。**

- ==最后成功的方法==

感谢[善良的小猪](https://blog.csdn.net/qq_43206665/article/details/123215855)博主的分享!
1. 先将之前安装的 `WMware Tools` 卸载掉
```shell
sudo /usr/bin/vmware-uninstall-tools.pl
```
2. 安装 `open-vm-tools` 和 `open-vm-tools-desktop`
```shell
sudo apt-get install open-vm-tools open-vm-tools-desktop 
```
3. 无需重启虚拟机，执行下面的命令即可
```shell
vmware-user # 若有 warning，按下 enter 即可
```
**在重启虚拟机后，无需再次执行！！**

---

# 2 配置基础环境
## 2.1 使用主机代理
>==参考文章：==
>- [VMware上安装的Ubuntu Linux共享Windows主机VPN连接外网](https://arctee.cn/686.html)

>**主机环境说明：**
>- `Windows10` 操作系统 
>- 使用 `v2rayN` 代理软件

- 虚拟机网络配置为 `NAT` 模式
- windows 查看虚拟机 `VMnet8` 虚拟网卡的 `ip`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5ff808d350eb42f839e882b3f5946a74.png#pic_center =800x)
- 开启虚拟机，手动配置其网络代理
>**前提条件：**
>- 开启主机代理中的 **允许来自互联网的连接**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9516263c781a243474db389435acc720.png#pic_center =700x)
>- 查看主机代理的 **端口号**
>![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2ac76d6634bcc25e7dadd9e7ab7e472b.png#pic_center =700x)

由上述可知，`http/https` 的端口为 `10809`，`socks` 的端口为 `10808`。
左边填  `VMnet8` 虚拟网卡的 `ip`，右边填相应的端口号。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d40405b4b0b9263814a532d4383ead8a.png#pic_center =800x)

- 配置完成，进入 `Google` 测试一下~ 

## 2.2 安装 Google Chrome
>==参考文章：==
>- [如何在 Ubuntu 20.04 上安装 Google Chrome 网络浏览器](https://zhuanlan.zhihu.com/p/137114100)

- 下载安装包并安装
```shell
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt-get install ./google-chrome-stable_current_amd64.deb
```
- 左下角的应用程序中找到 `Google Chrome`，并添加至收藏夹
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bf4d79b4ea31f8385a6f6f8ff7809da4.png#pic_center =600x)

- 查看 `Google Chrome` 软件源是否被添加

**当一个新版本发布时，这将确保你的 Google Chrome 可以被升级。**
```shell
cat /etc/apt/sources.list.d/google-chrome.list
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/897a1a37f2676b83e20e7d0c81eb9917.png#pic_center =800x)
## 2.3 安装搜狗输入法
- 进入 [Linux 版官网](https://pinyin.sogou.com/linux?r=pinyin)，下载安装包
- 按照自动弹出的 [安装指导](https://pinyin.sogou.com/linux/guide) 中 `20.04` 版本的进行配置
1. 安装 `fcitx` 输入法框架
```shell
sudo apt install fcitx
```
2. 设置 `fcitx` 为系统输入法
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/292995b74bfbb9b6cc9d94488f5a1371.png#pic_center =700x)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3d7e0ad529244ae633b186a84ae7e63c.png#pic_center =400x)

3. 安装搜狗输入法

>`dpkg` 命令进行安装
```shell
sudo dpkg -i xxx.deb
sudo dpkg -l so* # 查看是否安装好了
```
4. 安装输入法依赖
```shell
sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2

sudo apt install libgsettings-qt1
```
- 重启虚拟机
- 配置输入法（将搜狗输入法加进来）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/583d60462c23cca8602242c184d97097.png#pic_center =200x)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/723a86eab6f94b682f57c67d364dc1e6.png#pic_center =600x)
- 安装完成
- `Ctrl + 空格` 切换输入法

## 2.4 美化桌面、系统
>==参考文章：==
>-  [Ubuntu18.04 linux - 美化（gnome shell 插件）](https://blog.csdn.net/LawssssCat/article/details/108555682#t5)
>- [Ubuntu18.04配置与美化入门版（Gnome）](https://blog.csdn.net/Radium_1209/article/details/96479607)
>- [史上最良心的 Ubuntu desktop 美化优化指导(1)](https://zhuanlan.zhihu.com/p/63584709)

### 2.4.1 前提知识：什么是 GNOME ?
 - `GNOME` 是一套纯粹自由的计算机软件，运行在操作系统上，**提供图形桌面环境**
- `GNOME` 是 `Linux` 操作系统上**最常用的图形桌面环境之一**

### 2.4.2 查看 GNOME-Shell 版本
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a2c9953338007304acbc65f80c707a53.png#pic_center =600x)
### 2.4.3 安装 GNOME Tweaks
- 命令行安装
```shell
sudo apt-get install gnome-tweaks
```
- 在应用程序中搜 `tweaks` 双击即可打开

### 2.4.4 安装扩展插件
**方式一：命令行**
>个人感觉这种方式不是很灵活，需要查找所需扩展相应的命令行才能安装。
- 终端输入
```shell
sudo apt install gnome-shell-extensions
```
- 重启虚拟机，打开 `Gnome Tweaks`

可以看到多出来了一些扩展，如下图所示。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f9dd3102fea433e9bd2b60c0b7d6349a.png#pic_center =800x)


**方式二：Web 端**
> 这种方法需要 `fan qiang`（看前面的配置代理）
- `Google` 浏览器安装插件 `GNOME Shell integration`
- 虚拟机安装 `host connector`
```shell
sudo apt install chrome-gnome-shell
```
- 进入 [GNOME 扩展管理网站](https://extensions.gnome.org/)（可以注册个账号，保留数据）
> 你可以在这里找到并安装扩展，同时管理它们，甚至不需要用到 `GNOME Tweaks`！
- 选择想要安装的扩展
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b4527f581d4fede3a85e77292d0c85e0.png#pic_center =700x)
- 列举几个重要的扩展
> 可以看这篇文章 [2020 gnome 桌面插件推荐](https://zhuanlan.zhihu.com/p/270645604) 中列出的扩展是不是你需要的~

 扩展 | 作用 | 效果图
 --- | --- | ---
 `User Themes` | 从用户目录加载 `shell` 主题 | ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e5310e5311790aa0fcbcd0c2a9f7fde3.png#pic_center =500x)
`Dash to Dock` | 可以理解为 `Ubuntu` 的任务栏管理器 | ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/19d87f673c5a3fe59814d9574a0f7435.png#pic_center =500x)
`Dash to Panel` | `Dash to Dock` 和这个二选一，类似于 `Windows` 任务栏| ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/eb0d9fb1a826477ba005d1b7c1e41d16.png#pic_center =600x)
| `AlternateTab` | 让切屏变丝滑 | \

### 2.4.5 更改主题
> 一开始，我选择将主题更改为 `Flat Remix`。
> - 试了很久之后发现 `Ubuntu18.04` 的 `GNOME-Shell` 版本（`3.28`）不支持这个主题。。。
> - [安装教程](https://www.opendesktop.org/p/1013030) 里其实有提醒，但是我没想到 `18.04` 的版本已经被抛弃了
> ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d693df64e3bd6cc151c88a427fd2c450.png#pic_center =600x)
> - 已无法下载
> ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3b459f48e85f869ffe11b75939f41829.png#pic_center =600x)

最后我选用了 [GTK3/4 Themes](https://www.opendesktop.org/browse?cat=135&page=1&ord=rating) 中的 `Nordic`。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bb2a56f4be5058a827b76f0df3c9a180.png#pic_center =700x)
原因是它的这句话（支持早期版本的 `GNOME`）。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a62663b275207c380835ffd87f10f73a.png#pic_center =700x)

- 下载喜欢的主题配色
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7a9c317760c0d9d2ef796d6751644899.png#pic_center =600x)
- 解压后，将包含有 `index.theme` 文件的文件夹放入 `~/.themes` 文件夹
```shell
cd 
mkdir .themes
tar -xvf xxx.tar.xz -C ~/.themes
```
- 打开 `Tweaks` 进行配置
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/876d18894b999f6f4ce6103acea72c98.png#pic_center =600x)
- 重启虚拟机

### 2.4.6 更改壁纸
- 在 [wallhaven](https://wallhaven.cc/toplist?page=2) 中，选择自己喜欢的壁纸
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ef5910aedc1663e963576cb250b5696f.png#pic_center =800x)

- 选择相应的图片分辨率进行下载
- 在 `~` 中创建 `.wallpapers` 文件夹（用于放置壁纸）
- 打开软件，对 **背景** 和 **锁屏** 进行设置
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e8e35f224cdbd5715c37c94745d6ec40.png#pic_center =700x)
### 2.4.7 更改图标
>我选择的图标主题是 [Tela-icon-theme](https://www.opendesktop.org/p/1279924)

- 下载喜欢的主题配色
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/298392d031a11e2cb0d191830981783f.png#pic_center =800x)
- 解压至 `~/.icons` 文件夹中
```shell
mkdir ~/.icons
sudo tar -xvf xxx.tar.xz -C ~/.icons
```
- 打开 `Tweaks` 进行配置
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ff2bd32439dd65f5479b4bf1455428f6.png#pic_center =700x)
- 完成，看一下效果吧
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e5ac4153736f09c084d5f24c3ca11218.png#pic_center =700x)

### 2.4.8 更改鼠标光标
>我选择的鼠标主题是 [Oreo Cursors](https://www.opendesktop.org/p/1360254)

- 下载喜欢的主题配色
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/63afb96153fc86f6ae1f5d52a77a8d20.png#pic_center =700x)
- -解压至 `~/.icons` 文件夹中
```shell
sudo tar -xvf xxx.tar.xz -C ~/.icons
```
- 打开 `Tweaks` 进行配置
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/09e9c328a3bd9be315439e31335ea0c1.png#pic_center =700x)
- 完成，效果如下图
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/88cc1cc769e5cbf68fbb859a3c8a1a0f.png#pic_center =500x)


@[toc](配置 Jetson Nano 基础环境（一）)
# 1 将镜像烧录进 TF 卡
## 1.1 准备
- TF 卡，即 Micro SD卡（越大越好，`64G` 以上）
- TF 卡转接器

## 1.2 镜像下载
[官网下载](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bd3f237e7039d0d72d1cdf6734fcd1bd.png#pic_center =900x)
## 1.3 镜像烧录
操作很简单，就不多说了

- 格式化 TF 卡

>我用的这个格式化软件 `SD Memory Card Formatter`
>- [下载链接](https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/)

- 镜像烧录

>我用的这个软件 `Rufus`
>- [下载链接](http://rufus.ie/zh/)
>- [开源链接](https://github.com/pbatard/rufus)


完成上述步骤后，即可将 TF 卡插入 `Nano`，外接一个显示器，上电开机。当填写完一些基本设置后，就完成了系统的安装。
# 2 升级 Ubuntu 为 20.04（不建议）
>官网镜像中 `Ubuntu` 的版本是 `18.04`，但由于本人需要 `ROS2` 的开发环境，为了避免 `Python` 版本带来的不必要的麻烦，所以我选择将系统升级为 `20.04`。

==经过后续的使用，发现升级为 20.04 并不是一个好的选择，原因如下：==
- 原生系统中的 `cuda` 会被清除，需要自己安装
- 没有合适的 `pytorch` .whl 文件可以使用（系统 `JetPack` 为 `4.6`，默认 `python` 版本为 `3.8`，当你一些基础环境配好了之后，发现了这个问题会很绝望）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7b76501cd2cb1641983a4e4874a141d7.png)



参考文章：[Install Ubuntu 20.04 on Jetson Nano](https://qengineering.eu/install-ubuntu-20.04-on-jetson-nano.html)

跟着上面文章的步骤走，即可完成升级，过程可能需要将近 `1 ~ 2` 个小时的时间。


# 3 apt 换清华源
- 备份原文件
```bash
sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
sudo touch /etc/apt/sources.list
```
- 查询系统架构
```bash
arch
```

- 找 `arm64` 架构（`Ubuntu Ports` 镜像）

[点这里](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu-ports/)

- 写入 `sources.list`
```txt
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-proposed main restricted universe multiverse
```

- 更新
```bash
sudo apt update
sudo apt upgrade
```

# 4 安装 jtop
`Pypi` [官网介绍](https://pypi.org/project/jetson-stats/)：
>jetson-stats is a package for monitoring and control your NVIDIA Jetson [Xavier NX, Nano, AGX Xavier, TX1, TX2] Works with all NVIDIA Jetson ecosystem.

- 安装 `pip3`

```bash
sudo apt install python3-pip
```
- 安装 `jetson-stats`
```bash
sudo -H pip3 install -U jetson-stats
```
- 重启服务
```bash
sudo systemctl restart jetson_stats.service
```
- 重启 `Nano`
- 运行命令
```bash
jtop
```

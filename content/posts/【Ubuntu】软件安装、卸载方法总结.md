@[TOC](【Ubuntu】软件安装、卸载方法总结)
# 1 apt
了解 `apt` 的常用命令：[Linux apt 命令 | 菜鸟教程](https://www.runoob.com/linux/linux-comm-apt.html)
## 1.1 apt 与 apt-get
>参考文章：
>- [Linux中apt与apt-get命令的区别与解释](https://www.sysgeek.cn/apt-vs-apt-get/)
>- [[Linux]apt 和 apt-get 之间有什么区别？](https://zhuanlan.zhihu.com/p/350690109)

`apt` 相当于 `apt-get`、`apt-cache` 和 `apt-config` 中最常用命令选项的集合，应该首先使用 `apt`。

## 1.2 添加 apt 仓库（PPA）

> 参考文章：
> - [How To Add Apt Repository In Ubuntu](https://linuxize.com/post/how-to-add-apt-repository-in-ubuntu/)

>插入一个命令的讲解：
>`add-apt-repository`
>- [How to Use “add-apt-repository” Command in Ubuntu 22.04](https://linuxhint.com/add-apt-repository-command-ubuntu/)
>- [Ubuntu PPA 使用指南](https://zhuanlan.zhihu.com/p/55250294)
>---
>`PPA` 表示 个人软件包存档 (`Personal Package Archive`)
>在 [launchpad.net](https://launchpad.net/) 中查找 `PPA` 仓库

当你使用 `PPA` 时，它不会更改原始的 `/etc/apt/sources.list` 文件。相反，它在 `/etc/apt/sources.d` 目录中创建了两个文件，一个 `.list` 文件和一个带有 `.save` 后缀的备份文件。


- 添加相应软件的 `PPA` 仓库
```shell
sudo add-apt-repository <PPA_info> # 将 PPA 仓库添加到软件包列表中
sudo apt update # 更新软件包列表
sudo apt install <package_in_PPA> # 安装软件包
```
- 删除 `PPA` 仓库
```shell
sudo add-apt-repository --remove <PPA_info> # 将 PPA 仓库从软件包列表中删除
cd /etc/apt/sources.list.d
sudo rm xxx.list xxx.list.save # 删除空白的文件
sudo apt update # 更新软件包列表
```

## 1.3 apt 卸载软件包
-  卸载软件包（保留配置文件）
```shell
sudo apt remove xxx1 [xxx2 xxx3...]
```
- 卸载并清除软件包的配置
```shell
sudo apt purge xxx1 [xxx2 xxx3...]
```
- 对所有软件包进行清理（慎用）
> 我曾经因为卸载某个包，把整个 `ROS` 的依赖给卸载了...
```shell
sudo apt autoremove
```
**如果你因为这个命令删除了一些其他软件会用到的库，导致其他软件无法正常使用，还有非重装系统的补救方法：**
>如果你使用的是虚拟机，且电脑磁盘空间比较多，可以使用 **快照** 来进行回档

> 如果你使用的是物理机，可以按下面的步骤：
>1. 打开 `/var/log/apt/history.log`
>2. 找到你输入的命令所对应的历史信息
>![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2dd552942b0268fd3c6447c1803cd5ae.png#pic_center =1000x)
>3. 将你卸载的东西一个一个的 `install` 回来
> ````shell
> sudo apt install <package_name>=<version_number>
> ````

# 2 wget
`wget`（名字是 `World Wide Web` 与 `get` 的结合）不是安装方式，是一种软件安装包的下载方式。
如果要下载一个软件安装包（`.deb`），我们可以直接输入 `wget url`，通过 HTTP、HTTPS、FTP 三个最常见的 TCP/IP 协议下载。


# 3 dpkg


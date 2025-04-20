@[toc](【WSL 2】在 Windows10 上配置 WSL 2 连接 USB 设备 D435i)
# 1 Notes

- `WSL` 现在通过 Microsoft Store 同时支持 Windows10 和 Windows11，**这意味着 Windows10 用户现在可以访问最新的内核版本，而无需从源码编译**
- 如果无法更新到 Microsoft Store 支持的 `WSL` 版本并自动接收内核更新，则需要自己编译生成已启用 USBIP 的 `WSL 2` 内核


如果想要可视化的配置教程，可以参考此视频: [Video](https://www.youtube.com/watch?v=t_YnACEPmrM)

# 2 安装 USBIPD-WIN
>参考文档：[Link](https://learn.microsoft.com/en-gb/windows/wsl/connect-usb#install-the-usbipd-win-project)

 `WSL 2` 本身并不支持连接 USB 设备，所以需要安装 `usbipd-win`。

安装包下载: [Link](https://github.com/dorssel/usbipd-win/releases)，下载 .msi 文件，双击安装。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1960b1e4616ceef37248722df2294dca.png#pic_center)

# 3 连接 USB 设备

>参考文档：[Link](https://learn.microsoft.com/en-gb/windows/wsl/connect-usb#attach-a-usb-device)

在连接 USB 设备之前，请确保 Ubuntu Distribution 命令行已打开，以保持 `WSL 2` 处于活动状态。


1. 以**管理员模式**打开 PowerShell 并输入以下命令
```bash
usbipd list
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/05cf60ab1eb9a2b9a6da1d4213c24ddd.png#pic_center)

2. 复制要连接到 `WSL 2` 的设备的 `BUSID`，例如上图中的 1-14

3. 共享设备，使其能连接到 `WSL 2`

```bash
usbipd bind --busid <busid>
```

使用 `usbipd list` 命令验证设备是否已共享，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5c2825ea3216b7fad026e06a063d4783.png#pic_center)

4. 连接 USB 设备

==请注意，只要 USB 设备连接到 WSL 2，Windows 就无法使用它。==

```bash
usbipd attach --wsl --busid <busid>
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3d7aa39e390fccc7f74fcb3788ed9ae7.png#pic_center)

使用 `usbipd list` 命令验证设备是否已连接，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/af15aea491cd86e658eb5988dc891777.png#pic_center)


5.  打开 Ubuntu Distribution 命令行，列出所连接的 USB 设备

```bash
lsusb
```

下图是连接 USB 设备的前后对比。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/706043c929812bbf7f4144446a9323af.png#pic_center)

可能有些应用需要 `sudo` 才能使用此 USB 设备，后续可以通过配置 `udev` 规则，以允许非 root 用户访问设备。


6. 使用完后，可物理断开 USB 设备，或在 PowerShell 中执行 `usbipd detach --busid <busid>`


下次连接时，只需打开 PowerShell 并输入命令 `usbipd attach --wsl --busid <busid>`，但要确保 Ubuntu Distribution 命令行已打开，以保持 `WSL 2` 处于活动状态，否则会有报错，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2e533191f28ed492c98982244aa932c8.png#pic_center)

# 4 编译 WSL 2 内核

在 WSL 2 中 `lsusb` 可以看到 USB 设备，但是 `/dev` 文件夹里却没有，比如: `/dev/video0`。

>当出现这种情况时，说明需要编译内核来提供驱动，参考文档: [Link](https://github.com/dorssel/usbipd-win/wiki/WSL-support#building-your-own-wsl-2-kernel-with-additional-drivers)。

1. 更新 WSL
```bash
wsl --update
```

2. 确认使用的是 WSL 2

```bash
wsl --list --verbose
```

3. 打开 Ubuntu Distribution 命令行，安装依赖
```bash
sudo apt update
sudo apt upgrade
sudo apt install build-essential flex bison libssl-dev libelf-dev libncurses-dev autoconf libudev-dev libtool
```

4. 查询此 Ubuntu Distribution 内核版本
```bash
uname -r
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0cc8a5fa7d6d34c120527683457ab409.png#pic_center)

5. clone 相应版本的 WSL 2 Kernel 源码，我的是 5.15.146.1

```bash
git clone https://github.com/microsoft/WSL2-Linux-Kernel.git
cd WSL2-Linux-Kernel
git checkout linux-msft-wsl-5.15.146.1
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2b01c11ad2ffe321d894c47630a8efbb.png#pic_center)

6. 复制当前的配置文件

```bash
cp /proc/config.gz config.gz
gunzip config.gz
mv config .config
```

7. 确保 `.config` 文件中设置了 `CONFIG_USB=y`

```bash
vim .config
# /CONFIG_USB=
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/080e8b936e08a3d400218a79578c4871.png#pic_center)

8. 运行 `menuconfig` 选择要添加的内核功能

```bash
sudo make menuconfig
```

- 进入 Device Drivers

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0042c1fd9e6b2e65a876966eb1ba49eb.png#pic_center)


- 按 `Y` 选择 USB support，并进入

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bff594b76a3c8e00c12df3e1ac7d71c2.png#pic_center)
按 `Y` 选择以下内容，选中后会显示 `[*]`:

```txt
USB announce new devices
USB Modem (CDC ACM) support
USB/IP support
USB/IP support -> VHCI hcd
USB Serial Converter support -> USB FTDI Single Port Serial Driver
```

- 退出 USB support，按 `Y` 选择 Multimedia support，并进入

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a09cfb90f48c875fca809d126643db0b.png#pic_center)
按 `Y` 选择以下的选项，选中后会显示 `[*]`:

```txt
Filter media drivers
Media device types -> Cameras and video grabbers
Video4Linux options -> V4L2 sub-device userspace API
Media drivers -> Media USB Adapters -> USB Video Class (UVC)
Media drivers -> Media USB Adapters -> USB Video Class (UVC) -> UVC input events device support
Media drivers -> Media USB Adapters -> GSPCA based webcams
```


- 按两次 ESC，选择 Yes 保存并退出

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d7e33c107287d1ccb5074fccbe1283cc.png#pic_center)

9. 编译并安装

运行 `getconf _NPROCESSORS_ONLN` 查询内核数量，我的是 8

```bash
sudo make -j 8 && sudo make modules_install -j 8 && sudo make install -j 8
```

编译可能需要等待几分钟，完成后，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d2bfc06ff9d34eee6f20b3fc21276236.png#pic_center)


如果编译过程中出现下面的报错:
```txt
BTF: .tmp_vmlinux.btf: pahole (pahole) is not available
Failed to generate BTF for vmlinux
Try to disable CONFIG_DEBUG_INFO_BTF
```


解决方法:
```bash
sudo apt install dwarves
```

10. 复制镜像

```bash
cp arch/x86/boot/bzImage /mnt/c/Users/<user>/usbip-bzImage
```

11. 在 `/mnt/c/Users/<user>/` 上创建 `.wslconfig` 文件，并写入以下内容

```txt
[wsl2]
kernel=c:\\users\\<user>\\usbip-bzImage
```

12. 关闭 Ubuntu Distribution 命令行，打开 PowerShell 关闭 `WSL`，并连接 USB 设备

```bash
wsl --shutdown 
wsl --list --verbose
usbipd attach --wsl --busid <busid>
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5c0b6c193b0fbede3f316ccdf1495339.png#pic_center)

13. 重新打开 Ubuntu Distribution 命令行

```bash
lsusb
ls /dev/video*
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/08885f1f1ae55a596723b31d2cab45e0.png#pic_center)

可以发现，编译内核后，连接的 USB 设备就可以在 `/dev` 中找到。

# 5 什么是 .wslconfig 文件
> 参考文档: [Link](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config#wslconfig)

- 默认情况下，`.wslconfig` 文件不存在，并且它必须创建在 `%UserProfile%` 目录中才能应用这些设置，例如我的路径是 `C:\\Users\\chenh\\.wslconfig`
- 只能用于 WSL 2 运行的发行版
- **为使用 WSL 2 运行的所有已安装 Linux 发行版全局配置设置**
- 更改内容后，需要运行 `wsl --shutdown` 来关闭 WSL 2 VM，然后重启 WSL 实例以使更改生效

如果在使用其他 `WSL 2` 的 Linux 发行版时不需要连接 USB 设备，记得要将 `.wslconfig` 文件的内容用 `#` 注释掉，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/abd8eeb4111caa05ec78fb91092de857.png#pic_center)


# 6 使用 D435i 测试

## 6.1 安装 cheese

```bash
sudo apt install cheese
```

## 6.2 测试

可以在 `Preferences` 里修改 Device 和分辨率。

```bash
cheese
```

显示效果如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/018ab9225efa2728da4de1785b967113.png#pic_center)




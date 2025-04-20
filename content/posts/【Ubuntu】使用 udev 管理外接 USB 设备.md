@[toc](使用 udev 管理外接 USB 设备)

# 1 udev 是什么
>参考: [udev - Wikipedia](https://en.wikipedia.org/wiki/Udev#:~:text=The%20udev%2C%20as%20a%20whole%2C%20is%20divided%20into,%2Fdev.%203%20Administrative%20command-line%20utility%20udevadm%20for%20diagnostics.)

## 1.1 概述
`udev`（`userspace /dev`）是 Linux 内核中的一个设备管理器。作为 `devfsd` 和 `hotplug` 的继承者，`udev` 主要管理 `/dev` 目录下的设备节点。
同时，`udev` 还处理当硬件设备被添加到系统中或从系统中移除时引发的所有用户空间事件，包括某些设备所要求的固件加载。

## 1.2 三个部分
`udev` 作为一个整体，可分为三个部分：

- Library libudev that allows access to device information; it was incorporated into the systemd 183 software bundle.

`libudev` 库，允许访问设备信息
- User space daemon udevd that manages the virtual /dev.

用户空间守护程序 `udevd`，管理虚拟 `/dev`
- Administrative command-line utility udevadm for diagnostics.

命令行管理工具 `udevadm` 用于诊断



# 2 添加 rules 规则
- 

@[TOC](【Ubuntu】双硬盘安装双系统 Windows 和 Ubuntu)
# 1 安装顺序

我选择先在一块 SSD 上安装 Windows 再在另一块 SSD 上安装 Ubuntu，建议先安装 Windows

# 2 Ubutnu 20.04
## 2.1 准备工作

1. 制作启动盘
- 下载系统镜像 `Desktop image`: [Link](https://www.releases.ubuntu.com/focal/)
- 使用 Rufus 制作 U 盘启动盘: [Link](https://rufus.ie/en/)

2. Bios 相关
- 如果已经安装 Windows，查询是否关闭安全启动：`win + r` 执行 `msinfo`

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cccfed27666d4880ba3ed51ddd16eda2.png#pic_center)

- 了解本机的启动项快捷键

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ac5511ae02a94a3eb5df939e5a76cc40.png#pic_center)


## 2.2 自定义分区

1T 的 SSD 分区如下：

- `swap`：内存大小的 1 - 2 倍
- `/boot`：500 MB
- `/`：240G
- `/home`: 剩下所有的空间 700G


选择 `boot loader` 的安装位置：Ubuntu 所在的 SSD

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e025f1265ac74e9db85d34d6d51883d0.jpeg#pic_center)

## 2.3 遇到的一些问题

- 当安装过程没有连接网络时，系统中会少一些东西，比如：`gcc`、`g++`

```bash
sudo apt update
sudo apt install build-essential
```


- 当没有独立显卡时，安装完成后进入 Ubuntu20.04 系统会卡在这个界面

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8c87ef97429f460c90ac62443384e5f2.jpeg#pic_center)

但是同样没有独立显卡，进入 Ubuntu22.04 系统是正常的。


@[TOC](【WSL 2】Windows10 安装 WSL 2，并配合 Windows Terminal 和 VSCode 使用)

# 1 安装 Windows Terminal

> 官方文档: [Link](https://learn.microsoft.com/zh-cn/windows/terminal/install)

在 Microsoft Store 中获取: [Link](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=zh-cn&gl=cn&rtc=1)

# 2 安装 WSL 2
> 官方文档: [Link](https://learn.microsoft.com/zh-cn/windows/wsl/install)

1. 确定安装的 Linux 发行版

列出所有的可用发行版（默认情况下，安装的 Linux 分发版为 Ubuntu）
```bash
wsl --list --online
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/01c4e8ec89086b8d739919de92f32ea6.png#pic_center)


2. 安装 Linux 发行版
```bash
wsl --install -d Ubuntu-20.04
```

3. 设置 Linux 用户名和密码

4. 查看 WSL 版本（如果是 WSL 2，可跳过下一步）
```bash
wsl -l -v 
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9c678e6867bb9923396e49445058a262.png#pic_center)

5. 从 WSL 1 升级到 WSL 2

> 官方文档: [Link](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3b3471a004a0a8eaa5ab90870fb89406.png#pic_center)

- 启用适用于 Linux 的 Windows 子系统
```bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
- 启用虚拟机功能
```bash
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
- 下载并运行 Linux 内核更新包: [Download Link](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
- 将 WSL 2 设置为默认版本（后面再安装就无需更改了）
```bash
wsl --set-default-version 2
```
- 更改已安装 Linux 的 WSL 版本
```bash
wsl --set-version Ubuntu-20.04 2
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/839c7fb53da7aad3d431ff3b40f12986.png#pic_center)

- 确认现在的版本
```bash
wsl -l -v
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/41a5aa0cd0fd3b26c0aa79870f543959.png#pic_center)

6. 使用 Windows Terminal 打开 Linux 发行版

- 在 Windows Terminal 中输入
```bash
wsl
```
或是 Distribution Name
```bash
Ubuntu-20.04
```
- 直接在 Windows Terminal 中打开
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/944929e7f9e1f8f25233ae79d2bbf7f7.png#pic_center)

# 3 在 Windows 文件资源管理器中打开 WSL 项目

1. 使用 Windows Terminal 打开 Linux
2. `cd` 到想要打开的目标路径
3. 输入下面的命令

其中 `.` 表示打开当前所在路径
```bash
explorer.exe .
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/24db5d81dac40f71d7c8ee25b86b3c17.png#pic_center)

# 4 在 VSCode 中使用  WSL 2

> 官方文档:  [Link](https://code.visualstudio.com/blogs/2019/09/03/wsl2)

## 4.1 必要准备

1. 在 VSCode 中安装插件 WSL

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f8e4ac66f6871adfec2a784f74ef6a76.png#pic_center)

2. 使用 Windows Terminal 打开 Linux

某些 WSL Linux 发行版缺少启动 VS Code 服务器所需的库，先更新一下

```bash
sudo apt update
sudo apt upgrade
```

## 4.2 从 VSCode 中 Connect WSL

1. 打开 VSCode，点击左下角图标
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ac61eb1d2f418a8b003ff731fce2dfe1.png#pic_center)
2. 点击这两个中的其中一个

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/73dc93d8e03557d2df730428a1e640a3.png#pic_center)

3. 选择要打开的 Linux 发行版

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/62b73db959048a1b4edf578bc4aae2de.png#pic_center)

4. 打开后，左下角会显示当前的连接状态

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/60113db184b07cc9ce54593148456c23.png#pic_center)

5. 选择要打开的文件系统路径即可

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ac3bd2909027c78e158ccfbc73e7f988.png#pic_center)
## 4.3 从 Linux 中打开 VSCode


1. 使用 Windows Terminal 打开 Linux

2. `cd` 到想要打开的目标路径

3. 输入下面的命令

```bsah
code .
```
即可直接打开该目录的文件系统

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0e891c892bbcc5a2f10bef3b01cc0a97.png#pic_center)


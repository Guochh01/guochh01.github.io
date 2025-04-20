@[TOC](【服务器】在 Linux CLI 下安装 Anaconda)
# 1 系统环境
1. 查看系统信息
```bash
cat /etc/os-release
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/60b2909ca413579ea72c389445a9648b.png#pic_center)
2. 查看架构
```bash
uname -a
# output
# Linux localhost.localdomain 4.18.0-193.28.1.el8_2.x86_64 #1 SMP Thu Oct 22 00:20:22 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

# 2 下载安装包
>官方地址: [Link](https://www.anaconda.com/download#downloads)

1. 选择安装包，右键复制链接

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0171b64cda529ce6532fb938bd64c243.png#pic_center)

如果需要安装其他版本，可以在这里找: [Link](https://repo.anaconda.com/archive/)

2. wget 下载: `wget -P [指定安装路径] url[url为复制的链接]`

```bash
 wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
```
# 3 安装
> 参考文档: [Link](https://docs.anaconda.com/anaconda/install/linux/#)

1. 运行 .sh 安装脚本
```bash
bash Anaconda3-2023.09-0-Linux-x86_64.sh
```
- 长按 `Enter`，Do you accept the license terms? [yes|no]: `yes`
- Anaconda3 will now be installed into this location: `Enter` (建议安装至用户目录下)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2bfdbf02e2cd065705f7c70d04041ea0.png#pic_center)

- You can undo this by running \`conda init --reverse $SHELL\`? [yes|no]: `no`（如果这里选择了 `yes`，可直接跳至第 3 步）

完成后，终端显示: Thank you for installing Anaconda3!

2. 初始化 conda
```bash
source ~/anaconda3/bin/activate
conda init
```

3. (Optional) 设置打开终端不自动进入 conda 的 base 环境，即需要自己激活相应的虚拟环境
```bash
source ~/.bashrc
conda config --set auto_activate_base false
```

4. 验证

```bash
source ~/.bashrc
conda env list
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fb560678597928ead99206928fd6d1c6.png#pic_center)





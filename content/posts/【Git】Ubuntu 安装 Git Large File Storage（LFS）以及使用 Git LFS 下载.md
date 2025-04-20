@[TOC](【Git】Ubuntu 安装 Git Large File Storage（LFS）以及使用 Git LFS 下载)

# 1 安装

## 1.1 使用脚本安装
>参考文档: [Link](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage?platform=linux)

1. 下载安装包: [Link](https://git-lfs.com/)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bf32eadb2ea74f9c810c2d65042a4170.png#pic_center)

2. 解压安装包

```bash
tar -xzvf git-lfs-linux-amd64-v3.6.1.tar.gz
```

3.  执行安装脚本

```bash
cd git-lfs-3.6.1
sudo ./install.sh
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5f29f007cad94c66ab99977dc524ee71.png#pic_center)

4. 设置 Git LFS（每个用户只需运行一次）

```bash
git lfs install
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ebbf420daf2344648e6022fbcbcdfec0.png#pic_center)


## 1.2 使用 packagecloud 安装
>参考文档: [Link](https://github.com/git-lfs/git-lfs/blob/main/INSTALLING.md)

1. 添加仓库

```bash
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
```

2. 安装

```bash
sudo apt install git-lfs
```

# 2 使用

## 2.1 下载

可以直接使用 `git clone` 来下载相应的仓库，但是它有个缺点是无法显示 LFS objects 的下载进度，有一种陷入假死的感觉。

下面介绍两个可以显示 LFS objects 下载进度的方法，以 `stable-video-diffusion-img2vid-xt` 库为例（使用 Hugging Face 的国内镜像 HF-Mirror）：

- 方法一： `git lfs clone` 可能在 Git LFS (v4.0.0) 中被移除，参考 issue: [Link](https://github.com/git-lfs/git-lfs/issues/5544)
```bash
git lfs clone https://hf-mirror.com/stabilityai/stable-video-diffusion-img2vid-xt
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/084669c94bac4d2d936145d4674490ce.png#pic_center)


- 方法二：分两步进行，先下载小文件，再下载 LFS objects
```bash
# If you want to clone without large files - just their pointers
GIT_LFS_SKIP_SMUDGE=1 git clone https://hf-mirror.com/stabilityai/stable-video-diffusion-img2vid-xt
cd stable-video-diffusion-img2vid-xt
git lfs pull
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5efe43aa7b8c4e4e87c71070b70d0389.png#pic_center)


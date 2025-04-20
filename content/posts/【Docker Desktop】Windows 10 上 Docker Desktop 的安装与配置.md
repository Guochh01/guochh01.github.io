@[toc](【Docker Desktop】Windows 10 上 Docker Desktop 的安装与配置)

# 1 必要准备
## 1.1 概述
>官方文档: [Docker overview](https://docs.docker.com/get-started/overview/)


## 1.2 容器和镜像

>[关于docker容器和镜像的区别](https://www.cnblogs.com/baizhanshi/p/9655102.html)

- 容器是镜像的实例，类似于面向对象中的类与其实例化，也可以说镜像是文件, 容器是进程。
- 容器是基于镜像创建的, 即容器中的进程依赖于镜像中的文件, 这里的文件包括进程运行所需要的可执行文件， 依赖软件， 库文件， 配置文件等。

# 2 修改 Docker Desktop 的默认安装路径（Optional）

1. 确保 `C:\Program Files` 路径中没有 `Docker` 文件夹
2. 在想要的安装路径中新建 `Docker` 文件夹
3. 管理员模式打开 Windows Terminal

创建目录链接，其中 `F:\Docker` 为想要安装的位置
```bash
mklink /j "C:\Program Files\Docker" "F:\Docker"
```
4. 下载并运行安装包: [Link](https://docs.docker.com/desktop/install/windows-install/)
5. 安装完成后，可以发现 `C` 盘占用没有发生变化

# 3 修改本地镜像的存储位置（Optional）
原存储路径：`C:\Users\GCH\AppData\Local\Docker\wsl`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/895b553e6faf4a8504f0e86e50c5f8c9.png#pic_center)

1. 停止运行中的容器

查询是否有运行中的容器
```bash
docker ps
```
2. 右键图标，点击 `Quit Docker Desktop`，退出 Docker Desktop

Docker Desktop 安装了两个特殊用途的内部 Linux 发行版: `docker-desktop` 和 `docker-desktop-data`（两者都不能用于一般开发）

- 第一个（docker-desktop）用于运行 Docker engine
- 第二个（docker-desktop-data）用于存储 containers 和 images


```bash
# 查询 Docker Desktop 运行状态
wsl -l -v
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7525161920e070cdc2795d19fd76c15c.png#pic_center)

3. Export, unregister then import

只迁移 `docker-desktop-data` 即可，若已有的 images 较大，这个过程会耗点时间。
```bash
wsl --export docker-desktop-data F:\docker-desktop-data.tar
wsl --unregister docker-desktop-data
wsl --import docker-desktop-data "F:\\docker_images" "F:\\docker-desktop-data.tar" --version 2
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5e29abeb8a1617f2f2eef242f3b5ed2b.png#pic_center)

4. 重新启动 `Docker Desktop`
5. `pull` 一个镜像，测试一下

没问题的话，就可以把 `docker-desktop-data.tar` 压缩文件删掉了

# 4 设置代理

> 如果有梯子，可以设置相应的代理。

下面以 v2rayN + Docker Desktop 为例：


1. v2rayN 的 Dashboard 左下角可以看到 http 代理端口号，记为 `port`
2. 打开 Docker Desktop，Settings --- Resources --- Proxies

在 http 和 https 中填入

```txt
http://127.0.0.1:port
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fbf0a9342fd99cf223ba0a23de333e31.png#pic_center)


# 5 修改 Docker Hub 国内镜像

> 如果设置了代理，可以不修改 Docker Hub 镜像

1. 打开 Docker Desktop，Settings --- Docker Engine


![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b78757c42584c9e98d1ac7ff4b5a2688.png#pic_center)

2. 在 `registry-mirrors` 中添加国内镜像

- 阿里云镜像：需要先创建账号，每个人有不同的域名前缀
![!\[在这里插入图片描述\](https://img-blog.csdnimg.cn/b35386db716d40228af907351f807254.png](https://i-blog.csdnimg.cn/blog_migrate/2362bb63819474094fb5b0c5899cc72c.png#pic_center =900x)
- Docker 中国镜像
```txt
https://registry.docker-cn.com
```

3. 点击 `Apply & Restart`
4. 测试

pull 一个镜像会发现速度快了很多

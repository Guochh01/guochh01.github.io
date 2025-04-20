@[TOC](【Docker】在 Ubuntu20.04 上配置 Docker 开发环境)

# 1 安装 Docker
> 参考文档: [Link](https://docs.docker.com/engine/install/ubuntu/)

- 卸载以避免冲突
```bash
 for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
- 设置 Docker 的 apt 仓库
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

也可以改用清华源的镜像: [Link](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- 安装
```bash
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- 验证是否成功
```bash
sudo docker run hello-world
```

# 2 加入 Docker 用户组

为了免去每次输入 docker 命令都需要 `sudo`

1. 查询有无 docker 用户组
```bash
cat /etc/group | grep docker
```
如果有，直接跳至 `3`

2. 创建 docker 用户组
```bash
sudo groupadd docker
```

3. 将当前用户加入 docker 组
```bash
sudo gpasswd -a $USER docker
```

查看当前用户是否在 docker 用户组中
```bash
cat /etc/group | grep docker
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cef0fb259117e123e228deea90e427fa.png#pic_center =600x)

4. 重启
```bash
sudo service docker restart
sudo reboot
```

5. 测试是否成功
```bash
docker images
```

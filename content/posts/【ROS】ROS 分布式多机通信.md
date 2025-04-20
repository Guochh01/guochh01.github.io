@[TOC](【ROS】ROS 分布式多机通信)

# 一、前提条件
 - 查询时间是否同步
```shell
date
```


 - 安装 `ssh` 服务
```shell
sudo apt-get install openssh-server
```

# 二、修改 /etc/hosts 文件
1. `hostname` 查询名称
>本文中主机为 `ros1`，从机为 `XIAOMI`
2. `ifconfig` 查询主机和从机的 `ip` 地址
> **主机**

图片就不放了

> **从机**

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b9e8f8d73e65415ca9da1e7f0c8b3fea.png)

3. 修改主机和从机的 `hosts` 文件
> **主机**

- 输入命令
```shell
sudo vim /etc/hosts
```
- 修改内容（添加`从机的 ip 地址与其 hostname`）
>用 Tab 键对齐 hostname
```shell
127.0.0.1	localhost
127.0.1.1	ros1

# 添加在这里
192.168.1.133	XIAOMI

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

> **从机**
- 输入命令
```shell
sudo vim /etc/hosts
```
- 修改内容（添加主机的` ip 地址与其 hostname`）
>用 Tab 键对齐 hostname
```shell
127.0.0.1	localhost
127.0.1.1	XIAOMI

# 添加在这里
192.168.1.135	ros1

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

# 三、修改 .bashrc 或 .zshrc 文件
1. 在主机（即工控机）的 `.bashrc` 或 `.zshrc` 文件中加入
```shell
export ROS_HOSTNAME=ros1
export ROS_MASTER_URI=http://ros1:11311
```
2. 在从机（即自己控制的PC端）的 `.bashrc` 或 `.zshrc` 文件中加入
```shell
export ROS_HOSTNAME=XIAOMI
export ROS_MASTER_URI=http://ros1:11311
```
>**注意事项：**
>- 在修改后，最好 `source .bashrc` 或 `source .zshrc` 对终端进行更新
>- 如果设置了开启新终端时自动 `source`，也可以打开一个新终端来执行后面的命令

# 四、测试
1. PC端终端使用 `ssh` 连接工控机

- 连接前可以先 `ping` 一下测试 `ip` 是否正确。
- `ssh` 客户端用户名@客户端p地址
 >`ssh ros1@192.168.32.21` （需要输入客户端的密码才可完成连接）

客户端与主机最好处在`同一局域网内`（ WIFI 或 热点 ），这样可以直接进行连接，无需其他操作。

2. 在终端中输入 `roscore`

3. 开启另一终端运行 `rviz`
如果成功打开，即完成。

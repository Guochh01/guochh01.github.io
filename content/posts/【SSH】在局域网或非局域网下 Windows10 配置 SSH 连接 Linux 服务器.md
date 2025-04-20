@[TOC](【SSH】在局域网或非局域网下 Windows10 配置 SSH 连接 Linux 服务器)
# 1 环境介绍
- 服务端（被连接方）：`Ubuntu20.04`
- 客户端（连接方）：`Windows10`、`VSCode`

# 2 服务端配置（Ubuntu20.04）
1. 安装 OpenSSH

```bash
sudo apt install openssh-server openssh-client
```

2. 开启 ssh 服务

```bash
sudo systemctl enable ssh
```

3. 查看 ssh 服务状态

```bash
sudo systemctl status ssh
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/44794f011ff2f8d6abffc08f9936aecf.png#pic_center)


# 3 客户端配置（Windows10）
> 官方文档：[Link](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)

## 3.1 安装 OpenSSH

1. 打开**设置**，选择**系统**>**可选功能** 

2. 扫描列表，查看是否已安装 OpenSSH。 如果未安装，请在页面顶部选择**添加功能**

- 查找**OpenSSH 客户端**，点击**安装**
- 查找**OpenSSH 服务器**，点击**安装**


3. 安装完成后，查看已安装功能中是否有 **OpenSSH 客户端** 和 **OpenSSH 服务器**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/de8b13c2df4d8b7b00acad0a398d4df2.png#pic_center =600x)
> 在 Windows 终端中可执行的命令

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ff0e52ae3ae92a9605f31c877cdd1b2e.png#pic_center =600x)

4. 启动 OpenSSH SSH Server（Optional）

>当 Windows10 作为被连接方时，才需要启动 SSH Server

- `win + r`，输入 `services.msc` 打开**服务**
- 双击 OpenSSH SSH Server

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0d2bc2de7a2d3a8daf015af562b7bd72.png#pic_center)

- 启动类型选择自动，服务状态选择启动，点击确定

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/89b8afc37e5b686c811a40945225e71d.png#pic_center =500x)

## 3.2 生成密钥
1. 打开终端输入

> 若不加 `-t` 参数，默认为 `rsa` 
```bash
ssh-keygen -t ed25519
```
- 选择生成密钥的路径，可以按 `Enter` 来接受默认值
- 系统会提示你使用密码来加密你的私钥文件（这个密码要记住）
>可以为空，但不建议这样做。 将密码与密钥文件一起使用来提供双因素身份验证。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/51cca3b964a4fc632874ce309bc5a6a6.png#pic_center =700x)

2. 生成 `id_ed25519` 和 `id_ed25519.pub` 两个文件

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5f87621e95c572534ac3407bfa14d163.png#pic_center)


## 3.3 VSCode 配置
1. 安装 Remote - SSH 插件
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1fc7d6fbdf1f0cb6ce6819c9b0a97f94.png#pic_center =700x)
2. 加入远程服务器
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e7ddb87da051e135b38c158323fa5f20.png#pic_center =800x)
3. 配置 `config` 文件
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/221e363894a73e9628a4fb7d8c73ee16.png#pic_center =800x)


大概内容如下：
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/238b1f4ea8ed7f268061153a92a67ec9.png#pic_center =500x)

> `Host`: 自定义名称
> `HostName`: 服务端 **ip 地址或域名**
> `User` 服务端**用户名**
> `Port`: 端口号（如果是 `22`，则不需要配置）

==以上配置完成后，在终端执行 `ssh Host`，比如: `ssh intel`，即可连接。==

4. 开启连接
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e53b775c14ab0d031f6e2e77316972c2.png#pic_center =600x)
 
 6.  左下角可以看到已连接的标志，点击 open folder 可以访问服务端的文件系统
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9b42d84507ad1ca4a88fd3228bd68bca.png#pic_center =800x)
 
# 4 部署公钥（免密登录）

1. 确保存在 `.ssh` 文件夹
```bash
# 可用于执行单个命令
ssh -p xxx HOSTNAME@ip mkdir ~/.ssh
```

2. Windows Terminal 中执行

```bash
scp -P xxx C:\Users\USERNAME\.ssh\id_ed25519.pub HOSTNAME@IP:~

ssh -p xxx HOSTNAME@IP
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys 
rm ~/id_ed25519.pub

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
如果完成了 3.3 小节中的 `.ssh/config` 文件配置 ，则同样可以用 `HOST` 代替 `HOSTNAME@IP`，比如: `scp -P xxx C:\Users\USERNAME\.ssh\id_ed25519.pub intel:~`。

==可以借助这个命令来传输一些文件==，若传输文件夹，则加 `-r` 参数。
```bash
# 从本机到另一台电脑传输
scp -P xxx windows文件 HOSTNAME@IP:linux路径
# 从另一台电脑到本机传输
scp -P xxx HOSTNAME@IP:linux文件  windows路径
```

3. 验证
```bash
ssh -p xxx HOSTNAME@IP
```
若不需要输入密码，即完成配置。

# 5 .ssh 文件夹中各个文件的作用
> 参考文章: [Link](https://blog.csdn.net/qq_16268979/article/details/108899178)


| 文件名 | 功能 |
|:--------:|:-------------:|
| `id_xxx` | 经过 `xxx` 算法生成的私钥 |
| `id_xxx.pub` | 经过 `xxx` 算法生成的公钥 |
| `authorized_keys` | 存放需要免密登录的客户端公钥，实现免密连接 |
| `known_hosts` | 记录访问过的计算机的公钥 |
| `config`| 可以不用输入 `HOSTNAME@ip`，用其中的 `Host` 名代替 |

# 6 非局域网内使用 ssh
## 6.1 服务端（Ubuntu）配置
- 查询硬件架构
```bash
arch
# output
# x86_64
```
```bash
uname -a
# output
# Linux localhost.localdomain 4.18.0-193.28.1.el8_2.x86_64 #1 SMP Thu Oct 22 00:20:22 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```
- 下载[花生壳](https://hsk.oray.com/download/)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/378e50523b8cccb1b9b38d06d59fb991.png#pic_center =800x)


- 安装
```bash
cd ~/下载
# 自己在官网复制
wget "https://down.oray.com/hsk/linux/phddns_5.2.0_amd64.deb" -O phddns_5.2.0_amd64.deb
sudo apt install phddns_5.2.0_amd64.deb
# 若存在依赖相关错误，则执行下面的命令
sudo apt install -f
```
>下载完成后，会有如下输出（左下角的 `SN` 码有用）：
>也可以通过 `phddns status` 命令查询

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2320bbcdaeaeab6579945f2f3727c535.png#pic_center =900x)

- 登录[花生壳管理平台](http://b.oray.com)（上图中的地址信息）
>第一次登录需要激活认证

- 配置内网穿透
1. 领取免费映射
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/27bb90bac3583ae772a2db167c5830e2.png#pic_center =800x)

2. 新增映射
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/71eaafcae1a90b2e3de3813c898c3a71.png#pic_center =800x)

3. 填写**内网主机地址（即真实的 `ip` 地址）和端口（`ssh` 默认使用端口号 `22`）**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8db5408e4c2dd3ab20b9b9d97e023880.png#pic_center =800x)
4. 诊断，查看是否连接成功
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9199020c434d0584452c65f29183d752.png#pic_center =800x)

## 6.2 客户端（Windows）配置
>可以直接使用 `ssh` 命令进行连接
```bash
ssh -p xxx HOSTNAME@外网域名
```
>简单配置一下免密连接
- 传输公匙给服务端
```bash
scp -P xxx C:\Users\USERNAME\.ssh\id_ed25519.pub HOSTNAME@外网域名:~

ssh -p xxx HOSTNAME@外网域名
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys 
rm ~/id_ed25519.pub

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
不想用命令也可以手动复制。

- 配置 `config` 文件
> `Host`: 自定义名称
> `HostName`: 可以是 `ip` 地址，也可以是域名
> `Port`: 端口号
> 
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/906365378633d93e1839d27cef64414c.png#pic_center =600x)

- 连接测试
>使用 `ssh` 命令连接
```bash
ssh -p xxx orb_out
```
>使用 `VSCode` 连接
>
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/362dcb4033bd6e95c9203765481df693.png#pic_center =600x)

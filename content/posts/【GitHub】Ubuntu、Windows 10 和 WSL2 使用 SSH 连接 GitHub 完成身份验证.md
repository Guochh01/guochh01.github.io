@[TOC](【GitHub】Ubuntu、Windows 10 和 WSL2 使用 SSH 连接 GitHub 完成身份验证)

# 1 远程仓库
> 参考文档: [Link](https://docs.github.com/zh/get-started/getting-started-with-git/about-remote-repositories)

远程 URL 是 Git 一种指示“代码存储位置”的绝佳方式。用户只能推送到两类 URL 地址：
- HTTPS URL，如 `https://github.com/user/repo.git`
- SSH URL，如 `git@github.com:user/repo.git`
## 1.1 使用 HTTPS URL clone
HTTPS URL 在所有存储库上都可用，在命令行上使用 HTTPS URL 对远程仓库执行 git clone、git fetch、git pull 或 git push 时，Git 将要求你提供 GitHub 用户名和密码。 
## 2.2 使用 SSH URL clone
SSH URL 通过 SSH（一种安全协议）提供 Git 仓库的访问权限。 若要使用这个 URL，必须在 PC 上生成 SSH 密钥对，并将公钥添加到 GitHub 帐户中。

如果不在 GiHub 账户中添加公钥，则会在 `git clone` 的时候输出下面的报错信息。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/28396337d4bc93844ec3384eb0ea4589.png#pic_center)

SSH 密钥由两个部分组成：

- 私钥（Private Key）：保密的部分，应该只由密钥的所有者保存。SSH 中，私钥文件通常是 `~/.ssh/id_xxx`
- 公钥（Public Key）：公开的部分，可以安全地共享给其他人。SSH 中，公钥文件通常是 `~/.ssh/id_xxx.pub`

# 2 Ubuntu SSH 身份验证

>参考文档: [Link](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys?platform=linux)


1. 查看是否存在SSH 密钥

```bash
ls -al ~/.ssh
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ae2bbc22279644d29248ba53c6788485.png#pic_center)

2. 生成新 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

如果已经存在一个密钥，建议重新命名

```bash
Enter file in which to save the key (/home/xxx/.ssh/id_ed25519): /home/xxx/.ssh/id_ed25519_new
```


3. 后台启动 ssh 代理

```bash
eval "$(ssh-agent -s)"
```

4. 将 SSH 私钥添加到 ssh-agent

```bash
ssh-add ~/.ssh/id_ed25519
```

5. 将 SSH 公钥添加到 GitHub 帐户

- 复制公钥内容

```bash
cat ~/.ssh/id_ed25519.pub
```

- GitHub 的 `Setting` 中找到 `SSH and GPG keys`，点击 `New SSH key`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/29a7f2ebee70f784a8b9ded9439dc038.png#pic_center)

- `Title` 自定义名称，`Key` 中粘贴公钥的内容

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d27275332afef36a71d8d693e291ff51.png#pic_center)

6. 测试 SSH 连接

```bash
ssh -T git@github.com
```
- 如果打印如下信息，即成功：`Hi xxxx! You've successfully authenticated, but GitHub does not provide shell access.`
- 如果出现连接超时: `ssh: connect to host github.com port 22: Connection timed out`，则通过 HTTPS 端口使用 SSH

> 官方文档: [Link](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)

在 `~/.ssh/config` 文件中加入以下内容：

```txt
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

# 3 Windows 10 SSH 身份验证

>参考文档: [Link](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/about-authentication-to-github#authenticating-with-the-command-line)

可以通过两种方式从命令行访问 GitHub 上的仓库：HTTPS 和 SSH，两者采用不同的身份验证，验证方法取决于 clone 仓库时选择的是 HTTPS 还是 SSH URL。
## 3.1 HTTPS
如果不使用 GitHub CLI 进行身份验证，则必须使用 personal access token。

> 参考文档: [Link](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

具体配置参考上述文档，很少用到就不搞了。
## 3.2 SSH


1. 在生成新的 SSH 密钥之前，检查本地计算机是否存在现有密钥
> 参考文档: [Link](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

- 打开 Git Bash
- 输入 `ls -al ~/.ssh`

检查 `.ssh` 目录是否存在或者 `.ssh` 目录中是否已经有 SSH 公钥（文件后缀名为 `.pub`）。

2. 生成新 SSH 密钥以用于身份验证，然后将其添加到 ssh-agent

> 参考文档: [Link](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- 以管理员身份运行 PowerShell

- 替换为自己的邮箱，创建新 SSH 密钥
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
按 Enter 键接受默认文件位置（若要更改密钥文件的名称，先输入默认文件位置，再将 id_ed25519 替换为自定义名称）→ 按 Enter 键不设置密码。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b4549aead9ed5eb1ae58d0b3159cd24f.png#pic_center)

- 可以在这里 `C:\Users\{username}\.ssh` 找到生成的密钥和公钥
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/242f4538a99aa0668d1845c879ceb977.png#pic_center)

- 确保 ssh-agent 正在运行
```bash
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
```
- 将 SSH 密钥添加到 ssh-agent
```bash
ssh-add c:/Users/{username}/.ssh/id_ed25519
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c0b12037163931229b618aff48a8e0dc.png#pic_center )

3. 将 SSH 公钥添加到 GitHub 帐户

> 参考文档: [Link](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

- 以管理员身份运行 PowerShell

- 复制 SSH 公钥
```bash
cat ~/.ssh/id_ed25519.pub
```
- GitHub 的 `Setting` 中找到 `SSH and GPG keys`，点击 `New SSH key`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/29a7f2ebee70f784a8b9ded9439dc038.png#pic_center)

- `Title` 起个名字，`Key` 中填刚刚复制的公钥内容
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d27275332afef36a71d8d693e291ff51.png#pic_center)


4. 测试 SSH 连接

```bash
ssh -T git@github.com
```
- 如果打印如下信息，即成功：`Hi xxxx! You've successfully authenticated, but GitHub does not provide shell access.`
- 如果出现连接超时: `ssh: connect to host github.com port 22: Connection timed out`，则通过 HTTPS 端口使用 SSH

> 官方文档: [Link](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)

在 `.ssh/config` 文件中加入以下内容：

```txt
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

# 4 WSL2 SSH 身份验证

>前提是 Windows 已完成上述配置


1. 使用 Windows 创建的密钥
```bash
cat /mnt/c/Users/{username}/.ssh/id_ed25519 >> id_windows
chmod 600 id_windows
```

要确保密钥文件的权限为 `rw------- (600)`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d5db1071240c645719eabda6685826ca.png#pic_center)

2. 在后台启动 ssh 代理

```bash
eval "$(ssh-agent -s)"
```

3. 将密钥添加到 ssh-agent

```bash
ssh-add ~/.ssh/id_windows
```

4. 测试 SSH 连接
```bash
ssh -T git@github.com
```

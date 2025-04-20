@[toc](【Ubuntu】配置 Jetson Nano 基础环境（二）)
#  1 初始化 root 密码
>如果想要通过 `su` 命令切换 `root` 用户，要先对 `root` 用户的密码进行设置

- 设置 root 密码

如果之前没有设置过，它会让你直接填入密码
```bash
sudo passwd
```
- 输入 `su` 命令测试一下

# 2 修改用户密码（非必要）
>个人建议用户密码不要太冗长

当你想修改密码时，最好要使用 `root` 权限:
- 普通用户修改自己的密码需要先输入自己的旧密码，只有旧密码输入正确才能输入新密码。不仅如此，此种修改方式对密码的复杂度有严格的要求，新密码太短、太简单，都会被系统检测出来并禁止用户使用

- 使用 `root` 用户，无论是修改普通用户的密码，还是修改自己的密码，都可以不遵守 `PAM` 模块设定的规则

```bash
# 若不加用户名，则是修改 root 用户密码
sudo passwd 用户名 
```

# 3 关闭、开启图形化
>为了节省内存、显存，可以选择关闭 `GUI`，来进一步提高性能

将相关命令写入 `.sh` 文件中，执行的时候会方便很多。
- 关闭图形化
```bash
vim ~/close_gui.sh
# 写入以下内容
---------------------------------------------------
sudo systemctl set-default multi-user.target
sleep 1
sudo reboot
---------------------------------------------------
bash close_gui.sh
```
- 开启图形化

```bash
vim ~/open_gui.sh
# 写入以下内容
---------------------------------------------------
sudo systemctl set-default graphical.target
sleep 1
sudo reboot
---------------------------------------------------
bash open_gui.sh
```

# 4 自动登录
## 4.1 当开启图形化时
1. 执行 `sudo vim /etc/gdm3/custom.conf`
 - `AutomaticLoginEnable = true`
- `AutomaticLogin = 自己的用户名`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/87f5daf0e0d3dfac7da9597fcf8ad0f2.png#pic_center =700x)

2. 重启 `Nano` 验证

## 4.2 当关闭图形化时
参考文章：
1. [Enable auto-logon with systemd (non-GUI) (Ubuntu 16.04+ and Debian 8+)](https://blackmanticore.com/8683a884ca8aad72b46442e0b7196cef#:~:text=First%2C%20determine%20which%20tty%20you%20wish%20to%20have,where%20you%20can%20adjust%20the%20parameters%20like%20so%3A)
2. [How do I override or configure systemd services?](https://askubuntu.com/questions/659267/how-do-i-override-or-configure-systemd-services/659268#659268)

- 由于关闭 GUI 的时候，默认是 `tty1`，所以要对 `tty1` 配置自动登录
- 执行下面的命令
```bash
sudo systemctl edit getty@tty1
```
- 填入下面的内容
>用自己的用户名代替下面的 `myusername`
```txt
[Service]
ExecStart=
ExecStart=-/sbin/agetty --noissue --autologin myusername %I $TERM
Type=idle
```
- 保存修改后，关闭图形化测试一下

由于只配置了 `tty1`，所以当用 `Alt + F2\F3\F4` 切换成 `tty2\tty3\tty4` 时，还是要输入用户名和密码才能登录。

# 5 开启最高频率（非必要）
参考文章：[关于 jetson clocks 的命令](https://www.cnblogs.com/mrlonely2018/p/14790168.html)
>Jetson 开发板使用一种叫做 `DVFS` （`Dynamic Voltage and Frequency Scaling `）的技术，根据需要调整各个处理器的电压、功率，并将他们的运行功率、频率限制在当前性能模式（`MAXN` 或 `5W`）设定的最大值之下。
>而使用 `jetson_clocks` 可以取消 `DVFS` 的动态调整，并将各处理器的频率强行设定为当前性能模式下的最大值。

- 查询 `jetson_clocks` 命令相关参数
> - `--show`: 显示当前频率信息
> - `--store`: 将当前设置存储下来（可以指定文件名和位置）
> - `--restore`: 还原文件中的设置
> - 当不使用任何参数时，即可将 `CPU、GPU、EMC` 频率设置为最大

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4631e1ae7bdbe3d005e57bdeeae1bd08.png#pic_center =800x)

- 运行 `jtop`

可以看到当前 `Jetson Clocks` 的状态是 `inactive`。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ee7670d0a6746e959242ef9aa59514b5.png#pic_center =900x)

- 先对当前设置进行存储

> 加上 `sudo` 相当于 `root` 用户，执行完后默认将配置文件保存在 `/root/l4t_dfs.conf`
```bash
sudo jetson_clocks --store
```
- 激活 `jetson clocks`

```bash
sudo jetson_clocks
```
- 查看 `jtop`

可以看到当前 `Jetson Clocks` 的状态是 `running`，说明已经开启。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4965695d153d4a54e1488069f2bdbb3b.png#pic_center =900x)

- 查看频率信息

```bash
sudo jetson_clocks --show
```
可以看到均已达到最大频率
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5365504b9ca2e04e4e3ecc4912d2cbf0.png#pic_center =800x)

- 退出 `jetson clocks`

```bash
sudo jetson_clocks --restore
```
- 查看 `jtop` 中 `Jetson Clocks` 的状态是否为 `inactive`




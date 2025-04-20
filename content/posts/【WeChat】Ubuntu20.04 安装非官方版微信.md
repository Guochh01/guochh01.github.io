@[TOC](【WeChat】Ubuntu20.04 安装非官方版微信)

# 1 安装 Flatpak
> 参考文档: [Link](https://www.flatpak.org/setup/Ubuntu)

1. 安装
```bash
sudo apt install flatpak
```

2. 添加 Flathub 仓库
```bash
 flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

3. 重启电脑

# 2 安装 WeChat

> GitHub: [Link](https://github.com/web1n/wechat-universal-flatpak)

1. 根据架构下载安装包

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/abb723d1368f4813b81098dd813b0f84.png#pic_center)

2. 安装

```bash
flatpak update
flatpak install com.tencent.WeChat-x86_64.flatpak
```

3. 查看安装列表

```bash
flatpak list --app
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d674433eb0c74949848c333aa8a2d68e.png#pic_center)

# 3 遇到的问题

## 3.1 聊天窗口无法输入中文

> 参考 issue: [Link](https://github.com/web1n/wechat-universal-flatpak/issues/33#issuecomment-2202243919)

我的输入法框架是 `ibus`，对 issue 中的解决方法进行了优化，直接修改 `.desktop` 文件中的执行命令。（如果输入法框架是 `fcitx` 自行看 issue 中的解决方法）

1. 找到 Flatpak 安装的 WeChat 路径: `/var/lib/flatpak/app`

2. 找到 WeChat 的 `.desktop` 文件: `/var/lib/flatpak/app/com.tencent.WeChat/current/active/export/share/applications`

3. 修改文件中的 `Exec` 参数，在前面添加 `env IBUS_USE_PORTAL=1`

```bash
sudo vim com.tencent.WeChat.desktop
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dd8d78326f5a47e4ae77ce5ad3346a0b.png#pic_center)

4. 重启电脑

# 4 卸载 WeChat

```bash
flatpak uninstall com.tencent.WeChat
```

@[TOC](【PyCharm】Ubuntu20.04 卸载 PyCharm 并安装激活 Professional)
# 1 卸载
> 参考文档: [Link](https://www.jetbrains.com/help/pycharm/uninstall.html#standalone)

1. 删除安装目录

删掉之前压缩包解压出来的目录，例如：我之前是放在家目录下
```bash
rm -rf ~/pycharm-community-2023.2.1
```

2. 删除配置文件

```bash
rm -rf ~/.config/JetBrains/PyCharmCE2023.2/
rm -rf ~/.cache/JetBrains/PyCharmCE2023.2/
rm -rf ~/.local/share/JetBrains/
```

3. 删除快捷方式
```bash
sudo rm -f /usr/share/applications/jetbrains-pycharm-ce.desktop
```


# 2 安装激活

> 参考文档: [Link](https://www.jetbrains.com/help/pycharm/installation-guide.html#standalone)

1. 下载压缩包: [Link](https://www.jetbrains.com/pycharm/download/?section=linux)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e3b946b8d1294d7c89131f516a7ef760.png#pic_center)

2. 解压，将 PyCharm 安装到指定目录（按个人喜好选择即可）

```bash
sudo tar xzf pycharm-*.tar.gz -C /opt/
```

3. 运行

```bash
cd /opt/pycharm-2024.2.3/bin/
sh pycharm.sh
```

4. 获取免费教育许可证: [Link](https://www.jetbrains.com/zh-cn/community/education/#students)

- 申请时使用学校的邮箱
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0529eb142185491d95778d0adf7398c1.png#pic_center)

- 点击邮件中的链接，创建帐号进行绑定

- 运行 PyCharm 时登录帐号即可激活

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4b29414bf17f486baaf4d7afb40df781.png#pic_center)


5. 创建快捷方式

点击 `Tools` -- `Create Desktop Entry`

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/17d3649846454c4db988e2dc62ae16f6.png#pic_center)


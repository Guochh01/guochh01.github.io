

# 1 mihomo

> GitHub 地址: [Link](https://github.com/MetaCubeX/mihomo)

## 1.1 介绍

> 参考文档: [Link](https://findladders.com/clash-meta/)

Clash Meta（现更名为 mihomo）是一个基于广受欢迎的开源项目[Clash](https://findladders.com/clash/)的高级版本。它继承了 Clash 的核心功能，保留了原始 Clash 的灵活性和高效性，并增加了一些独特的特性，并包括部分 Clash Premium 核心功能，是目前网络代理和数据流管理最强大的软件。

Clash Meta（mihomo）主要是一个内核，其配置可能稍微复杂，但与[Clash Verge](https://clashverge.net/clash-verge-rev/)等GUI客户端一起使用，可以在获得强大功能的同时也拥有友好的操作体验。

## 1.2 配置

> 参考文档: [Link](https://wiki.metacubex.one/startup/service/)

1. 下载二进制可执行文件: [Link](https://github.com/MetaCubeX/mihomo/releases)

![image-20250410141024681](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250410141024681.png)

2. 解压

```bash
# 使用 `-k` 选项表示保留原文件（Keep）
gzip -d mihomo-linux-amd64-v1.19.4.gz
mv mihomo-linux-amd64-v1.19.4 ~/proxy/mihomo
chmod +x ~/proxy/mihomo
```

3. 下载配置文件

购买机场订阅后，复制 Clash 订阅链接。

- 命令行下载

```bash
wget -O xxx.yaml "订阅链接"
```

- 浏览器下载：复制到浏览器地址栏，即可下载

配置文件中部分内容如下：

```yaml
#---------------------------------------------------#
## 更新：2025-03-25 12:01:02
#---------------------------------------------------#

# HTTP 代理端口
port: 3801 

# SOCKS5 代理端口
socks-port: 3802 

# https://wiki.metacubex.one/config/inbound/port/#_1
mixed-port: 3803

# Linux 和 macOS 的 redir 代理端口
redir-port: 3804 

# 允许局域网的连接
allow-lan: true

# 规则模式：Rule（规则） / Global（全局代理）/ Direct（全局直连）
mode: Rule

# 设置日志输出级别 (默认级别：silent，即不输出任何内容，以避免因日志内容过大而导致程序内存溢出）。
# 5 个级别：silent / info / warning / error / debug。级别越高日志输出量越大，越倾向于调试，若需要请自行开启。
log-level: info
# Clash 的 RESTful API
external-controller: '127.0.0.1:3890'
experimental:
  ignore-resolve-fail: true
# RESTful API 的口令
secret: '' 

# 您可以将静态网页资源（如 clash-dashboard）放置在一个目录中，clash 将会服务于 `RESTful API/ui`
# 参数应填写配置目录的相对路径或绝对路径。
# external-ui: folder
#  # Clash DNS 请求逻辑：
#  # (1) 当访问一个域名时， nameserver 与 fallback 列表内的所有服务器并发请求，得到域名对应的 IP 地址。
#  # (2) clash 将选取 nameserver 列表内，解析最快的结果。
#  # (3) 若解析结果中，IP 地址属于 国外，那么 Clash 将选择 fallback 列表内，解析最快的结果。

#  # 注意：
#  # (1) 如果您为了确保 DNS 解析结果无污染，请仅保留列表内以 tls:// 开头的 DNS 服务器，但是通常对于国内没有太大必要。
#  # (2) 如果您不在乎可能解析到污染的结果，更加追求速度。请将 nameserver 列表的服务器插入至 fallback 列表内，并移除重复项。

geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

4. 运行

```bash
mv config.yaml  ~/.config/mihomo/config.yaml
~/proxy/mihomo -f ~/.config/mihomo/config.yaml
```

![image-20250410155704529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250410155704529.png)

## 1.3 配置过程中的 Bug

> 参考 issue: [Link](https://github.com/clash-verge-rev/clash-verge-rev/issues/1377#issuecomment-2222746616)

`~/proxy/mihomo -f ~/.config/mihomo/config.yaml` 输出报错如下：

```txt
INFO[2025-03-25T00:10:49.712288423+08:00] Start initial configuration in progress
INFO[2025-03-25T00:10:49.722151449+08:00] Geodata Loader mode: memconservative
INFO[2025-03-25T00:10:49.722166989+08:00] Geosite Matcher implementation: succinct
INFO[2025-03-25T00:10:49.768352092+08:00] Can't find MMDB, start download
ERRO[2025-03-25T00:12:19.769700806+08:00] can't initial GeoIP: can't download MMDB: Get "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.metadb": context deadline exceeded
FATA[2025-03-25T00:12:19.769781893+08:00] Parse config error: rules[13733] [GEOIP,CN,直接连接] error: can't download MMDB: Get "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.metadb": context deadline exceeded
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fc499a2072f24696948dffa67ce6d7c3.png)

在 `config.yaml` 文件中加入 GEO 下载地址，来自: [Link](https://wiki.metacubex.one/config/general/)

```yaml
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

再次执行 `~/proxy/mihomo -f ~/.config/mihomo/config.yaml` 后，在 ~/.config/mihomo/ 下出现 `geoip.metadb` 文件。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b066e95ecf114d84b92702b1222238a5.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8c902f62829543929ed5830def8f3f28.png)

# 2 配置 metacubexd Web UI

> 参考文章: [Link1](https://12520.net/archives/debian-mihomo-clash.mate-webui)、[Link2](https://linux.do/t/topic/241319#p-2197991-external-control-5)

> GitHub Usage: [Link](https://github.com/MetaCubeX/metacubexd?tab=readme-ov-file#usage)

1. 创建 ui 文件夹并 clone

```bash
mkdir -p ~/proxy/ui
git clone https://github.com/metacubex/metacubexd.git -b gh-pages ~/proxy/ui
# 定期更新本地仓库
git -C ~/proxy/ui pull -r
```

2. 修改 config.yaml 文件

```bash
#---------------------------------------------------#
## 更新：2025-03-25 12:01:02
#---------------------------------------------------#

# HTTP 代理端口
port: 3801 

# SOCKS5 代理端口
socks-port: 3802 

# https://wiki.metacubex.one/config/inbound/port/#_1
mixed-port: 3803

# Linux 和 macOS 的 redir 代理端口
redir-port: 3804 

# 允许局域网的连接
allow-lan: true

# 规则模式：Rule（规则） / Global（全局代理）/ Direct（全局直连）
mode: Rule

# 设置日志输出级别 (默认级别：silent，即不输出任何内容，以避免因日志内容过大而导致程序内存溢出）。
# 5 个级别：silent / info / warning / error / debug。级别越高日志输出量越大，越倾向于调试，若需要请自行开启。
log-level: info
# Clash 的 RESTful API
external-controller: 0.0.0.0:3890
external-ui: /home/guochenhui/proxy/ui
# external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip" #从 GitHub Pages 分支获取

experimental:
  ignore-resolve-fail: true
# RESTful API 的口令
secret: '' 

# 您可以将静态网页资源（如 clash-dashboard）放置在一个目录中，clash 将会服务于 `RESTful API/ui`
# 参数应填写配置目录的相对路径或绝对路径。
# external-ui: folder
#  # Clash DNS 请求逻辑：
#  # (1) 当访问一个域名时， nameserver 与 fallback 列表内的所有服务器并发请求，得到域名对应的 IP 地址。
#  # (2) clash 将选取 nameserver 列表内，解析最快的结果。
#  # (3) 若解析结果中，IP 地址属于 国外，那么 Clash 将选择 fallback 列表内，解析最快的结果。

#  # 注意：
#  # (1) 如果您为了确保 DNS 解析结果无污染，请仅保留列表内以 tls:// 开头的 DNS 服务器，但是通常对于国内没有太大必要。
#  # (2) 如果您不在乎可能解析到污染的结果，更加追求速度。请将 nameserver 列表的服务器插入至 fallback 列表内，并移除重复项。

geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

3. 运行 `~/proxy/mihomo -f ~/.config/mihomo/config.yaml`
4. 在浏览器输入 `http://[服务器 ip 地址]:3890/ui` 即可打开WebUI控制面板

![image-20250411215724072](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250411215724072.png)

5. 填写后端地址和密钥，点击添加即可进入

- 后端地址填写 `http://[服务器 ip 地址]:3890`
- 如果在 config.yaml 中设置了密钥 secret，则还需要填写密钥

![image-20250411220554474](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250411220554474.png)

# 3 命令行代理

- 打开

```bash
export http_proxy=http://127.0.0.1:3801
export https_proxy=http://127.0.0.1:3801
```

- 关闭

```bahs
unset http_proxy https_proxy
```

- 测试

```bash
curl -i google.com
```

![image-20250410160734112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250410160734112.png)













3. 创建用户服务文件

```bash
mkdir -p ~/.config/systemd/user
vim ~/.config/systemd/user/mihomo.service
```

```txt
[Unit]
Description=mihomo Daemon, Another Clash Kernel.
After=network.target NetworkManager.service systemd-networkd.service iwd.service

[Service]
Type=simple
LimitNPROC=500
LimitNOFILE=1000000
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
Restart=always
ExecStartPre=/usr/bin/sleep 1s
ExecStart=/home/guochenhui/mihomo -d /home/guochenhui/.config/mihomo
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

4. 启动 mihomo

```bash
systemctl --user daemon-reload
systemctl --user enable mihomo
systemctl --user start mihomo
```



5. 查看 mihomo 运行状态

```bash
systemctl --user status mihomo
```


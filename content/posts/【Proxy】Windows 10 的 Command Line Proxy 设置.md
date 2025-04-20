# 1 本机环境

- Windows 10
- v2rayN

在 v2rayN 界面下方可以看到 `socks` 和 `http` 的端口号，分别为 `10808` 和 `10809`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/32cf23078c5b57e42cc4790c3380f992.png)

# 2 PowerShell
每次打开新窗口，执行下面的命令
```bash
$env:HTTP_PROXY="http://127.0.0.1:10809"
$env:HTTPS_PROXY="http://127.0.0.1:10809"
```
# 3 CMD
每次打开新窗口，执行下面的命令
```bash
set http_proxy=http://127.0.0.1:10809
set https_proxy=http://127.0.0.1:10809
```






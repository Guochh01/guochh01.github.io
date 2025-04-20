@[TOC](【软链接】在 Windows 和 Linux 创建文件或文件夹软链接)

# 1 Windows 创建软链接
> 官方文档: [Link](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/mklink)


**Note:** 必须在 CMD 中执行

- 文件链接
```bash
mklink [生成的链接文件] [源文件]
```

- 文件夹链接
```bash
mklink /d [生成的链接文件夹] [源文件夹]
```
# 2 Linux 创建软链接

**Note:** 路径必须为绝对路径

```bash
 ln -s [源文件或目录] [生成的链接文件或目录]
```

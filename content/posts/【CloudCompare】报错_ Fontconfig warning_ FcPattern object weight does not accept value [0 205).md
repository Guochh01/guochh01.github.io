@[TOC](【CloudCompare】报错: Fontconfig warning: FcPattern object weight does not accept value [0 205])

# 1 问题描述
在 Ubuntu 的应用商店或者利用 flathub 安装后，无法打开 CloudCompare。
# 2 解决方法
>参考 [Link](https://github.com/keshavbhatt/olivia/issues/95#issuecomment-774747492)

1. 利用命令行打开，查看报错信息
```bash
cloudcompare.CloudCompare
```
报错如下：
>Fontconfig warning: FcPattern object weight does not accept value [50 200)
>Segmentation fault (core dumped)

2. 

```bash
sudo rm /var/cache/fontconfig/*
rm ~/.cache/fontconfig/*
fc-cache -r
```




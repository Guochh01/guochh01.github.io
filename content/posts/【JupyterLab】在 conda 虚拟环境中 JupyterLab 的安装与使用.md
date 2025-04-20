@[TOC](【JupyterLab】在 conda 虚拟环境中 JupyterLab 的安装与使用)
# 1 JupyterLab 介绍

> 官方文档: [Link](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html#get-started)

JupyterLab 是 Project Jupyter 旗下其他笔记本编写应用程序（如 Jupyter Notebook 和 Jupyter Desktop）的同胞兄弟。与 Jupyter Notebook 相比，JupyterLab 提供了更先进、功能更丰富、可定制的体验。
# 2 安装
> 官方文档: [Link](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#installation)

```bash
pip install jupyterlab
```
## 2.1 Jupyter Kernel 与 conda 虚拟环境

本人目前的建议是：在每个虚拟环境中都完整地安装 `jupyterlab`（**在运行前一定要激活所需的虚拟环境**）。

在使用时，用这个默认的 Kernel 即可，它调用的就是所在虚拟环境的 Python Interpreter。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7c23913b22f227ef16e685bd40a98e3e.png#pic_center)
可以用下面的代码运行验证一下：
```python
import os
import sys
print(f"Python 版本信息: {sys.version}")
print(f"\n解释器路径: {sys.executable}")
print(f"\n当前工作目录: {os.getcwd()}")
```

此外，这篇问答 [How to use Jupyter notebooks in a conda environment?](https://stackoverflow.com/questions/58068818/how-to-use-jupyter-notebooks-in-a-conda-environment) 给出了 3 种不同的使用方式，想要尝试的话可以参考。
# 3 使用

## 3.1 安装中文语言包(Optional)
> 官方文档: [Link](https://jupyterlab.readthedocs.io/en/stable/user/language.html)

```bash
pip install jupyterlab-language-pack-zh-CN
```
将页面切换为中文

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2224d50d624a4292a0a4b571a2db6711.png#pic_center)

## 3.2 启动

- `--no-browser`: 禁止启动时自动打开浏览器
- `--ip=<Unicode>`: Jupyter 服务器监听的 IP 地址，默认为 `localhost`
- `--port=<Int>`: Jupyter 服务器监听的端口
- `--notebook-dir=<Unicode>`: 工作目录（顶层）
- `--app-dir=<Unicode>`: 启动时所在的目录（包含于 notebook-dir）
- `--pylab=<Unicode>`: 默认为 `disabled`，需要在 notebook 中使用 `%pylab` 或 `%matplotlib` 来启用 matplotlib

```bash
jupyter lab --notebook-dir=E:/ --preferred-dir E:/Documents/Somewhere/Else
```
## 3.3 常用快捷键
- `ESC`: 切换到命令模式
- `ENTER`: 切换到编辑模式
- `Ctrl` + `Enter`: 运行 Cell
- `Shift` + `Enter`: 运行 Cell，并切换至下一个 Cell

### 3.3.1 命令模式下
- `a`: 上方插入新 Cell
- `b`: 下方插入新 Cell
- `y`: 将 Cell 转为 Code
- `m`: 将 Cell 转为 Markdown
- `d` + `d`: Restart Kernel

## 3.4 远程访问局域网内计算机
若要访问局域网外设备，需要公网 IP 或内网穿透工具，本文不会涉及。

默认情况下，Jupyter 服务器在本地运行，且只能从 localhost 访问。

### 3.4.1 利用 IP 地址访问

### 3.4.2 利用 SSH 端口转发

>SSH 端口转发讲解视频: [Link](https://www.bilibili.com/video/BV1C7411P7Er/?share_source=copy_web&vd_source=8a35b1598be5e3074a8113c792f1a194)

```bash
jupyter lab --notebook-dir=E:/ --preferred-dir E:/Documents/Somewhere/Else --ip="192.168.31.177" --port=12345 --no-browser
```

待更ing

@[toc](【C++】解决在 Windows 上工程发布为 exe 时，dll 动态库相关问题)

# 1 查询 exe 需要的所有 dll 文件
> 使用 `dependencies` 工具
 
其概述为：
>`Dependencies is a rewrite of the legacy software Dependency Walker which was shipped along Windows SDKs, but whose development stopped around 2006. Dependencies can help Windows developers troubleshooting their dll load dependencies issues.`
>
>---
>即 `Dependency Walker` 在 2006 年已经停止维护，而 `dependencies` 是由前者发展而来的，帮助 `Windows` 开发人员排除 `dll` 依赖相关的问题。

## 1.1 下载
在 [GitHub](https://github.com/lucasg/Dependencies) 中下载。
## 1.1 使用
- 解压后，运行 `DependenciesGui.exe`
- 选择 `File --> Open`，选择需要查询的 exe 文件
>效果如下图

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f1a79241aaaf461b108d66cd4f83c975.png#pic_center =1000x)
# 2 寻找本地中相应的 dll 文件
>使用 `Everything` 软件（可以快速查找本地文件）

## 2.1 安装
在 [官网](https://www.voidtools.com/zh-cn/downloads/) 中选择相应版本安装。

## 2.2 使用
- 双击打开后，直接在输入框中输入要找的 `dll` 文件名即可

>效果如下图

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2377fe772fbba9334cc61938865249c6.png#pic_center =800x)
# 3 下载本地中缺失的 dll 文件
一般系统的 `dll` 文件都可以在本地找到，如果实在找不到，可以在一些 `dll` 文件下载网站上搜索一下。
>例如：[DLL‑FILES.COM](https://cn.dll-files.com/)

# 4 打包将要发布的程序
- 将 `exe` 文件与 `dll` 文件放在同一目录下
- 压缩文件夹，发给其他电脑进行运行测试
- 在其他电脑中双击 `exe` 后如果仍有 `dll` 缺失相关的错误
>缺什么就往文件夹里补什么（按 `2` 和 `3` 中说的找一下）
- 当所有需要的 `dll` 文件都放在目录里时，程序自然就可以在其他电脑上运行啦

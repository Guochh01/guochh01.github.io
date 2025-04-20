@[toc](【C++】Visual Studio 2019 配置 GSL 库)
# 1 GSL简介

>官网链接: [Link](https://www.gnu.org/software/gsl/)

GNU Scientific Library (GSL) 是一个供 C 和 C++ 程序员使用的数字库。它是 GNU 通用公共许可证下的免费软件。

该库提供了广泛的数学例程，例如随机数生成器、特殊函数和最小二乘拟合。总共有超过 1000 个函数和一个广泛的测试套件，Matlab 的大部分函数几乎都能借助它实现。

# 2 本机环境

- Windows10
- Visual Studio 2019

# 3 实现方法

有两种方法:

- 在 Visual Studio 2019 中下载 NuGet 程序包
- 编译源码

## 3.1 下载 NuGet 程序包
打开 VS2019 $\Rightarrow$ 项目 $\Rightarrow$ 管理 NuGet 程序包 $\Rightarrow$ 搜索 gsl

![NuGet](https://i-blog.csdnimg.cn/blog_migrate/26f47208ca335b6277ed586de0d80bd3.png#pic_center)
选择你自己相应的 x86 或者 x64，安装完成后，这个包会出现你的项目目录的 packages 中，include 头文件即可使用。

## 3.2 源码编译


1. clone 源码

```bash
git clone https://github.com/BrianGladman/gsl.git
```

2. 运行 `build.vc \ gsl.lib.sln` 即可实现静态库的编译 --- 在 lib 目录中生成 `.lib` 文件
3. 运行 `build.vc \ gsl.dll.sln` 即可实现动态库的编译 --- 在 dll 目录中生成 `.dll` 文件


> 以上两个直接运行会出现一些报错，可以参考这篇文章的前半部分来完成配置：[VS2019配置GSL库](https://blog.csdn.net/weixin_42967497/article/details/117899053)

4. 为自己的项目添加相应的头文件和 lib 库文件
- 添加头文件：项目 $\Rightarrow$ 属性 $\Rightarrow$ C/C++ $\Rightarrow$ 常规 $\Rightarrow$ 附加包含目录
 `E:\GSL\gsl\include`
- 添加 lib 静态库路径：项目 $\Rightarrow$ 属性 $\Rightarrow$ 链接器 $\Rightarrow$ 常规 $\Rightarrow$ 附加库目录
 `E:\GSL\gsl\lib\x64\Debug`
- 添加引用的 lib 文件名：项目 $\Rightarrow$ 属性 $\Rightarrow$ 链接器 $\Rightarrow$ 输入 $\Rightarrow$ 附加依赖项
 `cblas.lib;gsl.lib`

**Note:** 配置时要注意自己工程的版本，debug 版本和 release 版本都有相应的 lib 库。


完成以上步骤即可直接 include 头文件开始开心调库了！！在调库的时候可以参考下面给出的官方 API 文档。

# 4 参考文章
1. [GSL官网API文档](https://www.gnu.org/software/gsl/doc/html/index.html)
2. [C++ 数学计算库汇总](https://www.cnblogs.com/bokeyuan-dlam/articles/8926548.html)
3. [在VS中添加lib库的三种方法](https://www.cnblogs.com/dongsheng/p/4011145.html)

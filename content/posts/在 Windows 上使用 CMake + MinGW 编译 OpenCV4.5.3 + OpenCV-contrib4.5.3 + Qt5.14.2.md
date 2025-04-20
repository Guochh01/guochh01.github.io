@[TOC](CMake + MinGW 编译 OpenCV4.5.3 + OpenCV-contrib4.5.3 + Qt5.14.2)
# 一、版本声明
- CMake: 3.21.1
- g++: 8.1.0
- gcc: 8.1.0
- OpenCV: 4.5.3
- OpenCV-contrib: 4.5.3
- Qt: 5.14.2
# 二、环境变量配置
![环境变量配置](https://i-blog.csdnimg.cn/blog_migrate/5bff2086b33296ec0fec8d0677ced7e4.png#pic_center =500x)
# 三、CMake GUI 配置
1. **选择 `Source、Build` 路径**
-  `Source`: 下载的 opencv 源码的路径
-  `Build`: 编译时生成文件的路径（主要是 makefile 文件）

> 推荐按下图路径进行配置~

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f23a5efa1406013478b9a4f2bb31b982.png#pic_center =700x)

2. **`Configure`**
- Specify the generator for this project: `MinGW Makefiles`
- `Specify native compilers`
- Next
- Compilers C: `gcc.exe` 的路径
- Compilers C++: `g++.exe` 的路径
- Finish

3. **配置选项**
- 勾选 `WITH_OPENGL`
- 勾选 `BUILD_opencv_world`（最后会集成在一个 `libopencv_world453.dll` 的动态库中）
- 勾选 `ENABLE_CXX11`（如果没有，则创建一个）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4abc0aef7e3dd45386e490e71d0a241d.png#pic_center =700x)

- 不勾选 `WITH_IPP`
- 添加 `opencv_contrib` 路径
>OPENCV_EXTRA_MODULES_PATH: E:/Opencv4.5.3/opencv_contrib-4.5.3/modules
- 勾选 `WITH_QT`
- 不勾选所有 `python` 相关选项（因为不需要 `build for python`）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/87c89439566c13f98662a90790dd319b.png#pic_center =500x)

- 不勾选所有 `TEST` 选项
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6682c844454fb286433a7243ec2270b5.png#pic_center =500x)

4. **`Configure`**

5. **配置选项**
- 添加 `Qt` 路径
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8bffccb7a9027dbc0cae99e098914798.png#pic_center =800x)
- 不勾选 `WITH_OPENCL_D3D11_NV`
- 不勾选 `WITH_MSMF`

- 不勾选 `BUILD_opencv_hdf`
>报错，用不到这个包就不编译了
- 不勾选 `BUILD_opencv_cvv`
>勾选了 `BUILD_opencv_world`，就不能勾选这个，二者选其一，都选就会报 `cvv` 有关的错误。
>用不到这个包就不编译了
- 不勾选 `BUILD_opencv_rgbd`
- 添加配置 `OPENCV_VS_VERSIONINFO_SKIP` 并勾选

6. **`Configure`**
7. **若 `GUI` 中无错误，点击 `Generate`**

# 四、make 编译
```shell
E: # windows 要先切换盘符
cd E:\Opencv4.5.3\opencv\build\x64\mingw
mingw32-make -j6 # 担心电脑卡，就不拉满了
```
若无报错，达到 100% 即可下一步。

# 五、make install 安装
```shell
mingw32-make install
```



@[TOC](【ORB-SLAM3】使用 RealSense D435i 跑 ORB-SLAM3 时遇到的一些 Bug)

# 1 hwmon command 0x80( 5 0 0 0 ) failed (response -7= HW not ready)

**问题描述:**

运行 `./Examples/Stereo-Inertial/stereo_inertial_realsense_D435i Vocabulary/ORBvoc.txt ./Examples/Stereo-Inertial/RealSense_D435i.yaml` 时，报错如下:

```txt
terminate called after throwing an instance of 'rs2::invalid_value_error'
 what(): hwmon command 0x80( 5 0 0 0 ) failed (response -7= HW not ready)
```

**解决方法:**

受到这个 issue: [Link](https://github.com/IntelRealSense/realsense-ros/issues/3026#issuecomment-1972404743) 的启发，想到自己为了省事，而选择了直接通过 apt 来安装 RealSense ROS，可能导致了相机固件、RealSense SDK 和 RealSense ROS 三者之间的版本不匹配。

1. 卸载 apt 安装的 RealSense ROS

```bash
sudo apt remove ros-noetic-realsense2-camera
sudo apt install ros-noetic-realsense2-description
sudo apt autoremove
```

2. 通过源码编译安装，具体步骤详见我的这篇文章: [Link](https://blog.csdn.net/G_C_H/article/details/137375276)


# 2 No rule to make target '/opt/ros/noetic/lib/x86_64-linux-gnu/librealsense2.so', needed by '../lib/libORB_SLAM3.so'

**问题描述:**
我通过源码编译安装 RealSense ROS 后，重新对 ORB-SLAM3 进行了编译，执行 `./build.sh` 时，报错如下:

```txt
make[2]: *** No rule to make target '/opt/ros/noetic/lib/x86_64-linux-gnu/librealsense2.so', needed by '../lib/libORB_SLAM3.so'.  Stop.
make[1]: *** [CMakeFiles/Makefile2:971: CMakeFiles/ORB_SLAM3.dir/all] Error 2
make: *** [Makefile:84: all] Error 2
```

**解决方法:**

我首先查看了 ORB-SLAM3 的 CMakeLists.txt 文件，发现不存在 `target_link_libraries()` 语句中的第三方库指向 `/opt/ros/noetic/lib/x86_64-linux-gnu/librealsense2.so` 这个路径，而这个路径正是我之前使用 apt 安装 RealSense ROS 创建的，卸载之后就不存在了，这个问题让我疑惑了很久。

当我看到 `build/CMakeFiles/mono_euroc.dir/build.make` 文件中存在这个路径（图 1），并配合 CMakeLists.txt 中的 `target_link_libraries()` 中第三方库的顺序（图 2），我觉得是 `Pangolin` 的原因。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1a5c8b6d68caedb22f77d689d580dc2f.png#pic_center)


![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c47d1b6087a9710211a439ef8558ecb2.png#pic_center)


当我看到 Pangolin 文件夹里存在 `CMakeModules/FindLibRealSense2.cmake` 时，更加印证了我的想法，真的没想到 Pangolin 也会用到 Realsense SDK 的库文件。

出现这个问题相当于 ORB-SLAM3 依赖 Pangolin，Pangolin 依赖 Realsense SDK，层层嵌套，最内层出现了问题，所以我重新编译安装了 Pangolin。


1. 重新编译安装 Pangolin
```bash
cd ~/3rdparty/Pangolin-0.6

rm -rf build
cmake -B build
cmake --build build			
sudo cmake --install build 	
```
2. 编译 ORB-SLAM3

```bash
rm -rf build
./build.sh
```


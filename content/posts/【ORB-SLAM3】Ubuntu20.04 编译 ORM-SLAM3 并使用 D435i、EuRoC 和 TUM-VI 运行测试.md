@[TOC](【ORB-SLAM3】Ubuntu20.04 编译 ORM-SLAM3 并使用 D435i、EuRoC 和 TUM-VI 运行测试)

ORB-SLAM3 GitHub 链接: [Link](https://github.com/UZ-SLAMLab/ORB_SLAM3?tab=readme-ov-file)
# 1 Prerequisites

如果要重新安装以下第三方库，可以在之前运行 sudo make install 的目录（一般是 build 文件夹）下运行 `sudo make uninstall` 对其进行卸载。

## 1.1 C++11 or C++0x Compiler

在 ORB-SLAM3 的 CMakeLists.txt 中会有检查，如果不满足会有报错。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fb1da7216ea5a1999bc993fa13d6c652.png#pic_center)

## 1.2 Pangolin

>参考文档: [Link](https://github.com/stevenlovegrove/Pangolin?tab=readme-ov-file#building)

这里我选择安装的是 Pangolin v0.6，下载链接: [Link](https://github.com/stevenlovegrove/Pangolin/releases)

1. 准备工作（如果是安装 v0.6 可以直接进行第 2 步）
```bash
git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
./scripts/install_prerequisites.sh recommended
```
如果有报错：`E: Unable to locate package catch2`，则

- 从 install_prerequisites.sh 中移除安装 catch2

>参考 issue: [Link](https://github.com/stevenlovegrove/Pangolin/issues/913)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6d45d8c78ccadf208adce80b40ad1154.png#pic_center)

- 自行安装 catch2

>参考文档: [Link](https://github.com/catchorg/Catch2/blob/devel/docs/cmake-integration.md#installing-catch2-from-git-repository)

```bash
git clone https://github.com/catchorg/Catch2.git
cd Catch2
cmake -B build -S . -DBUILD_TESTING=OFF
sudo cmake --build build/ --target install
```

2. 安装 Pangolin v0.6

```bash
unzip Pangolin-0.6.zip -d ~/3rdparty
cd ~/3rdparty/Pangolin-0.6

cmake -B build
cmake --build build			# instead of ”make“
sudo cmake --install build 	# instead of ”sudo make install“
```

## 1.3 OpenCV

>参考文档: [Link](https://docs.opencv.org/4.x/d7/d9f/tutorial_linux_install.html)

这里我选择安装的是 OpenCV 4.4.0 + OpenCV_Contrib 4.4.0

```bash
sudo apt update && sudo apt install -y cmake g++ wget unzip

wget -O opencv.zip https://github.com/opencv/opencv/archive/4.4.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.4.0.zip
unzip opencv.zip -d ~/3rdparty
unzip opencv_contrib.zip -d ~/3rdparty

cd ~/3rdparty/opencv-4.4.0 
mkdir build && cd build
# Configure
cmake -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.4.0/modules ..
# Build
cmake --build .
# Install
sudo cmake --install .
```

## 1.4 Eigen3

>要使用 Eigen，只需下载并提取 Eigen 的源代码，Eigen 子目录中的头文件是使用 Eigen 编译程序所需的唯一文件。

这里我选择安装的是 Eigen 3.3.0，下载链接: [Link](https://gitlab.com/libeigen/eigen/-/releases)

```bash
unzip eigen-3.3.0.zip -d ~/3rdparty
cd ~/3rdparty/eigen-3.3.0
cmake -B build
sudo cmake --install build
```

# 2 安装 Intel® RealSense™ SDK 2.0

> 参考文档: [Link](https://dev.intelrealsense.com/docs/compiling-librealsense-for-linux-ubuntu-guide)

如果不安装 librealsense2，运行 `./build.sh` 后不会生成 `stereo_inertial_realsense_D435i` 等与 RealSense 有关的可执行文件。

## 2.1 测试设备

使用 D435i 相机，设备信息如下:
- firmware version: 5.15.1
- librealsense2 version: v2.54.2

**下面有两种安装方式: 编译源码进行安装和利用预编译包进行安装，二选一即可，我这里用的是编译源码的方式。**

## 2.2 编译源码安装 (Recommend)

1. 安装依赖
```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
sudo apt-get install libssl-dev libusb-1.0-0-dev libudev-dev pkg-config libgtk-3-dev
sudo apt-get install git wget cmake build-essential
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev at
```

2. 下载源码以及准备工作

- 下载 librealsense master 分支: [Link](https://github.com/IntelRealSense/librealsense/archive/master.zip)

```bash
unzip librealsense-master.zip -d ~/3rdparty
```

- 设置 udev 规则

```bash
cd ~/3rdparty/librealsense-master
./scripts/setup_udev_rules.sh
```

- 查询系统内核版本
```bash
uname -r
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/afa3de2ebb8fd5cb4998083102ffdd6b.png#pic_center)


如果是 `Ubuntu 20/22 (focal/jammy) with LTS kernel 5.13, 5.15`，则

```bash
./scripts/patch-realsense-ubuntu-lts-hwe.sh
```

如果是 `Ubuntu 18/20 with LTS kernel (< 5.13)`，则

```bash
./scripts/patch-realsense-ubuntu-lts.sh
```

3. 编译安装

```bash
cd ~/3rdparty/librealsense-master
mkdir build && cd build
cmake ../ -DBUILD_EXAMPLES=true
sudo make uninstall && make clean && make && sudo make install
```

4. 测试

```bash
realsense-viewer
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/22535ddd7aa7f056c3d5e01c8ea00dc0.png#pic_center)


## 2.3 预编译包安装
>参考文档: [Link](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md)

1. 安装

```bash
# Register the server's public key
sudo mkdir -p /etc/apt/keyrings
curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null

# Make sure apt HTTPS support is installed
sudo apt-get install apt-transport-https

# Add the server to the list of repositories
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo tee /etc/apt/sources.list.d/librealsense.list
sudo apt-get update

# Deploy librealsense2 udev rules, build and activate kernel modules, runtime library and executable demos and tools
sudo apt-get install librealsense2-dkms
sudo apt-get install librealsense2-utils

# Install the developer and debug packages
sudo apt-get install librealsense2-dev
sudo apt-get install librealsense2-dbg
```

2. 测试
```bash
realsense-viewer
```

# 3 编译 ORB-SLAM3 (非 ROS)

修改 CMakeLists.txt 文件，将 `realsense_INCLUDE_DIR` 改为 `realsense2_INCLUDE_DIR`，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/89d3190c13581e7cfa9a55e8a648bd15.png#pic_center)


```bash
git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git ORB_SLAM3
cd ORB_SLAM3
chmod +x build.sh
./build.sh
```
完成后，将会在 lib 文件夹中创建 libORB_SLAM3.so，并在 Examples 和 Examples_old 文件夹中创建可执行文件。



# 4 使用 D435i 运行 ORB-SLAM3

## 4.1 运行

先校准相机，并写入校准文件 `your_camera.yaml`，但我这里直接用现成的。

```bash
./Examples/Stereo-Inertial/stereo_inertial_realsense_D435i Vocabulary/ORBvoc.txt ./Examples/Stereo-Inertial/RealSense_D435i.yaml
```
## 4.2 运行时遇到的 Bug

### 4.2.1 terminate called after throwing an instance of 'rs2::invalid_value_error'

**报错如下:**

```txt
terminate called after throwing an instance of 'rs2::invalid_value_error'
  what():  object doesn't support option #85
Aborted (core dumped)
```


**解决方法:**

> 参考 issue: [Link](https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/832)


将 D435i 相关的 `.cc` 文件中这一行内容注释掉: `RS2_OPTION_AUTO_EXPOSURE_LIMIT`，然后重新编译 `./build.sh`。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b81611fe82257b134f6790a3a5ba3501.png#pic_center)




### 4.2.2 Segmentation fault (core dumped)


**报错如下:**

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c9c34289dead82e3bc5904a164f53039.png#pic_center)


**解决方法:**
>参考 issue: [Link](https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/828#issuecomment-1826645958)


将 `System.cc` 文件中这一行内容注释掉: `cout << (*settings_) << endl`，然后重新编译 `./build.sh`。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bb670ce6c070efb423c3b1bd7c1ac859.png#pic_center)
# 5 编译 ORB-SLAM3 (ROS)

## 5.1 编译

1. 将 Examples_old/ROS/ORB_SLAM3 添加到 `ROS_PACKAGE_PATH` 环境变量中

```bash
gedit ~/.bashrc
# Replace PATH by the folder where you cloned ORB_SLAM3
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH/ORB_SLAM3/Examples_old/ROS
```
```bash
source ~/.bashrc
```

2. build_ros.sh 文件中的路径存在问题，修改如下
```bash
echo "Building ROS nodes"

cd Examples_old/ROS/ORB_SLAM3
mkdir build
cd build
cmake .. -DROS_BUILD_TYPE=Release
make -j
```

 3. 运行脚本
```bash
chmod +x build_ros.sh
./build_ros.sh
```

## 5.2 编译时遇到的问题


下面提到的 CMakeLists.txt 文件是 `ORB_SLAM3/Examples_old/ROS/ORB_SLAM3/CMakeLists.txt`。

### 5.2.1 OpenCV > 2.4.3 not found

**报错如下:** 

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6efe051c508ef3fedfc3013272b0b256.png#pic_center)

**解决方法:**

删掉 CMakeLists.txt 文件中 `find_package(OpenCV 3.0 QUIET)` 的指定版本。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0b117dd40dc922d7cf11f09c2dd2b67e.png#pic_center)


### 5.2.2 fatal error: sophus/se3.hpp: No such file or directory


**报错如下:**

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b4c6a4b7b8d9ed7a86b24bb920bb9684.png#pic_center)

**解决方法:**

在 CMakeLists.txt 文件中的 `include_directories()` 加入 `${PROJECT_SOURCE_DIR}/../../../Thirdparty/Sophus`。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/089cc600bc7fa1dccc838ea8777bf696.png#pic_center)

### 5.2.3 About AR


**报错如下:**

```txt
/home/polaris/workspace/ORB_SLAM3/Examples_old/ROS/ORB_SLAM3/src/AR/ViewerAR.cc:405:53: error: no matching function for call to ‘std::vector<cv::Mat>::push_back(Eigen::Vector3f)’
  405 |                 vPoints.push_back(pMP->GetWorldPos());
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1d7f71ab5b19c3e1f1e902223fdac428.png#pic_center)

```txt
/home/polaris/workspace/ORB_SLAM3/Examples_old/ROS/ORB_SLAM3/src/AR/ViewerAR.cc:530:42: error: conversion from ‘Eigen::Vector3f’ {aka ‘Eigen::Matrix<float, 3, 1>’} to non-scalar type ‘cv::Mat’ requested
  530 |             cv::Mat Xw = pMP->GetWorldPos();
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5bcdcd7f8d1187ab7218a3531e51be19.png#pic_center)

```txt
/home/polaris/workspace/ORB_SLAM3/Examples_old/ROS/ORB_SLAM3/src/AR/ros_mono_ar.cc:151:41: error: conversion from ‘Sophus::SE3f’ {aka ‘Sophus::SE3<float>’} to non-scalar type ‘cv::Mat’ requested
  151 |     cv::Mat Tcw = mpSLAM->TrackMonocular(cv_ptr->image,cv_ptr->header.stamp.toSec());
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/122ea8357ef79a881b21525d5c570a06.png#pic_center)


**解决方法:**
> 参考 issue: [Link](https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/479#issuecomment-1023779180)

我用不到 AR 的相关函数，所以选择将 CMakeLists.txt 文件中 AR 相关的编译部分注释掉。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4c49a1c2026dab4339640ea1d3e1dab3.png#pic_center)


# 6 使用 EuRoC 和 TUM-VI 运行 ORB-SLAM3

## 6.1 Notes

- GitHub README 中提到的 `euroc_examples.sh` 和 `tum_vi_examples.sh` 在历史提交中可以找到: [Link](https://github.com/UZ-SLAMLab/ORB_SLAM3/tree/def2d2d97d1c1ecabdd5725aa76b658b05fce4c8)
- 如果运行时，没有可视化 GUI 出现，请检查对应 `.cc` 文件的这一行，其中最后一个参数如果为 false，改为 true 即可

```cpp
ORB_SLAM3::System SLAM(argv[1],argv[2],ORB_SLAM3::System::MONOCULAR, false);
```

## 6.2 EuRoC

1. 下载数据: [Link](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0a254a27e212279cd7cb23d584db1920.png#pic_center)


2. 以 `Stereo` 为例，运行测试
```bash
roscore
```

每个 `.cc` 文件都会对其生成的可执行文件的所需命令行参数进行说明，以 `ros_stereo.cc` 为例:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7f3f32f890e5771b279ca76b91dfe12d.png#pic_center)

```bash
rosrun ORB_SLAM3 Stereo Vocabulary/ORBvoc.txt Examples_old/Stereo/EuRoC.yaml true
```

播放 bag 时要对话题名进行映射，所需的话题名在对应的 `.cc` 文件中可以找到，以 `ros_stereo.cc` 为例:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7df3de6ebdf7168a5b46176b221e54c1.png#pic_center)

```bash
rosbag play --clock V1_03_difficult.bag /cam0/image_raw:=/camera/left/image_raw /cam1/image_raw:=/camera/right/image_raw
```

## 6.3 TUM-VI

1. 下载数据: [Link](https://cvg.cit.tum.de/data/datasets/visual-inertial-dataset)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bcdba6d8081e7bc92f73b479e4d8b414.png#pic_center)

2. 以 `stereo_inertial` 为例，运行测试


在 `stereo_inertial_tum_vi.cc` 文件中可以找到，其生成的可执行文件所需的命令行参数，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9bf99aecba7cf093e533807b22332fca.png#pic_center)

```bash
./Examples/Stereo-Inertial/stereo_inertial_tum_vi Vocabulary/ORBvoc.txt Examples/Stereo-Inertial/TUM-VI.yaml ~/Datasets/TUM-VI/dataset-corridor1_512_16/mav0/cam0/data ~/Datasets/TUM-VI/dataset-corridor1_512_16/mav0/cam1/data Examples/Stereo-Inertial/TUM_TimeStamps/dataset-corridor1_512.txt Examples/Stereo-Inertial/TUM_IMU/dataset-corridor1_512.txt dataset-corridor1_512_stereoi
```

结束后，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f66242ae44ec643c59d08d2bd7d370e1.png#pic_center)


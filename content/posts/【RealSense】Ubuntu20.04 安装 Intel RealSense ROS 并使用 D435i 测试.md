@[TOC](【RealSense】Ubuntu20.04 安装 Intel RealSense ROS 并使用 D435i 测试)

# 1 本机环境

- Ubuntu20.04
- ROS Noetic

# 2 安装流程
>参考文档: [Link](https://github.com/IntelRealSense/realsense-ros/tree/ros1-legacy?tab=readme-ov-file#method-2-the-realsense-distribution)

1. 安装 Intel® RealSense™ SDK 2.0，参考上一篇文章: [Link](https://blog.csdn.net/G_C_H/article/details/136907206#t6)

2. 从源码安装 Intel® RealSense™ ROS

- 创建 catkin 工作空间

```bash
mkdir -p ~/workspace/realsense_ws/src
cd ~/workspace/realsense_ws/src
```
- Clone 最新的 Intel® RealSense™ ROS

```bash
git clone https://github.com/IntelRealSense/realsense-ros.git
cd realsense-ros/
git checkout `git tag | sort -V | grep -P "^2.\d+\.\d+" | tail -1`
cd ..
```

- 确保已安装 ros 软件包 `ddynamic_reconfigure`

```bash
sudo apt install ros-noetic-ddynamic-reconfigure
```

- 编译并安装
```bash
catkin_init_workspace
cd ..
catkin_make clean
catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
catkin_make install
```

- 测试

```bash
roslaunch realsense2_camera rs_camera.launch
```
```bash
rviz
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/935fe75c8d6620c059d58391c5c98f3c.png#pic_center)


# 3 存在的 bug

## 3.1 Resource not found: rgbd_launch

运行 `roslaunch realsense2_camera rs_rgbd.launch` 时报错如下:

```txt
Resource not found: rgbd_launch
ROS path [0]=/opt/ros/noetic/share/ros
ROS path [1]=/home/chenhui/workspace/realsense_ws/src
ROS path [2]=/opt/ros/noetic/share
The traceback for the exception was written to the log file
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6aba9cb587833ecbcafe92c1b5e5cfd2.png#pic_center)

解决方法:

>参考 issue: [Llink](https://github.com/IntelRealSense/realsense-ros/issues/1824#issuecomment-831789235)

```bash
sudo apt install ros-noetic-rgbd-launch
```


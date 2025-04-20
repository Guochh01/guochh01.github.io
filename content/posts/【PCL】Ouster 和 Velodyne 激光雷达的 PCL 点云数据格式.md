@[TOC](【PCL】Ouster 和 Velodyne 激光雷达的 PCL 点云数据格式)
# 0 news

Ouster 和 Velodyne 两公司合并。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a81393ea25ab24a247bc2ed215bdf97f.png)


# 1 Ouster
>GitHub: [Link](https://github.com/ouster-lidar/ouster-ros/blob/3f01e1d7001d8d21ac984566d17505b98905fa86/include/ouster_ros/os_point.h)

```cpp
namespace ouster_ros {

struct EIGEN_ALIGN16 Point {
    PCL_ADD_POINT4D;
    float intensity;
    uint32_t t;
    uint16_t reflectivity;
    uint16_t ring;
    uint16_t ambient;
    uint32_t range;
    EIGEN_MAKE_ALIGNED_OPERATOR_NEW
};
}  // namespace ouster_ros

POINT_CLOUD_REGISTER_POINT_STRUCT(ouster_ros::Point,
    (float, x, x)
    (float, y, y)
    (float, z, z)
    (float, intensity, intensity)
    // use std::uint32_t to avoid conflicting with pcl::uint32_t
    (std::uint32_t, t, t)
    (std::uint16_t, reflectivity, reflectivity)
    (std::uint16_t, ring, ring)
    (std::uint16_t, ambient, ambient)
    (std::uint32_t, range, range)
)
```

# 2 Velodyne

>GitHub: [Link](https://github.com/ros-drivers/velodyne/blob/29abd0e1361cb7f5eda451d2b51c35eeca45e0d5/velodyne_pcl/include/velodyne_pcl/point_types.h)

```cpp
namespace velodyne_pcl
{
struct PointXYZIRT
{
  PCL_ADD_POINT4D;                    // quad-word XYZ
  float         intensity;            ///< laser intensity reading
  std::uint16_t ring;                 ///< laser ring number
  float         time;                 ///< laser time reading
  EIGEN_MAKE_ALIGNED_OPERATOR_NEW     // ensure proper alignment
}
EIGEN_ALIGN16;
}  // namespace velodyne_pcl

POINT_CLOUD_REGISTER_POINT_STRUCT(velodyne_pcl::PointXYZIRT,
                                  (float, x, x)
                                  (float, y, y)
                                  (float, z, z)
                                  (float, intensity, intensity)
                                  (std::uint16_t, ring, ring)
                                  (float, time, time))
```

# 3 数据类型转换

下面的 demo 将 Ouster 点云数据类型转换为 pcl 内置的 `pcl::PointXYZINormal`。

```cpp
pcl::PointCloud<ouster_ros::Point> pl_orig;
pcl::PointCloud<pcl::PointXYZINormal> cloud_out;
pcl::fromROSMsg(*msg, pl_orig);
int plsize = pl_orig.size();
cloud_out.reserve(plsize);

double blind = 4; // 去除以雷达为中心的距离内的点云
for (int i = 0; i < pl_orig.points.size(); i++) {
    double range = pl_orig.points[i].x * pl_orig.points[i].x + pl_orig.points[i].y * pl_orig.points[i].y +
                   pl_orig.points[i].z * pl_orig.points[i].z;

    if (range < (blind * blind)) continue;

    pcl::PointXYZINormal added_pt;
    added_pt.x = pl_orig.points[i].x;
    added_pt.y = pl_orig.points[i].y;
    added_pt.z = pl_orig.points[i].z;
    added_pt.intensity = pl_orig.points[i].intensity;
    added_pt.normal_x = 0;
    added_pt.normal_y = 0;
    added_pt.normal_z = 0;
    added_pt.curvature = pl_orig.points[i].t / 1e6;  // 单位: ms

    cloud_out_.points.push_back(added_pt);
}
```

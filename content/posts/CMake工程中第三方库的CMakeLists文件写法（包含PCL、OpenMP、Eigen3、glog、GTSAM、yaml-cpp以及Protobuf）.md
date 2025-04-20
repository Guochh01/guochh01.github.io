@[TOC](CMake工程中第三方库的CMakeLists文件写法)

# 1 PCL
> PCL官网: [Link](https://pointclouds.org/documentation/tutorials/using_pcl_pcl_config.html)

```cmake
find_package(PCL REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

target_link_libraries(xxx ${PCL_LIBRARIES})
```

# 2 OpenMP
> CMake官网: [Link](https://cmake.org/cmake/help/latest/module/FindOpenMP.html)

> stack overflow: [Link](https://stackoverflow.com/questions/12399422/how-to-set-linker-flags-for-openmp-in-cmakes-try-compile-function)

```cmake
find_package(OpenMP REQUIRED)

target_link_libraries(xxx OpenMP::OpenMP_CXX)
```

# 3 Eigen3
> Eigen官网: [Link](https://eigen.tuxfamily.org/dox/TopicCMakeGuide.html)

```cmake
find_package (Eigen3 REQUIRED NO_MODULE)

include_directories(${EIGEN3_INCLUDE_DIR})

target_link_libraries (xxx Eigen3::Eigen)
```

# 4 glog
> GitHub: [Link](https://github.com/google/glog#consuming-glog-in-a-cmake-project)

```cmake
find_package (glog REQUIRED)

target_link_libraries (xxx glog::glog)
# target_link_libraries (xxx glog)
```

# 5 GTSAM

```cmake
find_package(GTSAM REQUIRED)

include_directories(${GTSAM_INCLUDE_DIR})

target_link_libraries(xxx gtsam)
```

# 6 yaml-cpp

> GitHub PR: [Link](https://github.com/jbeder/yaml-cpp/pull/446)

```cmake
find_package(yaml-cpp REQUIRED)

target_link_libraries(xxx yaml-cpp)
```

# 7 Protobuf
> CMake官网: [Link](https://cmake.org/cmake/help/latest/module/FindProtobuf.html)

```cmake
find_package(Protobuf REQUIRED)
include_directories(${Protobuf_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

# 生成cpp文件
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS foo.proto)
# 生成py文件
protobuf_generate_python(PROTO_PY foo.proto)

add_executable(xxx ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(xxx ${Protobuf_LIBRARIES})
```

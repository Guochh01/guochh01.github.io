@[TOC](【Ubuntu】在 Ubuntu20.04 Anaconda 下 OpenVINO 环境配置)
# 1 Anaconda 的安装
看我的[这篇文章](https://blog.csdn.net/G_C_H/article/details/125662984)即可。
# 2 OpenVINO 环境配置
- [OpenVINO 官网](https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/download.html)获取相应配置的命令
>下图是我的配置：
>![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3997c82e36435f99751e894068c39c14.png#pic_center =800x)
>![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8414ba906563e98f9611d39f2d1630a8.png#pic_center =800x)
- 输入相应的安装命令
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/187eff9a21418b6243d06851f2616431.png#pic_center =800x)
- `pip` 最好换为清华源（阿里源安装 `onnx` 会失败）
```bash
# 更新 pip 版本
python -m pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade pip
# 配置为清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
- 在 `conda` 虚拟环境中安装 `OpenVINO`（`python` 版本要满足 `openvino` 对系统的要求）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d243e6e5ce9cb78426c5a0aace415c6e.png#pic_center =800x)

```bash
# 创建虚拟环境
conda create -n openvino python=3.6
# 激活虚拟环境
conda activate openvino
# 安装 OpenVINO
pip install openvino-dev[onnx,pytorch,tensorflow]==2021.4.2
```
>在安装期间，我还做了一些操作：
>- 安装了 `protobuf`（如果安装 `onnx` 时有报错，可以运行下方命令）:
>
>`sudo apt install protobuf-compiler libprotoc-dev`
>- 更新了 `cmake`
>
>参考这篇文章：[Ubuntu 升级 Cmake的正确方式](https://blog.csdn.net/qq_27350133/article/details/121994229)
- 检查是否安装成功
```bash
pip list
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7e1282bdcfc489f239932e25c402eabd.png#pic_center =600x)








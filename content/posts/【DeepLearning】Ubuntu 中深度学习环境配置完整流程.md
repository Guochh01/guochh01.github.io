@[TOC](Ubuntu 中深度学习环境配置完整流程)

# 1 显卡驱动
> 支持 CUDA 的所有显卡型号: [Link](https://developer.nvidia.com/cuda-gpus)

1. 查询显卡型号
```bash
lspci -nn | grep VGA
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8e36db77736a4efc80e567c1cbc6fa98.png#pic_center)

即 `Vendor ID:Device ID` 为 `10de:2684`，在 [Link](https://devicehunt.com/search/type/pci/vendor/any/device/any) 或浏览器中搜索。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/16934c464f78493db0bb5dfb86dc0083.png#pic_center)

2. 填写显卡信息: [Link](https://www.nvidia.com/en-us/geforce/drivers/)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5aef77a949fd4a77a0a49fd2f27d1089.png#pic_center)

3. 选择要下载的版本（可以选个新一点的 ）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0d78c293b51440b9a54a33835e85c41f.png#pic_center)


4. 运行 `.run` 文件

```bash
sudo sh ./NVIDIA-Linux-x86_64-*.run
```

5. 测试
```bash
nvidia-smi
```

# 2 CUDA 
>参考文档: [Link](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

1. 选择要安装的版本: [Link](https://developer.nvidia.com/cuda-toolkit-archive)

- 先通过 `nvidia-smi` 查看驱动支持的 CUDA 最高版本，我的最高版本为 12.4
- 然后在此范围内选择项目中比较常用的 CUDA 版本，只要低于最高版本都可以

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0fc2f7d6583c4df4ae5a0a35f684e891.png#pic_center)


2. 查询本机系统信息

```bash
uname -m && cat /etc/*release
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/91903940816f47e192b433fb1b3e6025.png#pic_center)


3. 选择你的平台，下载相应的 `.run` 文件并运行

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4968bf6b885042009347c234570cc716.png#pic_center)

安装完成后，得到下面的输出信息。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/eaa2084dd22544ceb79d3872a02c30fe.png#pic_center)


4. 修改 `PATH` 和 `LD_LIBRARY_PATH` 变量来设置开发环境
>参考文档: [Link](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html#id8)

```bash
vim ~/.bashrc

# 添加以下内容
# >>> cuda >>>
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
# <<< cuda <<<
```

5. 测试

```bash
nvcc -V
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/552303b5f7da482caf2969e575eb65d5.png#pic_center)


# 3 cuDNN

>参考文档: [Link](https://docs.nvidia.com/deeplearning/cudnn/latest/reference/support-matrix.html#support-matrix)

CUDA 和 cuDNN 的兼容性

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1f06b7eeb93147a980fb08e97d6d4b83.png#pic_center)

## 3.1 cuDNN 9.0.0 之前版本

1. 安装 `Zlib`
```bash
sudo apt install zlib1g
```

2. 下载 cuDNN: [Link](https://developer.nvidia.com/cudnn-archive)，要注册个帐号

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e4b7326d38d7accfc133c472cd84c679.png#pic_center)

3. 根据安装的 CUDA 版本选择 cuDNN 版本，可以选新一点的

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f571751e77eb9953a4e5fd519a031416.png#pic_center)


4. 下载压缩包

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5a250a054be759a8d2ac91c73943ed3e.png#pic_center)

```bash
# 解压下载的文件
tar -xvf cudnn-linux-x86_64-8.x.x.x_cudaX.Y-archive.tar.xz

# 复制到 CUDA 的目录下
sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

5. 测试（有点麻烦，可以忽略）

```bash
sudo apt-get install libcudnn8=${cudnn_version}-1+${cuda_version}
sudo apt-get install libcudnn8-dev=${cudnn_version}-1+${cuda_version}
sudo apt-get install libcudnn8-samples=${cudnn_version}-1+${cuda_version}
```
>${cudnn_version} = 8.x.x.x
>${cuda_version} = cuda12.1 or cuda11.8...
>**Note:** 以自己安装的版本为准！

```bash
cp -r /usr/src/cudnn_samples_v8/ $HOME
cd  $HOME/cudnn_samples_v8/mnistCUDNN
make clean && make
./mnistCUDNN
```
如果 cuDNN 正确安装和运行，你会看到类似以下的信息：`Test passed!`


## 3.2 cuDNN 9.0.0 之后版本

>参考文档: [Link](https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html#upgrading-from-older-versions-of-cudnn-to-cudnn-9-x-y)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/582c6eeff83a4e84a6a3e17b4264dcfd.png#pic_center)

cuDNN 9 可以与之前的 cuDNN 版本共存，如果有旧版本的 cuDNN，安装 cuDNN 9 时不会自动删除旧版本。

如果要在旧版本与新版本之间切换，执行 `sudo update-alternatives --config libcudnn` 并选择相应的 cuDNN 版本。

下面是安装步骤：


>参考文档: [Link](https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html#package-manager-local-installation)


1. 选择安装的版本: [Link](https://developer.nvidia.com/cudnn-archive)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d5728920b6274d3c94245ace7ecbed51.png#pic_center)


2. 选择你的平台，下载相应的软件包

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e8cc6b96f6984d4d996d019cabf95ce5.png#pic_center)



```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.6.0/local_installers/cudnn-local-repo-ubuntu2004-9.6.0_1.0-1_amd64.deb

sudo dpkg -i cudnn-local-repo-ubuntu2004-9.6.0_1.0-1_amd64.deb

sudo cp /var/cudnn-local-repo-ubuntu2004-9.6.0/cudnn-*-keyring.gpg /usr/share/keyrings/

sudo apt-get update
```

3. 安装 cuDNN

- Install for CUDA 11
```bash
sudo apt-get -y install cudnn9-cuda-11
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/29e775d6dd1a433ca11b1d22f1ff040d.png#pic_center)


- Install for CUDA 12
```bash
sudo apt-get -y install cudnn9-cuda-12
```

## 3.3 pip 安装 cuDNN 9.0.0 之后版本

> 参考文档: [Link](https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html#python-wheels-linux-installation)

NVIDIA 提供了通过 pip 安装 cuDNN 的 Python Wheels，但是在 pip 环境之外使用 cuDNN 时，还须配置主机环境。


1. 更新 pip 和 wheel 模块
```bash
python3 -m pip install --upgrade pip wheel
```

2. 安装 cuDNN

- Install for CUDA 11
```bash
python3 -m pip install nvidia-cudnn-cu11
```
若要指定 cuDNN 版本，运行:

```bash
python3 -m pip install nvidia-cudnn-cu11==9.x.y.z
```

- Install for CUDA 12
```bash
python3 -m pip install nvidia-cudnn-cu12
```
若要指定 cuDNN 版本，运行:

```bash
python3 -m pip install nvidia-cudnn-cu12==9.x.y.z
```
# 4 torch

1. 根据 cuda 和自身需求确定要安装的版本
2. 下载 `.whl` 文件: [Link](http://download.pytorch.org/whl/torch/)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/40a973abfa91908c520f26e3207a2757.png#pic_center)

3. 安装

```bash
conda activate xxx
pip install torch-*+cu*-cp*-cp*m-linux_x86_64.whl
```

4. 测试
```bash
python

>>> import torch
>>> torch.cuda.is_available()
True
>>> torch.__version__
'1.12.1+cu113'
```

# 5 torchvision

1. 根据 torch 选择对应的 torchvision 版本: [Link](https://github.com/pytorch/vision)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/798a12755cf741ce9f111e7f5448b561.png#pic_center)


2. 下载 `.whl` 文件: [Link](https://download.pytorch.org/whl/torchvision/)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ff65bf09de456c961ec16bccb0b5a64a.png#pic_center)

3. 安装

```bash
conda activate xxx
pip install torchvision-*+cu*-cp*-cp*m-linux_x86_64.whl
```
4. 测试

```bash
python

>>> import torchvision
>>> torchvision.__version__
'0.13.1+cu113'
```

# 6 torchaudio

1. 根据 torch 选择对应的 torchaudio 版本: [Link](https://pytorch.org/audio/main/installation.html#compatibility-matrix)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/90a1943050774c6c9edfe884e1e88976.png#pic_center)

2. 下载 `.whl` 文件: [Link](https://download.pytorch.org/whl/torchaudio/)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/979de2f756f54a87a7c79e2bf4e4fa26.png#pic_center)

3. 安装

```bash
conda activate xxx
pip install torchaudio-*+cu*-cp*-cp*m-linux_x86_64.whl
```

4. 测试

```bash
python

>>> import torchaudio
>>> torchaudio.__version__
'0.12.1+cu113'
```


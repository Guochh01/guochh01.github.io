@[TOC](【PyTorch】libtorch_cpu.so: undefined symbol: iJIT_NotifyEvent)

# 1 报错信息

```bash
conda install pytorch=1.13.1 torchaudio=0.13.1 torchvision=0.14.1 cudatoolkit=11.7 -c pytorch
```

在 conda 环境中安装 torch 后测试，得到下面的错误：

```bash
>>> import torch
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/polaris/anaconda3/envs/xxx/lib/python3.9/site-packages/torch/__init__.py", line 218, in <module>
    from torch._C import *  # noqa: F403
ImportError: /home/polaris/anaconda3/envs/xxx/lib/python3.9/site-packages/torch/lib/libtorch_cpu.so: undefined symbol: iJIT_NotifyEvent
```

# 2 原因
>参考 issue: [Link](https://github.com/pytorch/pytorch/issues/123097#issuecomment-2055236551)

这个符号在 MKL 2024.1 中被移除了，然而 PyTorch 是根据旧版本的 MKL 构建的。 MKL 全称 Intel Math Kernel Library， 是由 Intel 公司开发的，专门用于矩阵计算的库。

- 通过 conda 发布的 PyTorch 二进制文件是动态链接到 MKL 的，所以会遇到这个错误。 
- 通过 pip 发布的 PyTorch 二进制文件是静态链接到 MKL 的（ 可以改用 pip install 来消除 MKL 的这个错误）

# 3 解决方法

>参考 issue: [Link](https://github.com/pytorch/pytorch/issues/123097#issuecomment-2301747689)

```bash
conda install mkl==2024.0
```

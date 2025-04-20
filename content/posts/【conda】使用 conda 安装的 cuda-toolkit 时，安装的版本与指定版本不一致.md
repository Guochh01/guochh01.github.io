

@[TOC](【conda】使用 conda 安装的 cuda-toolkit 时，安装的版本与指定版本不一致)

# 1 问题描述 

>参考博客: [Link](https://blog.csdn.net/weixin_43933424/article/details/139304567?spm=1001.2014.3001.5502)


与参考博客的问题相似，我本机是 `cuda 11.8`，使用 conda 安装 `cuda-toolkit 12.1`，但最终 `nvcc -V` 显示为 12.6

执行 `conda list` 后发现安装 cuda 相关的包来自不同的 channel:

- nvidia
- nvidia/label/cuda-12.1.0
- conda-forge

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5c6cc8eee46648d98f6f568d8691f4c5.png#pic_center)

# 2 channel 介绍

## 2.1 conda-forge

>参考文档: [Link](https://conda-forge.org/docs/user/introduction/)

conda-forge 是一个社区项目，为各种软件提供 conda 包，所有 conda-forge 包都可以在这里找到: [Link](https://anaconda.org/conda-forge)

## 2.2 nvidia

nvidia 的所有包都可以在这里找到: [Link](https://anaconda.org/nvidia)

### 2.2.1 cuda-toolkit

安装 cuda-toolkit 后会改变虚拟环境中的 cuda 版本，所有版本 cuda-toolkit 的 conda channel: [Link](https://anaconda.org/nvidia/cuda-toolkit)

如果要指定版本安装，应该选择相应的 channel，否则会安装最新的版本。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7c32b8b80ef84c238c256bf7242a3497.png#pic_center)


# 3 原因

>参考文档: [Link](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html#managing-channels)


我使用 .yml 文件来创建环境并安装各个包
```bash
conda env create -f /path/to/environment.yml
```

下面是我初始的 .yml 文件内容:

```yaml
channels:
  - pytorch
  - nvidia
  - nvidia/label/cuda-12.1.0
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - faiss-gpu=1.8.0
  - cuda-toolkit=12.1
  - pytorch=2.1.2
  - pytorch-cuda=12.1 
  - torchvision=0.16.2
```

默认情况下，conda 会优先选择来自高优先级 channel 的包，所以由于优先级的问题，导致 cuda-toolkit 中各个包安装的来源不同，版本不一致。

# 4 解决方法

1. 删除原环境

```bash
conda remove -n xxx --all
```

2. 修改 `.yml` 文件，将 `nvidia/label/cuda-12.1.0` 的优先级提前

```yml
channels:
  - pytorch
  - nvidia/label/cuda-12.1.0
  - nvidia
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - faiss-gpu=1.8.0
  - cuda-toolkit=12.1
  - pytorch=2.1.2
  - pytorch-cuda=12.1 
  - torchvision=0.16.2
```

3. 创建环境并安装

```bash
conda env create -f environment.yml
```

如果过程中出错造成中断，可以通过 update 继续安装

```bash
conda activate xxx
conda env update -f environment.yml
```


安装完成后，就是 `cuda 12.1` 了


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0896ce791cdd40d2970253e48dde4c2b.png#pic_center)


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1c3619a04e89442bb713fa03009df195.png#pic_center)


其中 `cuda-version` 这个包的 channel 为 conda-forge 的原因是在 conda 的 `nvidia/label/cuda-12.1.0` 这个 channel 中找不到 `cuda-version 12.1.0`，所以在低优先级的 channel 检索并安装。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/58eefeb1a7df46c7859799983c0a264d.png#pic_center)



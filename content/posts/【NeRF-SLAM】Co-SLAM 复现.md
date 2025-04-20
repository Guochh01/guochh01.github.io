@[TOC](【NeRF-SLAM】Co-SLAM 复现)

# 1 环境配置


```bash
/home/gch/anaconda3/envs/coslam/compiler_compat/ld: cannot find -lcuda: No such file or directory
collect2: error: ld returned 1 exit status
error: command '/usr/bin/g++' failed with exit code 1
```


>参考 issue: [Link](https://github.com/NVlabs/tiny-cuda-nn/issues/183#issuecomment-1342828785)


```bash
export LIBRARY_PATH="/usr/local/cuda/lib64/stubs:$LIBRARY_PATH"
pip install -r requirements.txt
```


>参考文档: [Link](https://docs.nvidia.cn/cuda/wsl-user-guide/index.html)

```bash
bash scripts/naruto/run_replica.sh office0 1 NARUTO 0
```

报错信息如下:

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f18a1e6460bd47bca31493df89c4a2b7.png#pic_center)

```bash
Platform::WindowlessEglApplication::tryCreateContext(): cannot get default EGL display: EGL_BAD_PARAMETER
WindowlessContext: Unable to create windowless context
```


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9d4f429386454a82bbc9a9bcb00a2ff7.png#pic_center)


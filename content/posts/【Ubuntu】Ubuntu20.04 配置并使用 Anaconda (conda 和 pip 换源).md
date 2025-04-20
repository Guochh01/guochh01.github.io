@[TOC](【Ubuntu】Ubuntu20.04 配置并使用 Anaconda)
# 1 安装 Anaconda
- [官网](https://www.anaconda.com/products/distribution#Downloads)下载安装脚本
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8202b607c06d00893af005f7d8b8452d.png#pic_center =700x)
- 运行 `.sh` 脚本
```bash
bash /home/terminal/下载/Anaconda3-2022.05-Linux-x86_64.sh
```
> - 按 `ENTER` 往下滑动，阅读协议
> - 输入 `yes` 同意协议
> - 按 `Enter` 安装在默认位置（`~/anaconda3/`）
> - 是否想要运行 `conda init`，输入 `yes`

- 加载环境变量
```bash
source ~/.bashrc
```
- 验证
```bash
conda
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/33b88537bf2d6486a6b253d6f5b980c4.png#pic_center =700x)

# 2 conda 换源
- 选择更换为[阿里源](https://developer.aliyun.com/mirror/anaconda)

- 用户目录下创建 `.condarc` 文件
```bash
touch .condarc
```
- 写入配置（最好自己去复制官网的）
```txt
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://mirrors.aliyun.com/anaconda/pkgs/main
  - http://mirrors.aliyun.com/anaconda/pkgs/r
  - http://mirrors.aliyun.com/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors.aliyun.com/anaconda/cloud
  msys2: http://mirrors.aliyun.com/anaconda/cloud
  bioconda: http://mirrors.aliyun.com/anaconda/cloud
  menpo: http://mirrors.aliyun.com/anaconda/cloud
  pytorch: http://mirrors.aliyun.com/anaconda/cloud
  simpleitk: http://mirrors.aliyun.com/anaconda/cloud
```
- 清除之前的索引缓存
```bash
 conda clean -i 
```
- 安装一个包验证一下即可
```bash
conda activate xxx
conda install xxx
```

# 3 pip 换源
- 选择更换为[阿里源](https://developer.aliyun.com/mirror/pypi)
- 寻找或创建 `~/.pip/pip.conf` 文件
```bash
mkdir ~/.pip
cd ~/.pip
touch pip.conf
```
- 写入配置（最好自己去复制官网的）
```txt
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
- 验证
```bash
pip config list
```

# 4 conda 的一些命令
## 4.1 创建虚拟环境
```bash
conda create -n env_name list of packages
```
- 例如：`conda create -n pose python=3.8`

## 4.2 删除虚拟环境
```bash
conda env remove -n xxx
```

## 4.3 查看已创建的虚拟环境
```bash
conda env list
```

## 4.4 进入、退出虚拟环境
```bash
conda activate xxx # 进入
conda deactivate   # 退出
```

# 5 批量导出和安装
>先进入要导出包或安装包的虚拟环境中
## 5.1 pip
- 批量导出环境中的包
```bash
pip freeze > requirements.txt
```
- 批量安装文件中的包
```bash
pip install -r requirements.txt
```
## 5.2 conda
- 批量导出环境中的包
```bash
conda list -e > requirements.txt
或者
conda env export > requirements.yaml
```
- 批量安装文件中的包
```bash
conda install --yes --file requirements.txt
或者
conda env create -f requirements.yaml
```

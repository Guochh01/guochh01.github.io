# 1 Command Line Interface (CLI)

## 1.1 教程

> 参考文档: [Link]((https://huggingface.co/docs/huggingface_hub/main/en/guides/cli)

1. 安装

```bash
 pip install -U "huggingface_hub[cli]"
```

2. 登录

```bash
huggingface-cli login
```

![image-20250327002850985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327002850985.png)

- 创建 Access Token: [Link](https://huggingface.co/settings/tokens )，复制粘贴 Token 后按 Enter

- 再按一次 Enter（默认为 Yes）

![image-20250327003559408](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327003559408.png)

3. 查询身份

```bash
huggingface-cli whoami
```

![image-20250327004132066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327004132066.png)

4. 设置 Hugging Face 镜像

```bash
# Linux
export HF_ENDPOINT=https://hf-mirror.com
# Windows PowerShell
$env:HF_ENDPOINT="https://hf-mirror.com"
```

5. 下载模型文件

- 下载单个文件

```bash
huggingface-cli download [repo_id] [filename]
```

- 下载多个文件

```bash
huggingface-cli download [repo_id] [filename] [filename]
# 使用 --include 和 --exclude 对文件进行过滤
huggingface-cli download [repo_id] --include "*.safetensors" --exclude "*.fp16.*"*
```

- 下载整个仓库

```bash
huggingface-cli download [repo_id]
# 指定 commit hash, branch name or tag
huggingface-cli download [repo_id] --revision xxx
# 安静模式（只输出保存路径）
huggingface-cli download [repo_id] --quiet
```

**（推荐修改）**默认下载路径为 `.cache/huggingface/hub/models--xxx/snapshots/xxx`，可以使用 `--local-dir` 自定义下载路径

```bash
# 指定下载路径
huggingface-cli download [repo_id] --local-dir xxx
```

**（推荐默认）**默认缓存路径为 `.cache/huggingface`，可以使用 `--cache-dir` 自定义缓存路径。缓存中包含下载文件的元数据，如果已经是最新的，就不会重新下载；如果元数据已更改，则会下载新版本的文件。

```bash
huggingface-cli download [repo_id] --cache-dir xxx/cache
```

- 下载数据集

```bash
huggingface-cli download [repo_id] --repo-type dataset
```

## 1.2 实践

```bash
huggingface-cli download deepseek-ai/deepseek-vl2-tiny --local-dir /data1/guochenhui/pre_train/huggingface/deepseek-ai/deepseek-vl2-tiny/
```

下载完成后查看缓存和下载文件的空间占用：

![image-20250327165150392](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327165150392.png)

如果不指定下载路径，则会默认下载到 `.cache/huggingface/hub` 下的目录。

![image-20250327165518357](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327165518357.png)

具体文件结构：

![image-20250327165746754](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250327165746754.png)

# 2 Python API

## 2.1 设置 Hugging Face 镜像

```python
import os
os.environ["HF_ENDPOINT"] = "https://hf-mirror.com"
```


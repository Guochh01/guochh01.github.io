

# 1 Framer 复现

## 1.1 Bug

==Bug 1==

> 参考问答: [Link](https://stackoverflow.com/a/79377102/18209110)

报错如下：

```bash
ImportError: cannot import name 'cached_download' from 'huggingface_hub' (/home/guochenhui/miniconda3/envs/framer/lib/python3.9/site-packages/huggingface_hub/__init__.py)
```

解决方法：

```bash
pip install huggingface-hub==0.25.2
```

==Bug 2==

> 参考 issue: [Link](https://github.com/gradio-app/gradio/issues/10662#issuecomment-2677236567)

运行`python app.py` 出现报错如下：

```bash
ERROR:    Exception in ASGI application
```

![image-20250329210138942](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329210138942.png)

解决方法：

```bash
pip install pydantic==2.10.6
```

![image-20250329210338908](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329210338908.png)

==Bug 3==

> 参考文档: [Link]([Sharing Your App](https://www.gradio.app/guides/sharing-your-app))

共享链接

```bash
To create a public link, set `share=True` in `launch()`.
```

修改 `app.py` 的代码：

```python
demo.launch(share=True)
```

运行 `python app.py` 出现信息如下：

```bash
Could not create share link. Missing file: /home/guochenhui/miniconda3/envs/framer/lib/python3.9/site-packages/gradio/frpc_linux_amd64_v0.2.
```

![image-20250329212148593](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329212148593.png)

按照提示步骤进行：

```bash
1. Download this file: https://cdn-media.huggingface.co/frpc-gradio-0.2/frpc_linux_amd64
2. Rename the downloaded file to: frpc_linux_amd64_v0.2
3. Move the file to this location: /home/guochenhui/miniconda3/envs/framer/lib/python3.9/site-packages/gradio
```

> 参考 issue: [Link](https://github.com/gradio-app/gradio/issues/3498#issuecomment-1651038942)

再次运行 `python app.py` 出现信息如下：

```bash
Could not create share link. Please check your internet connection or our status page: https://status.gradio.app.
```

![image-20250329213333833](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329213333833.png)

解决方法：

```bash
chmod +x ~/miniconda3/envs/framer/lib/python3.9/site-packages/gradio/frpc_linux_amd64_v0.2
```

![image-20250329215133978](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329215133978.png)

==Bug 4==

> 参考 issue: [Link](https://github.com/pytorch/vision/issues/8779#issuecomment-2517331226)

在 Web UI 中点击 Run 后，出现报错如下：

```bash
File "/home/guochenhui/workspace/Framer/app.py", line 219, in frames_to_video
    torchvision.io.write_video(output_video_path, video, fps=fps)
  File "/home/guochenhui/miniconda3/envs/framer/lib/python3.9/site-packages/torchvision/io/video.py", line 133, in write_video
    frame.pict_type = "NONE"
  File "av/video/frame.pyx", line 217, in av.video.frame.VideoFrame.pict_type.__set__
TypeError: an integer is required
```

![image-20250329220640996](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250329220640996.png)

解决方法：

```bash
pip install "av<14"
```


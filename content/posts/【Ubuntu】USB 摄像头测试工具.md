@[toc](【Ubuntu】USB 摄像头测试工具)
# 1 查询现有的摄像头设备
```bash
ll /dev/video*
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9a713b10fae78eaed7cd2300e8e59c33.png#pic_center =600x)
# 2 v4l2-ctl 工具包
## 2.1 安装
```bash
sudo apt install v4l-utils
```
## 2.1 常用命令
- 查询现有的摄像头设备
```bash
v4l2-ctl --list-device
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/74e301ecebb62ceda98ef752e47510c1.png#pic_center =600x)


- 查询指定设备的所有信息
```bash
v4l2-ctl -d /dev/video0 --all
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a41cbdc035c69c8c2d2639c2707420f1.png#pic_center =700x)
```bash
v4l2-ctl -d /dev/video1 --all
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1a43aa199c1c58571aa6df2bac6abb82.png#pic_center =700x)
- 查询摄像头支持的视频压缩格式

```bash
v4l2-ctl -d /dev/video0 --list-formats
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0dae228807173b11cff9eaaa47409e56.png#pic_center =700x)
> 一般 `USB` 摄像头都是免驱的（`UVC` 驱动）,且通常有 `YUYC` 和 `MJPG` 两种视频输出格式。
> - `YUYC` ：无压缩，是 `YUV` 码流的一种存储方式 
> - `MJPG ` ：有压缩，是每一帧图像都采用 `JPEG` 编码的视频压缩编码格式


- 查询摄像头不同视频压缩格式所支持的分辨率
```bash
v4l2-ctl -d /dev/video0 --list-framesizes=MJPG
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5c3d0c448d6345e39273bf8718f07cc6.png#pic_center =700x)

# 3 cheese 软件
>这是 Ubuntu 自带的相机软件
- 安装
```bash
sudo apt install cheese
```
- 启动指定摄像头
```bash
cheese -d /dev/video0
```
# 4 解决 OpenCV VideoCapture 中 select() timeout 报错
[参考文章](https://www.cnblogs.com/brt2/p/14156763.html)


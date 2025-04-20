@[toc](【Ubuntu】解决 PyQt 和 OpenCV-Python 同时使用后报错)
# 1 报错信息
`error` 如下：
```txt
qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in ".../lib/python3.8/site-packages/cv2/qt/plugins" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.
```
# 2 错误原因
`opencv-python` 与 `pyqt` 的 `GUI` 库产生冲突
# 3 解决方法
>当使用除 `OpenCV` 之外的软件包创建 `GUI` 时，应该选择安装 `opencv-python-headless` 或者 `opencv-contrib-python-headless`（但是这样就无法使用 `cv2.imshow()` 等方法了）
>
>----
>两个包的区别：
>- `opencv-python`: Main modules package
>- `opencv-contrib-python`: Full package (contains both main modules and contrib/extra modules)

`headless` 即不依赖 `GUI` 库。



[PyPi 官方文档解释](https://pypi.org/project/opencv-python-headless/)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e61a27d50f91dcdf6dcb0ec3fda16a2f.png#pic_center =800x)
- 卸载当前的 `opencv-python`
```bash
pip uninstall opencv-python
```
- 重新安装无 `GUI` 库的 `opencv-python`
```bash
pip install opencv-python-headless
```

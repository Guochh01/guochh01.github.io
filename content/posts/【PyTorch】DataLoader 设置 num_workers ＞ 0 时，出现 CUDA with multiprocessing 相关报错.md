@[TOC](【PyTorch】DataLoader 设置 num_workers > 0 时，出现 CUDA with multiprocessing 相关报错)
# 1 报错信息

```txt
RuntimeError: Caught RuntimeError in DataLoader worker process 0.

RuntimeError: Cannot re-initialize CUDA in forked subprocess. To use CUDA with multiprocessing, you must use the 'spawn' start method
```

# 2 报错分析
## 2.1 原因

Tensor 默认是在 CPU 上创建的，当我在数据集 Class 的 `__getitem__()` 中 return 时，将 Tensor 转移到了 GPU 上

```python
return (
	color.to(self.device).type(self.dtype),
	depth.to(self.device).type(self.dtype),
	intrinsics.to(self.device).type(self.dtype),
	pose.to(self.device).type(self.dtype),
	# self.retained_inds[index].item(),
)
```

同时，我在 DataLoader 定义时，设置了 `num_workers`，导致数据在多进程加载时使用了 CUDA Tensor

```python
data_loader = DataLoader(dataset, num_workers=dataset_config['num_workers'])

# Iterate over Scan
for time_idx, batch in tqdm(enumerate(data_loader)):
```

## 2.2 结论

>参考文档: [Link](https://pytorch.org/docs/stable/data.html#multi-process-data-loading)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/238abad8695148ab80becd1b396649e3.png#pic_center)

- 不建议在多进程加载时返回 CUDA Tensor，因为在多进程中使用 CUDA 以及共享 CUDA Tensor 有一些要注意的点
- 相反，可以使用 DataLoader 中的 `pin_memory`，将数据传输到共享内存中，然后再将 Tensor 转移到 CUDA GPU 上


# 3 解决方法

> 参考discuss: [Link](https://discuss.pytorch.org/t/dataloader-multiprocessing-with-dataset-returning-a-cuda-tensor/151022)

修改数据集 Class 的 `__getitem__()`，在 return 时不将 Tensor 转移到 GPU 上。






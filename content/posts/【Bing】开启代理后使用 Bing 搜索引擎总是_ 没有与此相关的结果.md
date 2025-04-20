@[TOC](【Bing】开启代理后使用 Bing 搜索引擎总是: 没有与此相关的结果)

# 1 问题描述

当我开了代理访问 Bing 时，经常会出现下面的页面:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b201c1756b760a0ecaa99a588c5b9005.png)

# 2 解决方法

我所知的有三种方法:

1. 手动关闭代理
2. 修改分流规则
3. 改用其他代理节点

建议通过修改分流规则来解决，首先要确定机场的规则分组是否包含 Microsoft，我使用的机场可以在个人中心 $\Rightarrow$ 分流规则中自定义规则分组


## 2.1 修改分流规则

>参考文章: [Link](https://www.zhihu.com/question/606050870/answer/3130649388)

### 2.1.1 Clash Verge

- 选择 Proxies 	$\Rightarrow$ Rule
- 找到 Microsoft，选择 DIRECT (或其他可用节点)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d27b1ed3e39099e1fb14bcdcb49f71d7.png#pic_center)

### 2.1.2 Clash Verge Rev

- 选择 Proxies 	$\Rightarrow$ Rule
- 找到 Microsoft，选择 DIRECT (或其他可用节点)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/218f55a49ab41e56ecef8a8119cc7509.png#pic_center)


### 2.1.3 V2RayN

>参考 issue: [Link](https://github.com/2dust/v2rayN/issues/2290#issuecomment-1320811052)


- 选择 设置 $\Rightarrow$ 路由设置
- 双击进入 绕过大陆(Whitelist)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/eb25a0d0cdb0c36a9c52f7d78f1a2bb0.png#pic_center)

- 双击进入 direct

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/53a2d5fa014af4df18f24c19424dcb30.png#pic_center)

- 在 Domain 中填写下面的内容

```txt
domain:bing.com,
domain:bing.net
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/189803194b971e0626bfd3d6657c2740.png#pic_center)


有关 Domain 的格式，参考文档: [Link](https://www.v2fly.org/config/routing.html#ruleobject)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/aa8dd5b3c946d4a3a767b352049e448c.png#pic_center)

- 修改后可以发现，这两个 domain 有关的连接均为 `[http -> direct]`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/58509c25e3cf4bd89872c521c6a53d9c.png#pic_center)



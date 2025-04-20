@[TOC](Type-C接口)

---
# 一、参考文章
---
## （一） Type-c的24\16\6Pin引脚定义
- [一文读懂USB TypeC与USB-PD。TypeC引脚定义-24P 16P 6P，CC1、CC2的作用，USB-PD介绍，USB2.0/3.0接口类型一览](https://blog.csdn.net/Mark_md/article/details/114578359)
- [快充协议介绍（包含USB-PD）——b站视频](https://www.bilibili.com/video/BV1fU4y1A7oR)

## （二） DFP、UFP、CC等关键词的解析
- [Type-C和PD有何不同？CC、UFP、DFP、DRP关键名词用途解析](http://www.huohuasai.org.cn/news/d1/20200509/334585.html)
- [USB Type-C Configuration Channel (CC)引脚功能介绍](https://www.jianshu.com/p/b2a5fba90225)

---
# 二、USB Type-C性能特点
---
1.母座与公头连接时，==接口可翻转==
2.在24Pin的引脚下，支持==USB 2.0, USB 3.0（即USB 3.1 Gen1）和 USB 3.1 Gen2标准==等，以及多种第三方协议，如：==HDMI==

---
# 三、USB Type-C分类
---

按照接口引脚的数量，可以大致分为 **24Pin、16Pin、6Pin** 的 Type-C（在上文中列出的参考文章写的很棒）。
我仅对三种类型做一下简单的概述：

- 24Pin为全功能,但一般的 MCU 并不支持 `USB 3.0`，`USB 2.0` 在绝大多数情况都可以满足需求 
- 16Pin在24Pin全功能的基础上对 `USB 3.0` 的相关引脚进行了阉割，二者唯一的区别就是有无 `USB 3.0/3.1` 的高速传输
- 在日常生活中，很多产品甚至并不需要 `USB 通信`。所以在6Pin的版本中，仅保留了 `VBUS、GND、CC1、CC2` 这四类引脚。
==根据我的设计需求仅需要用到6Pin的类型，下文将对6Pin的Type-C进行详细的讲解。==
---
# 四、6Pin USB Type-C
---
![Alt](https://i-blog.csdnimg.cn/blog_migrate/da318e0217710203b244a135d6e0ca92.jpeg#pic_center =480x480)
 **封装如下：**
![Alt](https://i-blog.csdnimg.cn/blog_migrate/6714ffe42724e7f5cb99ab50b7cff672.png#pic_center)
**CC1与CC2**
`USB-PD对电源设备的识别` 是依靠这两个引脚的，向供电端请求电源供给。在简单的设计中，通过CC1和CC2各自独立 `下拉一个5.1k电阻到地` 即可。而对于需要用到大功率供电或者高清视频传输功能的嵌入式设计，则必须要使用USB-PD控制芯片。*（ 树莓派4上面的这个USB-C接口，其CC1和CC2是连接在一起的，并共用了一个5.1k的电阻下拉到地。由于它少使用了一个5.1k电阻，使得其与许多USB Type-C的充电器不兼容，供电翻车）*

**VBUS与GND**
`VBUS 为总线电源`，GND 做到共地即可。

>**以上内容如有侵权，请联系我进行删除~**
>**上文内容的深度有限，各位看完如果有不同的理解都可以进行留言或者私信哦~ 欢迎各位前来更正！！**


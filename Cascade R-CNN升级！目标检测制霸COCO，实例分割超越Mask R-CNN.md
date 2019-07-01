## Cascade R-CNN升级！目标检测制霸COCO，实例分割超越Mask R-CNN

原创： CV君 [我爱计算机视觉](javascript:void(0);) *4天前*

点击我爱计算机视觉标星，更快获取CVML新技术

------



前天，arxiv上新出一篇论文《Cascade R-CNN: High Quality Object Detection and Instance Segmentation》，目标检测算法Cascade R-CNN 原作者对其进行扩展应用于实例分割。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZEgmXogefbaUB2mh0XF93h4RQiaFMRVoN3VgAy5R6jg4zfWnZPN4kMTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



两位作者均来自加州大学圣地亚哥分校，这可能是一篇投向TPAMI的论文。



在目标检测的实验中，借助于骨干网ResNeXt-152 的加持，在COCO数据集上AP达到50.9，超越之前的SOTA TridentNet算法的48.4，在实例分割实验中精度也显著高于Mask R-CNN。



**Cascade R-CNN 是什么？**



Cascade R-CNN 是2017年末出的目标检测算法，它的核心思想是：



**使用不同的IOU阈值，训练多个级联的检测器。**



它可以用于级联已有的检测器，取得更加精确的目标检测。



描述Cascade R-CNN算法的论文被CVPR 2018录用，该文一出，曾经相当长时间位于COCO检测数据集精度榜首（后被图森的TridentNet超越，这次又超了回来。。。）。



谷歌学术显示：

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

原论文已有111次引用。



作者开源了代码，该算法曾经被COCO 2018 instance segmentation challenge , OpenImage 2018 challenge , Wider Challenge 2018 的获胜团队使用（作者文中声称）。



**Cascade R-CNN 网络结构**



网络结构如下图b，



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZWfFlMaSFdcZHqROLX74YbssoqUAMiagBz2w3NRufjETCPWEXrSswyDw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



公式表示就是这样的：

![f^{'}(x,b)=f_T\cdot f_{T-1}\cdot f_{T-2}\cdot \cdot \cdot \cdot f_1(x,b)](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZBcQoeG2TxpclZEzFSu1z6IHCr1UNR2mibJtBw3fSgsHNlibtIVRia39Jw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上图中b和c很像，iterative bbox at inference 是在推断时候对回归框进行后处理，即生成了之后在多次处理，而Cascade R-CNN 在训练的时候就进行重新采样，不同的stage的输入数据分布已经是不同的了。



**Cascade Mask R-CNN 网络结构**



受Mask R-CNN发，作者将Cascade R-CNN推广到实例分割很简单，作者提供了三种策略，分别对应下图中b、c、d中将分割头S放在不同的位置。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZpaTxFQ8tAmLFxOicNWn6uA9l1Ru9FqKdq3hneGldfs2IUgw757maZfw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**实验结果**



作者在通用目标检测、实例分割数据集COCO上进行了实验，换上骨干网ResNeXt-152的Cascade R-CNN 又刷出了新高度！AP 达到50.9。如下图：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZEZDk5XAGvKqFh7KopiaHIO2rDeCh7xgzibjKKkQarglbJ8iauvjad7ibkw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



使用不同的检测器和骨干网，级联后都能获得大幅度的精度提升，如下图：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZSN4SbwYZvJe9ibCicTE09bPTib0icx9t0qHXkkepUKbhaTvFlX8g3IQnKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



同时可见，级联后推断速度有少许变慢，但在可接受的范围内。



下图是将Cascade Mask R-CNN与基线版本比较的结果，在实例分割任务中，也取得了明显的精度提升。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZWOf4bVCHxd7UNNn2iaiae5xuetRpPgfkhEyY53wwqMo4DksulcQ06iaTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



在PASCAL VOC 2007 数据集上同样表现出，使用级联后均获得精度提升。

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZ6be6mFq6icFzf8l7pibniaXj1G6sxQhxl0VaicrQVKnlDtWMWukqo7aAAw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



作者又在一些特殊目标类、不同数据集上做了实验。



下图为在KITTI车辆类的检测结果：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZTKdZ5FRHyia6iaew1KZQUIFvhF0n0rhasiawhYbmVk6rOpib7jcFgaAia1Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



在CityPersons验证集分割的结果，和在WIderFace数据集上人脸检测结果：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTuw8m70aXOicCtknia5eJC7NZ0Bf1nl8Z0bmguzDibqvI7PSleG8TzviaicJ0AAuPiaGugfXh4Tlof4BSgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



均表明该算法能一致性的提高精度。



论文地址：

https://arxiv.org/abs/1906.09756v1

代码地址：

https://github.com/zhaoweicai/cascade-rcnn（Caffe版）

代码地址：

https://github.com/zhaoweicai/Detectron-Cascade-RCNN（Detectron版）



------







**目标检测专业交流群**



关注最新最前沿的目标检测技术，欢迎加入52CV-目标检测专业交流群，扫码添加CV君拉你入群（如已为CV君好友，请直接私信，**不必重复添加**），

**（请务必注明:目标检测）：**

**![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTs1Ke4WXicIqN7QibMXL527MCvicgajlnePVw1mnomoLqFqL0WLf7UUpSkVGj2E1GGe83e8ZmY0G42jw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**

喜欢在QQ交流的童鞋可以加52CV官方QQ群：702781905。

（不会时时在线，如果没能及时通过还请见谅）



------

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTvVOnJBvePcP1qFUSWpyvrjpYAWNIZTZzUA7Zq4VPlReicJWcIeozxic5VhHlwNQNAFXmKQBtKf5xAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

长按关注我爱计算机视觉

文章已于2019-06-28修改







![img](https://mp.weixin.qq.com/mp/qrcode?scene=10000004&size=102&__biz=MzIwMTE1NjQxMQ==&mid=2247487486&idx=1&sn=dcaca7b7b1122a4aeac0efe8bf9571c4&send_time=)

微信扫一扫
关注该公众号
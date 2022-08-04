# Deeplab X

## Deeplab v1

主要解决两个问题

1. 重复池化和下采样导致位置信息丢失严重（深层模型容易丢失浅层模型）

2. 空间不变性导致细节信息丢失

解决方案

1. 膨胀卷积
2. 全连接条件随机场（Fully-connected Conditional Random Field）

![img](https://upload-images.jianshu.io/upload_images/4688102-0dde7c653b4526ac.gif?imageMogr2/auto-orient/strip|imageView2/2/w/395/format/webp)

图 膨胀卷积示意图

一般利用到条件随机场（CRFs）来处理分割中不光滑问题，它只考虑到目标像素点的附近点，是一个短距离的CRFs。由于想修补一些细节结构，所以通道了全连接CRF模型。

## Deeplab v2

相比v1想解决的问题

1. 图像中存在多尺度目标

方案

1. 使用不同膨胀因子的空洞卷积融合多尺度信息—atrous spatial pyramid pooling(ASPP)

ASPP一种类似X Inception中多尺度卷积计算的结构，只不过这苦使用的是膨胀卷积的不同膨胀系数来实现多尺度信息融合的。

## Deeplab v3

改动：

1. 相比v2去掉了全连接条件随机场
2. 并行ASPP变成串行的多膨胀卷积结构

不懂，感觉奇怪。感觉在刷榜。

## Deeplab v3+

改动：

1. v3中串联的膨胀卷积恢复到并行去（ASPP）
2. 使用了空间金字塔池化（spatial pyramid pooling module，SPP）
3. 使用了深度可分离卷积轻量化模型

## 总结

最后这个版本就没有提全连接条件随机场处理边界信息了，可能就是因为一开始没有考虑到目标边界分割不明确是因为网络深度（编码网络最后视野过大，语义和细节无法兼顾）。所以后面逐渐使用SPP和ASPP解决了多尺度问题，边界不精细问题也就被解决了大部分，这个条件下CRFs作用就不大了，也就消失不再使用了。
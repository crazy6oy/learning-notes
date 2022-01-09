# Data imbalance

目前我已知的处理数据不均衡的大方向：

1. over sampling
2. under sampling
3. hard mining
4. coat sensitive learning
5. one-class classification

## Addressing class imbalance in deep learning for small lesion detection on medical images [2020]

这篇文章有点综述的感觉，简述了很多种方法。

文中主要还是解决分割任务中目标和背景数量差太多时的实验探究，使用了下列方法：

1. over sampling of the lesion class by replication
2. over sampling of the lesion class by data augmentation
3. under sampling of the background class
4. combined oversampling and under sampling
5. hard mining (2-phase training) on the background class
6. one-class learning on the background class
7. cost-sensitive learning (weighted cross entropy loss)

结论是使用over sampling+对背景使用hard mining效果最好。网络说的DC-CNN效果最好。

### 结论

开头写了非常丰富的东西，数据不均衡的方法应该覆盖了绝大多数。后续对本研究使用的内容做法进行介绍，介绍多种比较的设计。

*PS：但做法细节设计还没有看，后续补充*

## Balanced-MixUp for Highly Imbalanced Medical Image Classification

https://github.com/agaldran/balanced_mixup


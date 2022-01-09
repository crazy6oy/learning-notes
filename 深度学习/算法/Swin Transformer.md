Swin Transformer: Hierarchical Vision Transformer using Shifted Windows

Swin全称Shifted window

这里主要是提出一种主干网络，这种主干网络基于transformer的。

![image-20211209160424839](C:\Users\wangyx\AppData\Roaming\Typora\typora-user-images\image-20211209160424839.png)

其中灰色的方块文中叫做patch但是后面说到了patch在transformer/NLP中可以被理解为token，这里我从NLP中理解为一个句子中的词。红色的大框即表示一个“句子”也就是每个transformer结构独立计算各个红框中的内容，但是后续因为shifted的原因可以达到全图融合而非只能理解局部信息。

![image-20211209161726751](C:\Users\wangyx\AppData\Roaming\Typora\typora-user-images\image-20211209161726751.png)
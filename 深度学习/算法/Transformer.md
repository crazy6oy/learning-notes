Attention is All You Need

seq2seq with self-attention ≈ transformer

RNN hard to parallel. 因为RNN在算当前时刻内容的时候需要上一时刻的状态，所以不能并行。

大结构

transformer有两个大结构：Encoder和Decoder；Encoder和Decoder中是由6个block组成。

输入：词Embedding和位置Embedding融合（相加）组成输入矩阵中的一个向量，在N方向拼接这些向量组合成输入transformer的矩阵输入；

![img](https://pic3.zhimg.com/80/v2-45db05405cb96248aff98ee07a565baa_720w.jpg)

编码：将输入信息输入到Encoder中得到编码结果，编码结果的形状和输入信息形状完全一致。其中Encoder block输出和输入形状完全一致；

![img](https://pic2.zhimg.com/80/v2-5367bd47a2319397317562c0da77e455_720w.jpg)

解码：输入两个内容：①编码网络的结果；②输出结果掩码。其中结果掩码中一开始只有句子\<begin>标志其他内容都是掩码，随着每个位置的预测结果出现掩码内容逐渐补充为预测结果。



细节内容

![preview](https://pic4.zhimg.com/v2-b0a11f97ab22f5d9ebc396bc50fa9c3f_r.jpg)

词Embedding：可以从word2vec、Glove算法预训练得到，也可以在transformer中训练得到。

位置Embedding：用PE表示，因为这里都是用全连接，也没有进行词与词之间的信息传递，那么是需要保留位置信息的，这个可以根据及算法公式得到也可以通过训练得到。论文中使用的公式是:
$$
PE_{(pos,2i)}=sin(pos/10000^{2i/d})
$$

$$
PE_{(pos,2i+1)}=cos(pos/10000^{2i/d})
$$

其中pos表示单子在句子中的位置，d表示PE的维度，2i表示偶数维度，2i+1表示奇数维度。

使用公式的优势：

能够适用比训练集最长句子更长的句子；

可以让模型容易的算出相对位置。**不懂**

![img](https://pic4.zhimg.com/80/v2-f6380627207ff4d1e72addfafeaff0bb_720w.jpg)

上图左侧为encoder block右侧为decoder block

![img](https://pic2.zhimg.com/80/v2-6444601b4c41d99e70569b0ea388c3bd_720w.jpg)

self-attention

**注意：self-attention中涉及到的矩阵运算都是矩阵乘法而不是点乘。**

在transformer中自注意力需要用到Q（查询）、K（键值）、V（值）。

![preview](https://pic3.zhimg.com/v2-4f4958704952dcf2c4b652a1cd38f32e_r.jpg)

Q、K、V是根据词向量组成的矩阵/上一级输出的特征矩阵通过矩阵乘法算的的结果。

得到Q、K、V后根据下面的公式就可以算到自注意力的结果了。
$$
Attention(Q,k,V)=softmax(\frac{QK^T}{\sqrt{d_k}})V
$$
其中为了防止Q、K内积太大，所以除以根号dk，Q乘K的转置得到的矩阵即可认为是词与词的attention强度（举例：第n行每个元素以此表示第n个词和1~n个词的attention强度）；这里softmax是在第二个维度做归一化。

multi-head attention

![img](https://pic2.zhimg.com/80/v2-b0ea8f5b639786f98330f70405e94a75_720w.jpg)

从上图就很清楚可以看到多头注意力就是多个并行的自注意力构成的。

但这里的Q、K、V是不是共享的有待确定。

图中concat是把结果在二维进行拼接。

通过线性层的时候也是矩阵乘法，不是点乘。

Add & Norm

回到encoder block，Add很简单就是类似于残差连接，Norm这是指layer norm

Feed Forward

简单的全连接计算，linear+ReLU+linear
$$
result_{feed forward}=max(0,XW_1+b_1)W_2+b_2
$$
到此为止，Encoder block就可以根据上述内容组成
$$
result_{encoer block}=LayerNorm(LayerNorm(x+result_{multi-head attention})+result_{feed forward})
$$
Decoder使用的模块也是这些，只不过组合和数据使用上有一些差别：

1. 包含两个 Multi-Head Attention 层；
2. 第一个 Multi-Head Attention 层采用了 Masked 操作；
3. 第二个 Multi-Head Attention 层的K, V矩阵使用 Encoder 的编码信息矩阵进行计算，而Q使用上一个 Decoder block 的输出计算；
4. 最后有一个 Softmax 层计算下一个翻译单词的概率。

Teacher Forcing？？？

因为transformer是一个词一个词的预测，所以在训练过程中，Decoder block中Masked Multi-Head Attention中self-attention中的softmax之前需要一个mask操作，即QK^T与mask矩阵点乘，如下图

![preview](https://pic2.zhimg.com/v2-35d1c8eae955f6f4b6b3605f7ef00ee1_r.jpg)

![preview](https://pic1.zhimg.com/v2-b26299d383aee0dd42b163e8bda74fc8_r.jpg)

如上图，第n个词能参考/关联的词的强度不能超过第n个词的位置。

第二个Multi-Head Attention和Encoder block里面的就一样了，只是KV是Encoder输出的结果，Q是上一个Decoder block的输出。

最后，Decoder最后的softmax层输出的最终结果，第一维度以此是每一个位置的预测结果。

遗留问题：

1. 为什么说transformer有并行计算能力？
2. 多头自注意力，没有说有什么操作加大组间距，那会不会有些时候多头自注意力就是一个自注意力的多次复制？
3. Teacher Forcing是啥？
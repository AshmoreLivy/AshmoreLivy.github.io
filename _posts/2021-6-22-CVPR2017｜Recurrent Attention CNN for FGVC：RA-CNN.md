# Abstract

识别纹理细密的物体类别，判别区域定位（discriminative region localization）和细粒度特征学习（fine-grained feature learning）是很具有挑战性的。区域检测（region detection）和细粒度特征学习（fine-grained feature learning）之间的相互关联性，可以互相强化，循环注意力卷积神经网络（recurrent attention convolutional neural network——**RA-CNN**），用互相强化的方式对判别区域注意力（discriminative region attention）和基于区域的特征表征（region-based feature representation）进行递归学习。在每一尺度规模（scale）上进行的学习都包含一个**分类子网络**（classification sub-network）和一个**注意力建议子网络**（attention proposal sub-network——**APN**）。APN 从完整图像开始，通过把先期预测作为参考，由**粗到细迭代**地生成**区域注意力**（region attention），同时**精调器尺度网络**（finer scale network）以循环的方式从先前的尺度规格输入一个放大的注意区域（amplified attended region）。

RA-CNN 通过**尺度内分类损失**（intra-scale classification loss）和**尺度间排序损失**（inter-scale ranking loss）进行优化，以相互学习精准的区域注意力（region attention）和细粒度表征（fine-grained representation）。RA-CNN 并不需要边界框（bounding box）或边界部分的标注（part annotations），而且可以进行端到端的训练。

# Introduction

提出的RA-CNN是一个**叠加网络**（stacked network），其输入为从**全图像到多尺度的细粒度局部区域**（fine-grained local regions）。在网络结构设计上主要包含**3个scale子网络**，每个scale子网络的网络结构都是一样的，只是网络参数不一样，在每个scale子网络中包含两种类型的网络：**分类网络和APN网络**。因此数据流是这样的：输入图像通过**分类网络提取特征**并进行分类，然后APN网络基于提取到的特征进行训练得到attention region信息，再将attention region**剪裁**（crop）出来并**放大**（zoom in），再作为第二个scale网络的输入，这样重复进行3次就能得到3个scale网络的输出结果，通过**融合不同scale**网络的结果能达到更好的效果。

# Related Work

研究沿着两个维度进行：

1. Discriminative Feature Learning：依赖于强大的卷积深层特征
2. Sophisticated Part Localization：使用**无监督**的方法来**挖掘注意力区域**，放大判别性的局部区域,以提高细粒度识别性能

输入图像从上到下按粗糙的完整大小的图像到精炼后的区域注意力图像排列。不同的网络分类模块（蓝色部分）通过同一尺度的标注预测 Y(s) 和真实 Y∗之间的分类损失 Lcl 进行优化，注意力建议（红色部分）通过相邻尺度的 p (s) t 和 p (s+1) t 之间的成对排序损失 Lrank（pairwise ranking loss Lrank）进行优化。其中 p (s) t 和 p (s+1) t 表示预测在正确类别的概率，s 代表尺度。APN 是注意力建议网络，fc 代表全连接层，softmax 层通过 fc 层与类别条目（category entry）匹配，然后进行 softmax 操作。+代表「剪裁（crop）」和「放大（zoom in）」

**每个scale网络是有两个输出的multi-task结构:****

1. 分类p(X) = f(Wc* X)，
   1. **Wc**： (b1)或(b2)或(b3)网络的参数,也就是一些卷积层、池化层和激活层的集合，用来从输入图像中提取特征。
   2. **Wc*X**：就是最后提取到的特征。
   3. **f()函数**：就是fully-connected层和softmax层,用来将学习到的特征映射成类别概率,也就是p(X)。
2. 区域检测[tx, ty, tl] = g(Wc* X)
   1. 这里假设检测出来的区域都是正方形，即tx和ty表示区域的中心点坐标，tl表示正方形区域边长的一半。
   2. g()函数: 也就是APN网络，可以用两个fully-connected层实现，其中最后一个fully-connected层的输出channel是3,分别对应tx、ty、tl。

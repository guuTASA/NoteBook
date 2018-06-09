# AlexNet

### 1-两篇论文：

1 - Imagenet Classification with Deep Convolutional Neural Networks（AlexNet的产生）

2 - DISC: Deep Image Saliency Computing via Progressive Representation Learning

### 2-AlexNet的网络结构

该神经网络有6000万个参数和650,000个神经元，由五个卷积层，以及某些卷积层后跟着的max-pooling层，和三个全连接层，还有排在最后的1000-way的softmax层组成。

为了使训练速度更快，我们使用了非饱和的神经元和一个非常高效的GPU关于卷积运算的工具。为了减少全连接层的过拟合，我们采用了最新开发的正则化方法，称为“dropout”，它已被证明是非常有效的。

（来自论文）



#### 1)数据扩充

平移变换和反射变换（2048倍的训练，10倍的预测），彩色变换（改变训练图像中RGB通道的强度）

#### 2)ReLU激活函数

提高了模型训练的收敛速度

#### 3)多GPU (2 x GTX580)

Alexnet最初的模型分为上下两层。

#### 4)局部标准化 (Local Response Normalization)

该方法的引入在CIFAR10数据集上降低了约2%的测试误差，不过该方法在之后并不常用。

#### 5)重叠池化 (Overlapping Pooling)

重叠池化比不重叠池化更不易出现过拟合。

#### 6)Dropout

避免过拟合。



### 3-局部响应归一化（？）

亮度归一化
http://code.google.com/p/cuda-convnet/



### Q&A

AlexNet其实已经过拟合了，为什么后面的网络对于相同的问题和数据集，网络却越来越深，参数越来越多？
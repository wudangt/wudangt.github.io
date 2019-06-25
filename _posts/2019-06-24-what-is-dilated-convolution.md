---
title: WHAT IS DILATED CONVOLUTION?
date: 2019-06-24 15:27:31
---

## why is dilated Convolution
引入空洞卷积不得不提到感受野，感受野是卷积神经网络的每一层输出的特征图上像素在原图像上映射的区域的大小。空洞卷积主要是为了解决图像分割问题而提出来的。在FCN中通过pooling增大感受野缩小图像尺寸，然后通过upsampling还原图像尺寸，但是这个过程中造成了精度的损失，那么为了减小这种损失理所当然想到的是去掉pooling层，然而这样就导致特征图感受野太小，因此空洞卷积应运而生。

## what is dilated Convolution


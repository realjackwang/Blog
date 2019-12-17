# ASC准备工作


## ASC Student Supercomputer Challenge (2019)Preliminary Contest Notifications

<!-- more -->

## 要求

一、学校或院系超级计算机活动简介（5分）

二、团队介绍（5分）

三、技术要求（90分）
1. **设计一个HPC系统（15分）**

2. **HPL和HPCG（15分）**

3. **单图像超分辨率挑战（30分**）

   **简介**

   在本次比赛中，参赛者需要设计一种算法，使用深度学习等SOTA策略对使用双三次核下采样的图像进行4x SR上标。例如。4倍放大后的400x600图像的分辨率为1600x2400。评估将以感知质量感知的方式进行。pim2018[^4]中定义的感知指数(perception index, PI)将用于计算重建的高分辨率图像的质量。PI越低，重建图像的质量越高。[Ma](#Ma)和[NIQE](#NIQE)是两种无参考图像质量度量[^5-6]。
   $$
   Perceptual \ index = \frac { 1 } { 2 } ( ( 10 - M a ) + N I Q E )
   $$
   每队提交80张重建的高分辨率图像进行评分测试。

   对于初始和最终阶段，每个团队还应该提交一个文件夹，其中包含可以重现测试结果的源代码和模型。

    Folder Name              | Contents                             
    ------------------------ | --------------------------------- 
    Single Image Super Resolution Challenge | Root directory  
    HR_images        | reconstructed high-resolution images 
    script            | PyTorch source code here             
    model                     | PyTorch model here                   

   **评判标准**

   score = S~PI~ + S~prop~
   $$
   S _ { P I } = \frac { 20 } { \left( P I / P I _ { \min } \right) ^ { 4 } }
   $$

   1. 参与者应该注意RMSE（Root-Mean-Square Error）应该位于8 <RMSE < 18的范围内，否则S~PI~为0

   2. S~prop~是委员会根据参与者的提案给出的分数。最大S~prop~为10。鼓励参与者详细描述他们的神经网络设计和网络性能。

   **The participants must use [PyTorch](https://pytorch.org/) framework for this task**

4. **CESM测试（30分）**

   **简介**

   在气候建模社区广泛使用的模型中，社区地球系统模型(community Earth System Model, CESM)已成为世界上最流行的气候模型之一，广泛应用于气候变化、气候预测和气候变化的各种研究。

   CESM是一个完全耦合的、社区的、全球气候模型，它提供了最先进的计算机模拟地球的过去。礼物。以及未来的气候状况。CESM是全球大约12个气候模型之一，可以用来模拟地球气候系统的许多组成部分，包括海洋、大气。海冰和陆地覆盖。使用CESM。研究人员现在可以模拟海洋生态系统与温室气体的相互作用:臭氧对气候的影响。尘埃和其他大气化学物质:碳在大气、海洋中的循环。陆地表面:以及温室气体对上层大气的影响。

   CESM各组成部分的数学原理和算法在参考文献[^1]和[^2]中有详细描述。我们建议使用稳定版本的cesm1.2.2，其源代码可在参考[^3]中获得。关于CESM的安装和使用的更多信息可以从参考[^4]中找到。

   **评判标准**

   评价我们可以使用CESM和RMSE的诊断包来评估模型结果

   CESM的诊断包可以获得每个变量的气候学和年变异性。

   我们用500hPa位势高度作为代表。然后计算模型结果与观测数据之间的RMSE
   $$
   \mathrm { RMSE } = \sqrt { \frac { \sum _ { t = 1 } ^ { n } \left( x _ { 1 , t } - x _ { 2 , t } \right) ^ { 2 } } { n } }
   $$

****

## 论文翻译

## T3[2] Perceptual Losses for Real-Time Style Transfer and Super-Resolution

> 参考：
>
> [基于感知损失函数的实时风格转换和超分辨率重建 (zhwhong)](https://www.jianshu.com/p/b728752a70e9)
>
> [深度学习可以做哪些有趣的事情？](https://www.jianshu.com/p/fe0c149ea806)

### 简介

**摘要：**我们考虑的图像转换的问题，即将一个输入图像变换成一个输出图像。最近热门的图像转换的方法通常是训练前馈卷积神经网络，将输出图像与原本图像的逐像素差距作为损失函数。并行的工作表明，高质量的图像可以通过用预训练好的网络提取高级特征、定义并优化感知损失函数来产生。我们组合了一下这两种方法各自的优势，提出采用感知损失函数训练前馈网络进行图像转换的任务。本文给出了图像风格化的结果，训练一个前馈网络去解决实时优化问题（Gatys等人提出的），和基于有优化的方法对比，我们的网络产生质量相当的结果，却能做到三个数量级的提速。我们还实验了单图的超分辨率重建，同样采用感知损失函数来代替求逐像素差距的损失函数

### 算法

**我们的系统由两部分组成：**

一个**图片转换网络f~w~** 

一个**损失网络 φ**（用来定义一系列损失函数l1, l2, l3）

图片转换网络是一个**深度残差网络**

参数是权重W，它把输入的图片x通过映射 y=f~w~(x)转换成输出图片y，每一个损失函数计算一个标量值li(y,y~i~), 衡量输出的y和目标图像y~i~之间的差距。

图片转换网络是用**SGD**训练，使得一系列损失函数的加权和保持下降

![图2：系统概览。左侧是Generator，右侧是预训练好的vgg16网络(一直固定)](https://upload-images.jianshu.io/upload_images/145616-16427bfcaab71f02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

损失网络对比生成网络生成的图片与每一幅训练集中的目标图片

损失函数可表示为：

![img](https://upload-images.jianshu.io/upload_images/145616-ca7f24d7d863f854.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

**网络设计：**

1. 我们不用任何的池化层，取而代之的是用**步幅卷积**或微步幅卷积

2. 我们的神经网络有**五个残差块**[42]组成，用了[44]说的结构。

   ![img](https://upload-images.jianshu.io/upload_images/5300364-f8e8898da3e5c066.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/538/format/webp)

3. 所有的非残差卷积层都跟着一个空间性的batch-normalization和RELU的非线性层，最末的输出层除外。

4. 最末层使用一个缩放的Tanh来确保输出图像的像素在[0,255]之间。
5. 第一个和最后一个卷积层使用9×9的核，其他卷积层使用3×3的核。

**输入和输出：**对于风格转换，输入和输出都是彩色图片，大小3x256x256。对于超分辨率重建，有一个上采样因子f，输出是一个高分辨率的图像3x288x288，输入是一个低分辨率图像 3x288/fx288/f，因为图像转换网络是完全卷积，所以在测试过程中它可以被应用到任何分辨率的图像中。

**下采样和上采样：**对于超分辨率重建，有一个上采样因子f，我们用了几个残差块跟着Log2f卷及网络（stride=1/2）。这个处理和[1]中不一样，[1]在把输入放进网络之前使用了双立方插值去上采样这个低分辨率输入。不依赖于任何一个固定的上采样插值函数，微步长卷积允许上采样函数和网络的其他部分一起被训练。

**单图超分辨率重建：**

在单图超分辨率重建中，任务是从一个低分辨率的输入，去产生一个高分辨率的输出图片。这是一个固有的病态问题，因为对一个低分辨率图像，有可能对应着很多种高分辨率的图像。当超分辨率因子变大时，这个不确定性会变得更大。对于更大的因子（x4 x8），高分辨率图像中的好的细节很可能只有一丁点或者根本没有出现在它的低分辨率版本中。

为了解决这个问题，我们训练了超分辨率重建网络，不使用过去使用的逐像素差损失函数，取而代之的是一个**特征重建损失函数**（看section 3）以保证语义信息可以从预训练好的损失网络中转移到超分辨率网络。我们重点关注x4和x8的超分辨率重建，因为更大的因子需要更多的语义信息。

传统的指标来衡量超分辨率的是PSNR和SSIM，两者都和人类的视觉质量没什么相关的[55,56,57,58,59].PSNR和SSIM仅仅依赖于像素间低层次的差别，并在高斯噪声的相乘下作用，这可能是无效的超分辨率。另外的，PSNR是相当于逐像素差的，所以用PSNR衡量的模型训练过程是让逐像素损失最小化。因此我们强调，这些实验的目标并不是实现先进的PSNR和SSIM结果，而是展示定性的质量差别（逐像素损失函数vs感知损失）

**模型细节：**我们训练模型来完成x4和x8的超分辨率重建，通过最小化特征损失（用vgg16在relu2_2层提取出），用了288x288的小块（1万张MSCOCO训练集），准备了低分辨率的输入，用高斯核模糊的（σ=1.0）下采样用了双立方插值。我们训练时bacth-size=4，训练了20万次，Adam，学习速率0.001，无权重衰减，无dropout。作为一个后续处理步骤，我们执行网络输出和低分辨率输入的直方图匹配。

**基础：**基本模型我们用的 SRCNN[1] 为了它优秀的表现，SRCNN是一个三层的卷积网络，损失函数是逐像素求差，用的ILSVRC2013数据集中的33x33的图片。SRCNN没有训练到x8倍，所以我们只能评估x4时的差异。

SRCNN训练了超过1亿次迭代，这在我们的模型上是不可能实现的。考虑到二者的差异（SRCNN和我们的模型），在数据，训练，结构上的差异。我们训练图片转换网络x4,x8用了逐像素求差的损失函数，这些网络使用相同搞得数据，结构，训练网络去减少lfeat

**评测：**我们评测了模型，在标准的集合5，集合6，BSD100数据集，我们报告的PSNR和SSIM[54]，都只计算了在Y通道上的（当转换成YCbCr颜色空间后），跟[1,39]一样。

### 结论

在这篇文章中，我们结合了前馈网络和基于优化的方法的好处，通过用感知损失函数来训练前馈网络。我们对风格转换应用了这个方法达到了很好的表现和速度。对超分辨率重建运用了这个方法，证明了用感知损失来训练，能带来更多好的细节和边缘。

未来的工作中，我们期望把感知损失函数用在更多其他的图像转换任务中，如上色或者语义检测。我们还打算研究不同损失网络用于不同的任务，或者更多种不同的语义信息的数据集

****

## T3[3] Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network

### 网络结构图（SRGAN）

![img](https://upload-images.jianshu.io/upload_images/8771353-415f53eb0c6ee4ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/539/format/webp)

### 算法

ISR是高分辨率图像，ILR是低分辨率图像，是由高分辨率图像先加高斯噪声然后经过一个r步长的下采样得到的，所以高低分辨率的图像大小分别是：rW x rH x C和W x H x C。

模型的目的就是生成网络G可以将输入的低分辨率图像重构出ISR，所以在生成器时就采用前馈CNN记作Gθ，参数是θ，那么此部分要优化的就是：

![img](https://upload-images.jianshu.io/upload_images/8771353-0b599f50ab2dc551.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/305/format/webp)

和标准GAN一样，模型需要训练下式：

![img](https://upload-images.jianshu.io/upload_images/8771353-1ac16d9f1b365ae2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

生成网络的结构如下，核心是下面的B残差块部分，重复的残差块用于生成高分辨率图像，然后接着两个亚像素卷积层用于恢复高分辨率图像的尺寸。

![img](https://upload-images.jianshu.io/upload_images/8771353-0bdab0f240cde072.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/539/format/webp)

判别网络的结构如下图，连续的卷积层、BN和Leaky ReLU，紧接着是稠密块和SIGMOD用于对图像分类。

![img](https://upload-images.jianshu.io/upload_images/8771353-1e0f0c73c5d4094c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/539/format/webp)

SRGAN的创新点在于 loss函数分为两部分

![img](https://upload-images.jianshu.io/upload_images/8771353-1d6768bef7c5c714.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/299/format/webp)

* 内容损失
* 对抗损失

**内容损失**

传统的loss都是以像素为单位的MSE，MSE的loss函数使得输出缺乏高频成分，过于光滑不适宜人们阅读。

所以本文在基于预训练的VGG19的RELU激活层来定义loss函数：

![img](https://upload-images.jianshu.io/upload_images/8771353-7cb65e4ef1ac0b7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/352/format/webp)

**对抗损失**

生成图像被判别为高分辨率图像的概率：

![img](https://upload-images.jianshu.io/upload_images/8771353-47f6835b6a2876e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/220/format/webp)

### 结论

* 用均方误差优化SRResNet，能够得到具有很高的峰值信噪比的结果。

* 在训练好的VGG模型的高层特征上计算感知损失来优化SRGAN，并结合SRGAN的判别网络，能够得到峰值信噪比虽然不是最高，但是具有逼真视觉效果的结果，基于VGG模型高层特征比基于VGG模型低层特征的内容损失能生成更好的纹理细节。

****

## T3[4] THE PIRM CHALLENGE ON PERCEPTUAL SUPER RESOLUTION

将感知失真平面划分为RMSE上阈值定义的三个区域。在每个区域中，获胜的算法都是感知质量最好的算法。

![img](https://www.pirm2018.org/img/regions.svg)

### 方法

![x](https://i.loli.net/2019/01/13/5c3b1060ce9c0.png)

注意，感知指数越低，表示感知质量越好。如果两个提交之间的感知指数存在边际差异(最多相差0.01)，RMSE越低的排名越高。

**Regions** The three regions are defined by
Region 1: RMSE ≤ 11.5
Region 2: 11.5 < RMSE ≤ 12.5
Region 3: 12.5 < RMSE ≤ 16
We encourage participation in all three regions.

****

## T3[5] Making a ‘Completely Blind’ Image Quality Analyzer 

<div id="Ma"></div>
### 简介

Natural image quality evaluator, [**NIQE**](https://blog.csdn.net/mazhitong1020/article/details/80415758)——无需利用人眼评分的失真图像进行训练在计算其局部MSCN 归一化图像后, 根据局部活性选择部分图像块作为训练数据, 以广义高斯模型拟合得到模型参数作为特征, 采用多变量高斯模型描述这些特征, 评价过程中利用待评价图像特征模型参数与预先建立的模型参数之间的距离来确定图像质量

NIQE质评价模型
NIQE质量评价模型不需要原始图像的主现评价分数，其在原始图像库中提取图像特征，再利用多元高斯( multivariate Gaussian. MYG)模型进行建模、图像像素归一化，通过分离力一化方法计算一幅图像的归一化亮度。

### 算法

假设亮度图像为I（i.j）则其分离归一化计算公式如下:

![s](https://i.loli.net/2019/01/13/5c3b14a397b0b.png)

......

****

## T3[6] Learning a No-Reference Quality Metric for Single-Image Super-Resolution

<div id="NIQE"></div>
### 简介

文献中提出了大量的单图像超分辨率算法，但是很少有研究涉及到基于视觉感知的性能评估问题。而大多数超分辨率图像都是用full来计算的。参考指标，有效性不明确，所需的基本事实在实践中并不总是可用的。为了解决这些问题，我们使用大量的超分辨率图像进行人体受试者研究，并提出一个从视觉感知评分中学习的无参考指标。具体来说，我们设计了三种空间域和频域的低阶统计特征来量化超分辨率的伪影，并学习了一种两阶段回归模型，在不参考地面真值图像的情况下预测超分辨率图像的质量分数。大量实验结果表明，该方法能够有效地评估基于人类感知的超分辨率图像的质量。

### 算法

我们利用三种统计性质作为特征，包括局部和全局频率变化和空间不连续，以量化伪影和SR图像的质量。每一组统计特征在金字塔上计算，以减轻SR工件的尺度敏感性。图8中显示了该算法学习无参考质量度量的主要步骤。图9显示了每种类型的特性的统计属性的概述

### 局部频率特征 Local Frequency Features

通过对离散余弦变换(DCT)系数的统计，有效地量化了图像退化[35]的程度和类型，并用于自然图像质量评价[23]。由于SR图像是由LR输入产生的，因此该任务可以看作是对LR图像高频分量的恢复。为了量化SR恢复所引入的高频伪影，我们提出将SR图像转换为DCT域，用[23]中的广义高斯分布(GGD)拟合DCT系数。

​                                                                           $f ( x | \mu , \gamma ) = \frac { 1 } { 2 \Gamma \left( 1 + \gamma ^ { - 1 } \right) } e ^ { - \left( | x - \mu | ^ { \gamma } \right) }$





![这是文字](https://i.loli.net/2019/01/13/5c3b49ecd350b.png "图8:建议的无参考度量的主要步骤。对于每个输入的SR mage，利用空间域和频域的统计量作为特征来表示SR图像。将提取的每一组特征在单独的集成回归树中进行训练，利用线性回归模型从大量的视觉感知得分中学习预测质量分数。")

图8:建议的无参考度量的主要步骤。对于每个输入的SR mage，利用空间域和频域的统计量作为特征来表示SR图像。将提取的每一组特征在单独的集成回归树中进行训练，利用线性回归模型从大量的视觉感知得分中学习预测质量分数。



### 全局频率特征 Global Frequency Features

​                                                                            $p _ { Y } ( y ) = \int \frac { 1 } { ( 2 \pi ) ^ { N / 2 } \left| z ^ { 2 } Q \right| ^ { 1 / 2 } } e ^ { \left( - \frac { Y ^ { T } \Q ^ { - 1 } Y } { 2 z ^ { 2 } } \right) } p _ { z } ( z ) d z$

### 空间特征 Spatial Features

$\rho = \frac { 2 \sigma _ { x y } + c _ { 0 } } { \sigma _ { x } ^ { 2 } + \sigma _ { y } ^ { 2 } + c _ { 0 } }$



###　两阶段回归模型 Two-stage Regression Model



$\theta _ { j } ^ { n * } = \underset { \theta _ { j } ^ { n } \in \mathcal { T } _ { j } } { \operatorname { argmax } } I _ { j } ^ { n }$





$I _ { j } ^ { n } = \sum _ { x _ { n } \in \mathcal { S } _ { j } } \log \left( \left| \Lambda _ { y } \left( x _ { n } \right) \right| \right) - \sum _ { i \in \{ L , R \} } \left( \sum _ { x _ { n } \in S _ { j } ^ { i } } \log \left( \left| \Lambda _ { y } \left( x _ { n } \right) \right| \right) \right)$

因此，我们将这三种特征的输出线性回归到感知得分。并估计最终的质量分数为： $\hat { y } = \sum _ { n } \lambda _ { n } \cdot \hat { y } _ { n }$

其中权重$\lambda$是通过最小化学习来的

​                                                            $\lambda ^ { * } = \underset { \lambda } { \arg \min } \left( \sum _ { n } \lambda _ { n } \cdot \hat { y } _ { n } - y \right) ^ { 2 }$

****

## T4[1] Description of the NCAR Community Atmosphere Model (CAM 4.0)

> 224页我的天啦

......

****

## T4[2] CESM1.1 Coupler Flow Diagram Standard Default Configuration

......

****

## T4[3] CPL7 User’s Guide

......

****

## T4[4] CESM User’s Guide 
......


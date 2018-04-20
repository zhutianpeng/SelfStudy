**卷积神经网络CNN浅析**

**我建议你先把CNN当作一个黑盒子，不要关心为什么，只关心结果。**

这里借用了一个分辨X和o的例子来这里看原文，就是每次给你一张图，你需要判断它是否含有"X"或者"O"。并且假设必须两者选其一，不是"X"就是"O"。

![https://pic2.zhimg.com/50/v2-07b94113cd90697b17a3b149cff7f081_hd.png](media/e9aeda024fa26864dc4bdf30dcc9695d.png)

下面看一下CNN是怎么分辨输入的图像是x还是o，如果需要你来编程分辨图像是x还是o，你会怎么做？可能你第一时间就想到了逐个像素点对比。但是，如果图片稍微有点变化呢？像是下面这个x，它不是标准的x，我们可以分辨它是x，但是对于计算机而言，它就只是一个二维矩阵，逐个像素点对比肯定是不行的。

![https://pic1.zhimg.com/50/v2-cc555b2db7b53159d41da4f6c9988610_hd.png](media/90b23ebdbf00c65b25f3ff1c6b1092dd.png)

CNN就是用于解决这类问题的，它不在意具体每个点的像素，而是通过一种叫卷积的手段，去提取图片的特征。

**卷积层**

所以对于CNN而言，第一步就是提取特征，卷积就是提取猜测特征的神奇手段。而我们不需要指定特征，任凭它自己去猜测，就像上图，我们只需要告诉它，我们喜欢左边的，不喜欢右边的，然后它就去猜测，区分喜不喜欢的特征

![https://pic1.zhimg.com/50/v2-389c7fe4b460d0c025c8413abfe5512c_hd.png](media/a1ac84b614dc406e6531845fa50a6c56.png)

假设，我们上面这个例子，CNN对于X的猜测特征如上，现在要通过这些特征来分类。

计算机对于图像的认知是在矩阵上的，每一张图片有rgb二维矩阵（不考虑透明度）所以，一张图片，应该是3x高度x宽度的矩阵。而我们这个例子就只有黑白，所以可以简单标记1为白，-1为黑。是个9x9的二维矩阵。

![https://pic4.zhimg.com/50/v2-170b72d851cbf324cc09c0d1730efbaf_hd.png](media/b8f26eedd81abd917609595ff43da4a4.png)

我们把上面的三个特征作为卷积核(我们这里是假设已经训练好了CNN，训练提出的特征就是上面三个，我们可以通过这三个特征去分类
X )，去和输入的图像做卷积(特征的匹配)。

![https://pic1.zhimg.com/50/v2-841bed5832b6d4bae2e22da73e77f99c_hd.png](media/1b48a24975dc7c100742cedd37aea3d6.png)

![https://pic4.zhimg.com/50/v2-c2c9d34affb2e12de57fc5542fd7be03_hd.png](media/ee8f04b1145703337c871fae16935d5b.png)

![https://pic1.zhimg.com/50/v2-b6c5054d1be43bc293640f68f600cb14_hd.png](media/0d6d02d10fce19af9de46685f70b8046.png)

![https://pic3.zhimg.com/50/v2-1edb7c99f28212c728b5c04cd7a3d9f2_hd.png](media/4b4c3b8c5e37b0cecbadfa982e12de8a.png)

![https://pic4.zhimg.com/50/v2-e39bbc74ac528680a6a14a23cbe280df_hd.png](media/024171deb4354d978a3dd99d3aab7e89.png)

看完上面的，估计你也能看出特征是如何去匹配输入的，这就是一个卷积的过程，具体的卷积计算过程如下(只展示部分)：

![https://pic1.zhimg.com/50/v2-eae07bcfbb079d9785a9bcb20436cc30_hd.png](media/eb75edbbc1066f50e6284fef883e4181.png)

![https://pic2.zhimg.com/50/v2-180debd15dc45f4fd095b8ebabf820e1_hd.png](media/02697d7f54b1744f2efcf98bf2d18e2b.png)

![https://pic4.zhimg.com/50/v2-dbe766bfcc63774a9a59d0a0761a3e5f_hd.png](media/c0de93d6877405ae6dd17687328a5760.png)

![https://pic3.zhimg.com/50/v2-d744d1de3a4c9ec4214b52805ef70452_hd.png](media/3aa02330bbb802ca0a3d9e2287430bc7.png)

把计算出的结果填入新的矩阵

![https://pic4.zhimg.com/50/v2-ff5a99b11326ec90e42257479fb0a597_hd.png](media/85cb3428fdf2e247df808b3d7c13aa59.png)

**其他部分也是相同的计算**

![https://pic3.zhimg.com/50/v2-dd011652f998e6a621eae0cd991cacfe_hd.png](media/1781f42c3c4cda838716421afd002b91.png)

![https://pic4.zhimg.com/50/v2-f6d277f4d8dc9fcd2ef3b4549ae6da93_hd.png](media/b24897b793748f281b843b246213621f.png)

**最后，我们整张图用卷积核计算完成后：**

![https://pic4.zhimg.com/50/v2-b52238a119466319f83668010efbc5db_hd.png](media/45f1c9116f4ff18c2fd7c2c31c2882b3.png)

**三个特征都计算完成后：**

![https://pic2.zhimg.com/50/v2-57d763b9349b1dc945bb7083035cc361_hd.png](media/59445856f54058a6d0586a9423def7d9.png)

不断地重复着上述过程，将卷积核（特征）和图中每一块进行卷积操作。最后我们会得到一个新的二维数组。其中的值，越接近为1表示对应位置的匹配度高，越是接近-1，表示对应位置与特征的反面更匹配，而值接近0的表示对应位置没有什么关联。

**以上就是我们的卷积层，通过特征卷积，输出一个新的矩阵给下一层。**

**池化层**

在图像经过以上的卷积层后，得到了一个新的矩阵，而矩阵的大小，则取决于卷积核的大小，和边缘的填充方式，总之，在这个XXOO的例子中，我们得到了7x7的矩阵。池化就是缩减图像尺寸和像素关联性的操作，只保留我们感兴趣（对于分类有意义）的信息。

常用的就是2x2的最大池。

![https://pic1.zhimg.com/50/v2-1819a146f4df7a1a764a4fa99f7ee4e0_hd.png](media/597a34264413b6d95f57b9300faa9645.png)

![https://pic2.zhimg.com/50/v2-274c2a16a6b519c3d480828628bcdfb9_hd.png](media/aee0e9620d6dda49d590a9b090778619.png)

![https://pic2.zhimg.com/50/v2-0e6cad2ee49abcf6baf202b8a09ead45_hd.png](media/f36584e39dd9397c8512fe56cb320fbf.png)

![https://pic4.zhimg.com/50/v2-f7e608bfa6a8da46db8073950e4d32e3_hd.png](media/1de4ce595b9e5a00a9cd8260a1155ef4.png)

看完上面的图，你应该知道池化是什么操作了。

通常情况下，我们使用的都是2x2的最大池，就是在2x2的范围内，取最大值。因为最大池化（max-pooling）保留了每一个小块内的最大值，所以它相当于保留了这一块最佳的匹配结果（因为值越接近1表示匹配越好）。这也就意味着它不会具体关注窗口内到底是哪一个地方匹配了，而只关注是不是有某个地方匹配上了。

这也就能够看出，CNN能够发现图像中是否具有某种特征，而不用在意到底在哪里具有这种特征。这也就能够帮助解决之前提到的计算机逐一像素匹配的死板做法。

![https://pic2.zhimg.com/50/v2-f707d3d2a2bacec1e40822a9473a3779_hd.png](media/1013e48b187817327cf76752605b5e82.png)

同样的操作以后，我们就输出了3个4x4的矩阵。

**全连接层**

全连接层一般是为了展平数据，输出最终分类结果前的归一化。
我们把上面得到的4x4矩阵再卷积+池化，得到2x2的矩阵

![https://pic2.zhimg.com/50/v2-9c967ec12fa4a969245de0ae7118bab9_hd.png](media/f292170412180fca3ff7b1887d81f4ae.png)

全连接就是这样子，展开数据，形成1xn的'条'型矩阵。

![https://pic3.zhimg.com/50/v2-b833cf3280c2f12b6f50e6bf1ce48e3a_hd.png](media/5d43758f36b7987898d54e9f2c629e3d.png)

然后再把全连接层连接到输出层。之前我们就说过，这里的数值，越接近1表示关联度越大，然后我们根据这些关联度，分辨到底是O还是X.

![https://pic4.zhimg.com/50/v2-e691bc8d06106b63539b280bd134c063_hd.png](media/ecfdad6fb487ff1976f82cbe788c0d1d.png)

![https://pic3.zhimg.com/50/v2-6840898a5893c31f77c89bcd539b4922_hd.png](media/6736dfc76b9cfd2488928da6d044de37.png)

看上图（圈圈里面的几个关键信息点），这里有个新的图像丢进我们的CNN了，根据卷积\>池化\>卷积\>池化\>全连接的步骤，我们得到了新的全连接数据，然后去跟我们的标准比对，得出相似度，可以看到，相似度是X的为0.92
所以，我们认为这个输入是X。

一个基本的卷积神经网络就是这样子的。回顾一下，它的结构：

![https://pic1.zhimg.com/50/v2-a1722f82172c24581f79c38c46f8a08c_hd.png](media/7acddcbb3e41096717f274d07b4ae9bd.png)

Relu是常用的激活函数，所做的工作就是max(0,x)，就是输入大于零，原样输出，小于零输出零。

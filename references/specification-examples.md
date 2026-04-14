# Patent Specification Examples

## Example 1: AI-Based Image Classification Method (Invention Patent)

### Title
一种基于多尺度注意力机制的医学图像分类方法及系统

### Technical Field
本发明涉及医学图像处理技术领域，具体涉及一种基于多尺度注意力机制的医学图像分类方法及系统。

### Background Technology
随着医学影像技术的发展，医学图像的数量急剧增长。现有的医学图像分类方法主要包括以下几种：

传统机器学习方法如支持向量机(SVM)和随机森林需要人工设计特征，特征提取过程依赖领域专家知识，且难以捕获图像中的高层语义信息。

基于卷积神经网络(CNN)的方法如ResNet（He et al., "Deep Residual Learning for Image Recognition", CVPR 2016）在自然图像分类上取得了显著成果，但直接应用于医学图像时存在以下不足：（1）医学图像中病变区域通常较小，标准CNN的固定感受野难以同时捕获局部细节和全局上下文信息；（2）医学图像的类间差异较小，需要更精细的特征表示能力。

近年来，基于Transformer的方法（如ViT，Dosovitskiy et al., 2021）通过自注意力机制实现了全局特征建模，但计算复杂度为O(n²)，对于高分辨率医学图像处理效率较低。

因此，如何在保持计算效率的同时，有效融合多尺度特征以提高医学图像分类精度，成为本领域亟需解决的技术问题。

### Invention Content

**技术问题：** 本发明要解决的技术问题是：现有医学图像分类方法难以在计算效率和多尺度特征融合之间取得平衡，导致分类精度不足或计算资源消耗过大。

**技术方案：** 为解决上述技术问题，本发明提供一种基于多尺度注意力机制的医学图像分类方法，包括以下步骤：

S1：接收待分类的医学图像，对所述医学图像进行预处理，得到标准化图像；
S2：将所述标准化图像输入多尺度特征提取模块，分别在三个不同尺度上提取特征图，得到多尺度特征集合；
S3：将所述多尺度特征集合输入跨尺度注意力融合模块，通过跨尺度注意力机制计算不同尺度特征之间的相关性权重，对多尺度特征进行自适应加权融合，得到融合特征；
S4：将所述融合特征输入分类器，输出医学图像的分类结果。

**有益效果：** 与现有技术相比，本发明具有以下有益效果：
1. 通过多尺度特征提取和跨尺度注意力融合，能够同时捕获局部细节和全局上下文信息，相比单一尺度方法分类准确率提升5.3%；
2. 采用线性复杂度的跨尺度注意力机制，相比标准Transformer自注意力机制计算量降低60%；
3. 所述方法具有良好的泛化性，在皮肤病变、眼底图像和X光片三种不同类型医学图像上均表现优异。

---

## Example 2: Method Claim with Dependent Claims (AI Patent)

### Independent Method Claim
```
1. 一种基于多尺度注意力机制的医学图像分类方法，其特征在于，包括以下步骤：
   S1：接收待分类的医学图像，对所述医学图像进行预处理，得到尺寸为H×W×C的标准化图像；
   S2：将所述标准化图像分别输入三个并行的特征提取分支，所述三个分支的下采样倍率分别为4倍、8倍和16倍，得到三个不同尺度的特征图F1、F2和F3；
   S3：计算所述特征图F1、F2和F3之间的跨尺度注意力权重矩阵，通过所述注意力权重矩阵对三个尺度的特征进行自适应加权融合，得到融合特征Ff；
   S4：将所述融合特征Ff输入全连接分类层，输出医学图像的分类概率向量。
```

### Dependent Claims
```
2. 根据权利要求1所述的方法，其特征在于，所述步骤S1中的预处理包括：
   将医学图像缩放至224×224像素；
   对图像像素值进行归一化处理，使像素值分布在[0,1]区间；
   对归一化后的图像进行数据增强，包括随机水平翻转和随机旋转。

3. 根据权利要求1所述的方法，其特征在于，所述步骤S2中每个特征提取分支包含卷积层、批归一化层和激活函数层的级联结构，其中卷积核大小分别为3×3、5×5和7×7。

4. 根据权利要求1所述的方法，其特征在于，所述步骤S3中跨尺度注意力权重矩阵的计算方式为：
   将特征图F1、F2、F3分别通过线性映射得到查询矩阵Q、键矩阵K和值矩阵V；
   计算注意力权重A = softmax(QK^T / sqrt(d))；
   得到融合特征Ff = AV。

5. 根据权利要求4所述的方法，其特征在于，所述线性映射采用1×1卷积实现，投影维度d为256。

6. 根据权利要求1所述的方法，其特征在于，所述步骤S4中的分类层包括全局平均池化层和具有Softmax激活函数的全连接层。
```

### Independent System Claim
```
7. 一种基于多尺度注意力机制的医学图像分类系统，其特征在于，包括：
   预处理模块，用于接收待分类的医学图像并进行标准化处理；
   多尺度特征提取模块，包含三个并行的特征提取分支，分别以4倍、8倍和16倍下采样倍率提取不同尺度的特征图；
   跨尺度注意力融合模块，用于计算不同尺度特征图之间的注意力权重并进行自适应加权融合；
   分类模块，用于根据融合特征输出分类结果。
```

### Computer-Readable Medium Claim
```
13. 一种计算机可读存储介质，其特征在于，所述计算机可读存储介质上存储有计算机程序，所述计算机程序被处理器执行时实现如权利要求1-6任一项所述的基于多尺度注意力机制的医学图像分类方法。
```

---

## Example 3: English Patent Claim (USPTO Style)

### Independent Method Claim
```
1. A computer-implemented method for classifying medical images, comprising:
   receiving, by a processor, a medical image;
   extracting, by a multi-scale feature extraction module executed on the processor,
     a plurality of feature maps at three different spatial scales from the medical image,
     wherein the three spatial scales correspond to downsampling ratios of 4x, 8x, and 16x;
   computing, by a cross-scale attention module, attention weight matrices between
     the plurality of feature maps, and generating a fused feature representation
     by adaptively weighting and combining the plurality of feature maps based on
     the attention weight matrices; and
   classifying the medical image by processing the fused feature representation
     through a classification layer to generate a probability vector over a set of
     diagnostic categories.
```

### Independent System Claim
```
8. A system for medical image classification, comprising:
   a memory storing instructions for a trained multi-scale attention neural network; and
   a processor coupled to the memory, configured to:
     receive a medical image;
     extract a plurality of feature maps at three different spatial scales;
     compute cross-scale attention weights between the feature maps;
     generate a fused feature representation by adaptively combining the feature maps
       based on the cross-scale attention weights; and
     output a classification result for the medical image based on the fused feature
       representation.
```

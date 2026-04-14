# Patent Claim Examples by Technology Area

## AI/ML Claims

### Example 1: Training Method (CNIPA Style)

```
1. 一种神经网络模型的训练方法，其特征在于，包括：
   获取训练数据集，所述训练数据集包括多个样本对，每个样本对包含输入数据和对应的标签；
   将所述输入数据输入编码器网络，所述编码器网络包含N层Transformer编码层，每层包含多头自注意力子层和前馈神经网络子层，得到特征表示；
   基于所述特征表示和所述标签，计算对比学习损失函数和交叉熵损失函数的加权和作为总损失；
   利用所述总损失通过反向传播更新所述编码器网络的参数；
   当验证集上的性能指标连续M个训练周期无提升时，停止训练并保存模型参数。

2. 根据权利要求1所述的方法，其特征在于，所述多头自注意力子层中注意力头数为8，每个头的维度为64。

3. 根据权利要求1所述的方法，其特征在于，所述对比学习损失函数为InfoNCE损失，温度参数设置为0.07。

4. 根据权利要求1所述的方法，其特征在于，所述加权和中对比学习损失函数的权重为0.3，交叉熵损失函数的权重为0.7。

5. 根据权利要求1所述的方法，其特征在于，所述M的值为5，所述性能指标为F1分数。
```

### Example 2: Inference Method (USPTO Style)

```
1. A computer-implemented method for real-time object detection, comprising:
   receiving, by a processor, a video frame from an image sensor;
   resizing the video frame to a predetermined resolution;
   processing the resized video frame through a feature pyramid network
     to generate multi-scale feature maps at P3, P4, and P5 levels;
   applying, for each feature map level, a detection head comprising
     a 3x3 convolutional layer followed by parallel classification and
     regression branches to generate candidate bounding boxes; and
   applying non-maximum suppression with an IoU threshold of 0.5
     to the candidate bounding boxes to produce final detection results,
   wherein the method processes each video frame in less than 33 milliseconds
     on a single GPU.

2. The method of claim 1, wherein the feature pyramid network comprises
   a bottom-up pathway using ResNet-50 as backbone and a top-down pathway
   with lateral connections implemented as 1x1 convolutions.

3. The method of claim 1, wherein the classification branch outputs
   class probabilities using sigmoid activation, and the regression branch
   outputs bounding box offsets using linear activation.

4. The method of claim 1, further comprising:
   tracking detected objects across consecutive frames using a Kalman filter
   to maintain object identity.
```

## Software/System Claims

### Example 3: Data Processing System (CNIPA Style)

```
1. 一种分布式数据处理系统，其特征在于，包括：
   数据接收模块，配置为接收来自多个数据源的实时数据流；
   数据分区模块，配置为根据数据的时间戳和类别标签将数据流分配到不同的处理分区；
   并行处理模块，包含多个处理节点，每个处理节点配置为对分配到的数据分区执行预设的数据转换操作；
   结果聚合模块，配置为收集各处理节点的处理结果并按时间顺序合并；以及
   输出模块，配置为将合并后的结果输出至目标存储系统。

2. 根据权利要求1所述的系统，其特征在于，所述数据分区模块采用一致性哈希算法进行数据分配，以实现负载均衡。

3. 根据权利要求1所述的系统，其特征在于，所述并行处理模块还包括故障检测单元，配置为检测处理节点故障并自动将故障节点的任务重新分配至其他可用节点。
```

## Hardware-Integrated Claims

### Example 4: Edge Computing Device

```
1. An edge computing device for real-time inference, comprising:
   an input interface configured to receive sensor data at a rate of
     at least 30 samples per second;
   a dedicated neural processing unit (NPU) comprising:
     a matrix computation array configured to perform 8-bit integer
       matrix multiplications, and
     an on-chip SRAM buffer of at least 2 MB for storing intermediate
       activation values;
   a memory storing a quantized neural network model, the model
     comprising fewer than 5 million parameters; and
   a control processor configured to:
     preprocess the sensor data into input tensors,
     schedule inference operations on the NPU, and
     output inference results within 10 milliseconds of receiving
       the sensor data.
```

## Claim Scope Ladder Example

Showing how to create a scope ladder from broad to narrow:

```
Broadest:  "A method for processing data using a neural network"
           ↓ (too broad, likely has prior art)
Broader:   "A method for classifying images using a multi-scale neural network"
           ↓
Medium:    "A method for classifying medical images using a multi-scale
            attention-based neural network with cross-scale feature fusion"
           ↓
Narrower:  "A method for classifying medical images using a multi-scale
            attention-based neural network with cross-scale feature fusion,
            wherein the attention mechanism operates at three spatial scales
            with downsampling ratios of 4x, 8x, and 16x"
           ↓
Narrowest: [all of above] + "wherein the attention weights are computed
            using scaled dot-product attention with a projection dimension
            of 256 and the cross-scale fusion uses learned scale-specific
            weighting coefficients"
```

The independent claim should be at the "Medium" level -- broad enough for meaningful protection, narrow enough to avoid prior art. Dependent claims provide the narrower levels as fallback.

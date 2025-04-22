#deep-learning
## Neural Network
### 目标
* 给定输入，通过调节参数使输出更接近真实标签。这个过程是通过梯度下降和反向传播完成的
### 构成
* **输入层Input Layer**
	* 通常是向量或张量形式
* **隐藏层Hidden Layers**
	* 可能有很多层，每一层包含*线性变换*（Linear）+ *激活函数*（Activation）
	* 线性变换
		* 全连接层
		* 卷积层
	* 激活函数
		* ReLU
		* Sigmoid / Tanh / LeakyReLU
* **输出层Output layer**
	* 形式取决于任务
		* 分类任务 --> 一个vector + softmax(得概率)
		* 回归任务 --> 一个数或向量
		* 分割/检测 --> 特别结构（mask, boxes 等）
* 损失函数
	* 用来衡量输入和目标的差距
		* 分类
		* 回归
* 优化器
	* 根据loss梯度，来更新网络权重
		* 常见`SGD`, `Adam`, `RMSProp`
### 训练流程
1. **初始化参数**：  
    卷积核、全连接层权重、bias 都是随机初始的
2. **前向传播**：  
    输入数据通过网络，得到输出（预测结果）
3. **计算损失**：  
    将预测和真实标签比较，算一个 loss 值（如 CrossEntropy）
4. **反向传播**：  
    利用 loss 对每个参数计算“梯度”（偏导数）
5. **更新参数**（关键）：  
    用优化器（如 SGD 或 Adam）根据梯度 **更新参数的值**：
### 优化器
#### SGD Stochastic Gradient Descent（随机梯度下降）
* 目标： 通过不断降低损失函数loss来优化模型的表现
* $θ←θ−η⋅∇θ​L(θ)$
	* $L(\theta)$ 损失函数
	* $\theta$：模型的参数（比如权重、卷积核）
	- $η$：学习率（learning rate）
	- $\nabla_\theta L(\theta)$：损失函数对参数的梯度（方向）
- 为什么是 **随机**？
	- 传统的使用整个训练集计算梯度，效率比较低
	- SGD每次只用一个或一小批样本来估计梯度，是近似的带有 **随机性**
## Convolution卷积
* 滤波（filtering）
	* 尤其是在图像处理中，整个卷记得过程也可以叫filtering
* 卷积核（kernel）
	* 也叫mask/filter 掩膜/滤波器
	* 是一个在图像上滑动的小矩阵，对每个局部区域做加权求和
	* **作用**：决定提取什么特征，比如边缘、模糊、纹理等
	* Gaussian kernel(高斯核)
		* 根据高斯（正态）分布定义的卷积核，用来做图像平滑，效果比均值核更自然
		* 原理：在卷积时对中心像素权重更高，周围像素权重较低，符合“距离越近越重要”原则
		* 公式： $h(u,v) = \frac{1}{2\pi\sigma^2} e ^\left( -\frac{1}{2\sigma^2} (u^2 + v^2) \right)$
		* $\sigma$ 是标准差，控制高斯核的扩散范围，越大卷积时就要考虑更远的邻域，图像越模糊（**$\sigma$越大，平滑越强**）
	* Sobel kernels
		* 做边缘检测的卷积核
		* 检测图像中亮度变化剧烈的地方（即图像的边缘）
	* response最大
		* 即用多个不同的卷积核处理后的输出最大的
* 互相关运算（Cross-correlation）
	* 卷积Convolution，实际是将kernel旋转180后再做互相关运算（cross-correlation）
	* ppt上讲的就是用一个都是1/9的矩阵来和像素值相乘求和（即求平均值）
	* 在使用Gaussian kernel时，旋转kernel和之前一样，因此使用Gaussian kernel做卷积和做互相关运算是一样的
* 填充（padding）
	* 没有padding的话，输出会缩小，因为kernel只能在图像中央滑动
	* zero padding
		* 在图像边缘补0，使得kernel可以覆盖边缘，输出尺寸不变
* 步长（stride）
	* 每次跳几个像素滑动
	* 影响：输出尺寸
* 扩张（Dilation）
	* 拉大卷积核的覆盖范围
	* 如**Dilation=2** 时，卷积核中间会**隔一个像素再取值**
	* <img src="https://img.dirtsai.work/astro-blog/2025/04/b2e67cb181d66bd24a8aaaee6fa577dc.png" width="300">

## Convolutional neural networks 卷积神经网络
### layer
* 卷积层
	* 输入：局部窗口
	* 所有位置共享同一个卷积核权重
	* 提取局部特征
* 全连接层fully connected layer
	* 输入：所有输入（flatten）
	* 每个神经元有自己一整套独立权重
	* 能感知全局信息
* 注意力层
	* 输入：全局+加权注意力机制
	* 动态计算权重
	* 关注重要信息
* 池化层Pooling layer（非学习层）
	* 作用：缩小特征图的尺寸，同时保留最重要的信息。用简单的操作对局部区域做汇总
	* 如max-pooling
		* 4x4矩阵划分为2x2的，然后每个2x2取最大

### Over-fitting过拟合
* 现象
	* 在训练集表现越来越好，在测试集表现越来越差
* 解决
	* 简化模型结构（减少参数）
	* Data Augmentation（数据增强）
		* 通过图像变化扩充训练集，flip， translate等
		* 随机复合变换（random composite transformations）
			* 组合叠加变换
	* Dropout正则化方法
		* 训练时随机丢弃一部分神经元设为0，从而让模型不太依赖某一部分特征
		* 机制：
			* 每次训练，以概率p把输入张量中的一部分元素设为0
			* 保持期望值不变，其余保留下来的元素会放大，放大的比例是$\frac{1}{1-p}$
### Batch normalisation批量归一化
* 标准化
	* 在向前传播阶段，对mini-batch中每个特征通道（channel）的值，按batch维度做标准化处理
	* $r′=\frac{r - \mu_R}{\sigma_R}​$
		* $\mu$ 是均值， $\sigma$ 是标准差
	* 目的：把输入的每个通道变成**均值为 0、方差为 1 的分布**
* 仿射变换（Affine transformation）
	* 是ax+b ，线性变化加平移
	* $BN(x)=γ⋅\frac{x-\mu}{\sigma}​+β$
	* 标准化后胎规整了，削弱了模型表达能力，加上仿射变化就可以让network削除要不要再缩放或者平移

### 感受野（Receptive Field）

* 输入图像中，对输出特征图某个位置有影响的区域大小
* 每经过一层，感受野就会扩大，因为可以用我这一层较为小的区域感受上一层较为大的区域了

### Grad-Cam（Gradient-weighted Class Activation Mapping）
* 可视化CNN的方法，模型是看图像哪个区域判断类别的
* 原理
	* 选一层，
		* 通常为CNN最后的卷积层，这层保留了空间结构信息，还能表达高级语义
	* 拿到这个类别的梯度
		* 计算每个通道的梯度权重
	* 加权求和特征图
		* 对这层的 feature map，每个通道乘以上面对应的权重，然后**加在一起**，得到一个 2D 热图。
	* ReLU 激活 + 插值到原图大小
		* 保留正贡献部分，再插值变大，叠加到原图上。
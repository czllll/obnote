---
tags:
- ai
---
* even though the name of this method is called "regression",but it's a classification method, particularly in binary classfication
#### logistic/sigmoid function
* 逻辑回归的核心是通过一个逻辑函数（通常是Sigmoid函数）将输入特征映射到一个概率值，表示某个事件发生的可能性。
* 数学模型
	* $h(x)=\frac1{1+e^{-(\beta_0+\beta_1x_1+\beta_2x_2+...+\beta_nx_n)}}$
	* β是模型的参数，x是输入特征,由于$h(x)$的值在0-1 之间，因此特别适合表示概率
	* 若$h(x)$接近 0 或1，这种“极端概率”常常出现在模型**过拟合**的情形中
* 分类
	* 逻辑回归通过设定一个阈值（通常为0.5）来进行分类。如果预测的概率大于这个阈值，则将样本归类为正类（例如1），否则归类为负类（例如0）
#### Decision boundary
* 决策边界（Decision Boundary）是分类模型用于将不同类别的样本分开的边界。它本质上是一种可视化分类效果的方法。
* 对于逻辑回归模型，决策边界是由假设函数的参数决定的（即$z=\beta_0+\beta_1x_1+\beta_2x_2+...+\beta_nx_n$）。
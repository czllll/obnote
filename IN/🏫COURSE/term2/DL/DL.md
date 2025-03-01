# Optimisation
## the optimisation problem 最优化问题
* the optimisation problem 最优化问题
	* $x^* = \arg\min_{x} f(x)$ 最优化问题的最优解
		* 表示在所有可能的 $x$ 中，找到一个 使得$f(x)$ 取得最小值的 $x$
* cost function /loss function 成本函数 损失函数
	* 上面的$f(x)$
*  n-dimensional optimisation problem
	* $\mathbf{x}^* = \arg\min_{\mathbf{x} \in \mathbb{R}^n} f(\mathbf{x})$
		* $\mathbb{R}^n$ n维实数向量空间（n-dimensional real vector space）
* Gradient descent 梯度下降算法
	* 一种最优化算法
	* **梯度（Gradient)** 是函数在某个点的方向导数，表示函数值在该店变化最快的方向和变化率
	* 梯度指向函数上升最快的方向，梯度下降则是沿着$-\nabla f(x)$的方向进行优化以最小化$f(x)$
	* $\mathbf{x}' = \mathbf{x} - \eta \nabla f(\mathbf{x})$
		* $x'$ ：更新后的参数 $x'$
		- $x$ ：当前参数 x
		- $\eta$ ：学习率（learning rate）
		- $\nabla f(x)$ ：目标函数 f(x) 的梯度（gradient)
		- f(x)对各个变量的partial derivatives 偏导数
$$
\nabla f(x) =
\begin{bmatrix}
\frac{\partial f}{\partial x_1} \\
\frac{\partial f}{\partial x_2} \\
\vdots \\
\frac{\partial f}{\partial x_n}
\end{bmatrix}
$$
* For convenience, we will refer to the partial derivative of the output with respect to the value at any intermediate node (including input nodes) as the gradient at that node.为了方便起见，我们将输出对任何中间节点（包括输入节点）的偏导数称为该节点的梯度。**即某个节点的梯度就是损失函数 L 对该节点的偏导数**

## Backpropagation 反向传播
* 是什么
	* 用于计算神经网络梯度的算法， 是Gradient Descent在神经网络中的实现方式
* 目标
	* 通过计算**损失函数（Loss Function）对每个参数的梯度**，然后使用 **梯度下降** 来更新模型的参数，以最小化损失
* 核心思想
	* 链式法则（Chain Rule）**计算神经网络各层参数的梯度**，然后**沿着梯度的反方向更新参数**，使得损失降低
* 步骤
	* 前向传播（Forward Pass）
	- 计算损失（Compute Loss）：比较预测结果和真实值，计算误差（loss）。
	- 反向传播（Backward Pass）：通过链式法则计算每一层的梯度。
	- 参数更新（Gradient Descent）：使用梯度下降或其变体（如 Adam）更新神经网络参数。

## Functions of vectors 向量函数
### 张量tensor
* 是多维数组，在ml和dl中是数据的基本表现形式

| 阶数（Rank）              | 解释              | 例子                                                          |
| --------------------- | --------------- | ----------------------------------------------------------- |
| **0 阶张量（标量，Scalar）**  | 单个数             | $3.14,−7,422$                                               |
| **1 阶张量（向量，Vector）**  | 数组（一维）          | $\mathbf{x} = [1, 2, 3]$                                    |
| **2 阶张量（矩阵，Matrix）**  | 矩阵（二维）          | $\mathbf{A} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ |
| **3 阶张量（立方体，Tensor）** | 三维数据（例如 RGB 图像） | T\                                                          |
| **4 阶张量及以上**          | 高维数据（如批量图像）     | T                                                           |
* Jacobian 矩阵
	* 是一个用于表示**向量值函数对多个变量的偏导数**的矩阵
	* 一个向量值函数f(x)将n维输入映射到m维输出：$f:Rn→Rm$

$$
\mathbf{f}(\mathbf{x}) =
\begin{bmatrix}
f_1(x_1, x_2, \dots, x_n) \\
f_2(x_1, x_2, \dots, x_n) \\
\vdots \\
f_m(x_1, x_2, \dots, x_n)
\end{bmatrix}
$$
* Jacobian 矩阵表示
$$
J =
\begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} & \dots & \frac{\partial f_1}{\partial x_n} \\
\frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} & \dots & \frac{\partial f_2}{\partial x_n} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \frac{\partial f_m}{\partial x_2} & \dots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix}

$$
* **行数 m 表示输出维度，列数 nn 表示输入维度**
* **每个元素** $J_{ij}$​ **是 $f_i$​ 对 $x_j$​ 的偏导数**
* 反向传播中的梯度传播规则
	* $\frac{\partial L}{\partial x} = J^T \frac{\partial L}{\partial y}$

# Linear classifier线性分类器
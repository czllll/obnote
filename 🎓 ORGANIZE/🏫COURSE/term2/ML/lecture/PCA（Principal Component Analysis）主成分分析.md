* 是什么
	* 高位数据通常蕴含在低维结构中
	* PCA 是一个降维技术
* 核心思想
	* 投影：
		* 找到一个最能解释数据变化的方向，然后把数据投影到这个方向上（做降维）
		* 矩阵投影：$Z=XU \in \mathbb{R}^{N \times 3}$
		* 单样本投影：$z = x^Tu$
	* 重建
		* 原高维数据=低维度数据* 主成分方向（特征向量）
		* 矩阵重建：$X=ZU^T+X$
		* 单样本重建: $\bar{x}^i​=z_i​U^T+\bar x$
* 如何确定子空间（两种PCA的解释）
	* Maximum variance direction
	* minimum reconstruction error
* Objective 目标： 找一个单位向量，使得投影$x^Tu$（**向量 x 在单位方向向量 u 上的投影的“大小”**（也叫“投影长度”））的方差最大
	* x：是一个高维数据点（我们要投影的对象）
	- u：是一个方向向量（主成分方向）
		* $\max_{\|\mathbf{u}\|_2 = 1} \ \mathrm{Var}(\mathbf{x}^T \mathbf{u})$
		* 注意：
			* 推导过程 s.t. 是 subject to（前提是）的意思
			* 因为数据已经被centred过，所以使用的是样本方差共识，不用再减平均值了
			* 这里的方差是除以N-1是因为这里是用样本估计总体方差，若除以N则会低估方差，因此除以N-1，这个叫贝塞尔校正（Bessel's Correction）。使用N-1就是在做无偏估计。
			* 无偏估计（unbiased estimator）：用样本算出来的结果，平均气来不会高估或者低估真实值
		* ![image.png](https://img.dirtsai.work/astro-blog/2025/03/ef68c9214ae004ffb51e7c7232bbd490.png)
* 样本协方差矩阵（Sample Covariance Matrix）- 大括号里的
	* 反映两两变量之间的线性关系
* Rayleigh Quotient（Rayleigh 商）
	* Rayleigh 商表示将数据投影到某个方向后能保留多少方差信息
	* $R(u) = \frac{u^TAu}{u^Tu}$(若$||u|| = 1$, 则$R = u^TAu$)
* 拉格朗日函数Lagrangian
	* 适用于解决带约束的最优化问题的一种方法，特别适用于等式约束的情况
	* 目标函数：$min f(x)$
	* 带有等式约束：$g_i(x)=0, i=1,2,...,m$
	* 引入拉格朗日乘子Lagrange multipliers $\lambda_i$,乘子是一个被引入的变量,构造拉格朗日函数。这样我们就把约束也放到函数中了，这个函数的驻点（即对x和$\lambda$同时求导为0的点）就是原约束问题的候选最优解
		* $\mathcal{L}(x, \lambda) = f(x) + \sum_{i=1}^{m} \lambda_i g_i(x)$
	* 本质上把一个有等式约束的最优化问题转换成了一个无约束的最优化问题

* constrained optimization(约束优化)
	* PCA本质上就是一个带约束的优化问题
	* 拉格朗日乘子法Lagrangian
		* 把约束条件也合并进目标函数，构造一个新函数（拉格朗日函数），求其极值
		* $\mathcal{L}(u,\lambda) = u^T\sum u-\lambda(u^Tu-1)$, 约束条件就是u是单位向量
		* 对u求导得到$Σu=λu$, 即最优的u是协方矩阵的特征向量
	* **结论**：最大方差方向就是协方差矩阵中最大特征值对应的特征向量





## 线性代数基础
* 向量转置transpose：将列向量变为行向量或反之
* 内积dot product/ inner product: 两个向量的乘积，结果是标量
* 标量scalar：普通的数字没有方向
* 范数norm：向量的长度或大小
	* L1​ 范数：$\|\mathbf{x}\|_1 = \sum |x_i|$
	- L2​ 范数：$\|\mathbf{x}\|_2 = \sqrt{\sum x_i^2}$
	- L3​ 范数：$\|\mathbf{x}\|_3 = \left( \sum |x_i|^3 \right)^{1/3}$
* 投影projection：向量x在方向u上的投影长度为：$x^Tu$
	* 投影向量为$(x^Tu)u$
* 特征值和特征向量：
	* 当一个向量被矩阵变换时，它的方向不变，最多只被“拉伸或压缩”，这种特殊的向量就是特征向量，而它被拉伸的倍数就是特征值
* 张成（span）：
	* 用一组向量通过加权组合（线性组合）所能到达的所有点的集合。即这组向量能覆盖的空间
* 中心化和标准化
	* 中心化（Centering）
		* $x_i​←x_i​−\bar x$
		* 将数据平移，使得均值为0
	* 标准化（Normalization）
		* $x_i ​← \frac{x_i-\bar x}{\sigma}$
		* 让所有特征具有相同的量纲（均值为0，标准差为1）
* 250217
* objective
	* univariate & multivariate Gaussian distributions
	* MLE for Gaussian paran=meters
	* Spectral decomposition of the covariance matrix 协方差矩阵的谱分析
### univariate Gaussian Distribution
* parameters
	* $\mu$ : position of centre (mean)
	* $\sigma$ : width/ spread of distribution (standard deviation)标准差
* Notion of probabilities : *discrete* -> *continuous* 
#### Probability density function (pdf) 
* 概率密度方程
* probability of a continuous variable lying in a small interval
* 2 conditions
	* $p(x) >= 0$
	* $\int_{-\infty}^{\infty} p(x)dx = 1$
* pdf of Gaussian distribution
	* $\mathcal{N}(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} e ^\left( -\frac{1}{2\sigma^2} (x - \mu)^2 \right)$
	* Expectation(期望): $\mathbb{E}[\mathcal{N}(x \mid \mu, \sigma^2)] = \mu$
	* Variance（方差）：$var[\mathcal{N}(x \mid \mu, \sigma^2)]= \sigma^2$
	* Precision(精度)：$\beta = 1/\sigma^2$
	* Standard deviation: $\sigma = \sqrt{var}$
### Central Limit Theorem中心极限定理CLT
* Definition
	* Sums of random variables of any distribution follow an approximate Gaussian distribution
### Multi-variate Gaussian Distribution
* Ubiquity普遍性 of the Gaussian distribution can be explained by Central Limit Theorem

### 如何从一维扩展到二维的？
* 在多维随机变量时，不仅要关心每个变量的离散程度，还要关系不同变量之间是否存在某种相关关系
* 协方差
	* $Cov(x_i​,x_j​)=\mathbb{E}[(x_i​−μ_i​)(x_j​−μ_j​)]$
		* i = j, 协方差退化为方差 $Cov(x_i​,x_i​)=Var(x_i​)$
		* i != j, 协方差描述的是$x_i$ 和 $x_j$之间的线性关系
* 中心矩（Central Moment）
	* 用于描述随机变量相对于其均值的偏离情况。对于随机变量 X及其均值 $\mu = \mathbb{E}[X]$，第 k 阶中心矩定义为：$\mu'_k = \mathbb{E}\left[(X-\mu)^k\right]$
	* 第二阶中心矩 就是方差
* 协方差矩阵
	* 用来描述多个随机变量之间的联合离散程度及相互关系
	* 协方差矩阵是“(随机向量 - 均值向量)”的外积（Outer Product）再取期望
	* $Σ=E[(X−μ)(X−μ)^T]$
$$
\Sigma = 
\begin{bmatrix}
\operatorname{Var}(x_1) & \operatorname{Cov}(x_1, x_2) \\
\operatorname{Cov}(x_2, x_1) & \operatorname{Var}(x_2)
\end{bmatrix}
=
\begin{bmatrix}
\sigma_1^2 & \rho\,\sigma_1\,\sigma_2 \\
\rho\,\sigma_1\,\sigma_2 & \sigma_2^2
\end{bmatrix}.

$$

### 各向同性 (Isotropic) 和 各向异性(anisotropic)
* Isotropic
	* **各向同性**意味着分布在所有方向上的散布程度都相同。
	* $Σ=σ^2I$ 
		* $I$是单位矩阵
	* 二维情况下是一个圆形
* anisotropic
	* **各向异性**意味着在不同方向上的分布特征（方差、相关性）不一样
	* 当 $\sigma_2^2 \neq \sigma_2^2$或者 $ρ≠0$时,不同方向的方差就不一样

### 高斯分布的协方差矩阵有什么信息
* 对称的， 即$Σ = Σ^T$, 对称矩阵在适当的坐标系下可以表示成只含有对角元素的形式，这就是所谓的**特征分解**或**谱分解**
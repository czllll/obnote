# Bayes' Rule
* outline
* ![image.png](https://img.dirtsai.work/astro-blog/2025/01/cddf7a77e595abf1fa6d6a884113f75d.png)
## 一些名词
* 条件概率（Conditional Probability）
	* 在另一变量已知时的概率$P(X | Y = y)
* 联合概率Joint Probability
	* **X 和 Y 同时满足特定取值的概率**
	* $p(X,Y)$ 或$P(X \cap Y)$
	* 通过概率的乘法法则，我们有$P(X,Y)=P(X|Y)P(Y)$
		* 其中$P(Y)$是边缘概率
		* $P(X|Y)$是条件概率
	* 含义：
		* 离散变量：表示两个或多个随机变量发生的概率
		* 连续变量：严格定义是概率密度（因为单点概率是0，拿身高体重举例）
* 边缘概率Marginal Probability
	* 从联合概率分布中，通过对其他变量求和（离散）或积分（连续）得到的单个随机变量的概率分布
	* 忽略其他变量的额影响，只关注目标变量的概率
* 离散变量Discrete Variable
	* 取值可数
* 连续变量Continuous variable
	* 取值不可数，需要用积分处理
* 边缘化Marginalization/ Summing out
	* 通过求和/积分消除联合概率中的某些变量
* 随机变量Random Variable
	* 通常用大写字母表示如X Y
* 概率密度函数Probability Density Function PDF
* 概率质量函数Probability Mass Function PMF
* 积分 Integral
* 大小写P
	* **大写 P**：离散概率、一般概率测度。
	- **小写 p**：连续概率密度函数（或广义的概率表示）。
- 独立同分布（Independent and Identically distributed, iid）
	- 独立
		- 随机变量相互独立，一个变量的取值不影响其他变量
	- 同分布
		- 所有随机变量服从相同的概率分布
##  Sum and Product Rule
* **The Sum Rule**
	* $P(X) = \sum_Y P(X, Y)$ 离散
	* $P(X) = ∫p(X,Y)dY$ 连续
	* 和法则用于计算**边缘概率(margin probability)**,通过对联合概率求和或积分消去其他变量
* **The Product Rule**
	* $P(X,Y)=P(Y|X)P(X)$ 离散
	* $f_{X,Y}(x, y) = f_{X \mid Y}(x \mid y) \cdot f_Y(y)$ 连续
		* 联合概率密度和分解为条件概率密度和边缘概率密度的乘积
## Bayes's Rule
* First, we have the **Product Rule**:$P(X,Y)=P(Y|X)P(X)$ 
* We can decompose it: $P(X|Y) P(Y) = P(Y|X)P(X)$
* Or we can use the sum rule $P(X|Y) = \frac{P(Y|X)P(X)}{\sum_X P(Y|X)P(X)}$

## Discrete Distributions
### Bernoulli伯努利分布
* Definition
	* Single trial with only 2 possible outcomes(T/F) 只有0/1 两种取值
		* **成功（1）**：概率为 $\mu$（例如：硬币正面）。
		- **失败（0）**：概率为 $1−μ$（例如：硬币反面）。
* Bernoulli distribution formula:
	* $Ber(x|μ) = μ^x * (1-μ)^{1-x}, where 0 ≤ μ ≤ 1$
* Expected value: $E(x) = μ$
* Variance: var(x) = $μ(1-μ)$
### Likelihood 似然
* Supposed that we have a set of data point from a Bernoulli process
	* eg x1 = H x2 = T x3 = T
	* $μ(1 − μ)2 = Ber(x = 1 | μ)Ber(x = 0 | μ)Ber(x = 0 | μ)$
	* **Probability**
		* Known $\mu$, calculating chance of specific outcomes.已知参数计算出观测数据出现的可能性
	* **Likehood**
		* Known outcomes, treating expression as a function of parameter $\mu$
* **likelihood function**
	* $L(\theta|x) = P(x|\theta)$
	* $P(x|\theta)$ is a *probability* distribution(a conditional probability) and describes the likelihood of observing x given a particular $\theta$
	* $L(\theta|x)$ is a *function* of $\theta$ and is used to evaluate how likely different values of $\theta$ are, given the observed data.
	* $\theta$ - parameters
	* x - observation
	* eg: toss 10 times coins 7h 3t
		* $L(p) = p^7 * (1-p)^3$
		* usually use log to avoid multiply
![image.png](https://img.dirtsai.work/astro-blog/2025/01/93364e05e256540e3d38eea3f912e1b3.png)
topics
![image.png](https://img.dirtsai.work/astro-blog/2025/01/69df364f8694044fb7d1d3fdecba42c3.png)
# statistics 

## simple objectives

* **recap of statistics**
	* basic
		* stochastic variable随机变量
		* state space
		* probability distribution
		* events
		* sampling
	* Discret distributions:
		* Bernoulli
		* Binomial
	* Continuous distributions
		* Uniform均匀分布
		* Gaussian高斯分布
	* Expectation value期望值, variance方差, standard deviation标准差
* **Sum and Product rule of probability 概率的和积规则**
* **Bayes laws**

## Stochastic Variables and State Space
* Stochastic variable can take on certain values from state space
* $P(X='T') = 0.5$
	* X - stochastic variable
	* 'T' - possible outcome, must be an element of the *state space*
	* Outcomes are mutually exclusive相互排斥的
	*  In principle every element of a state space may be an outcome
	* *probability* - $p_i$
		* $0 \le p_i \le 1$
		* $\sum_i p_i = 1$

## Probabilities
### Bayesian贝叶斯
* Definition
	* Probabilities express a (subjective) degree of belief, quantified by $0 \le p \le 1$
* Mechanism
	* **start**: Requires a *prior probability distribution* (initial judgement/ experience)
	* **process**:Updates this probability using Bayes' theorem when data arrives
	* **result**: Produces a posterior probability distribution which is more precise and closer to the true value.
* Advantage
	* Can handle non- repeatable experiments or events, which is distinguishes it from *frequentist approaches*
* Limitation
	* No systematic way of choosing prior probabilities -> *subjectivity*
### Frequentist频率派
* Definition
	* Probabilities are estimates made over a large ensemble of identical experiments大量相同实验
	* Assume any experiment is *repeatable*: often problematic

## Expectation & Variance
* Expectation期望
	* Definition
		* The average value we "expect" to get after many trails
	* Formula
		* discrete variables
			* $E(X) = \sum (x * p(x))$
		* continuous variables
			* $E(X) = \int x * f(x) dx$
* Variance方差
	* Definition
		* Average squared deviation from the mean
	* Formula
		* $Var(X) = E[(X - μ)²] = E(X²) - [E(X)]²$
		* $\mu$ - mean
		*  只需要计算X^2 的期望和X的期望即可

## Discrete Distributions
### Bernoulli伯努利分布
* Definition
	* Single trial with only 2 possible outcomes(T/F)
* Bernoulli distribution formula:
	* $Ber(x|μ) = μ^x * (1-μ)^(1-x), where 0 ≤ μ ≤ 1$
* Expected value: E(x) = μ
* Variance: var(x) = μ(1-μ)
### Likelihood 似然
* Supposed that we have a set of data point from a Bernoulli process
	* eg x1 = H x2 = T x3 = T
	* $μ(1 − μ)2 = Ber(x = 1 | μ)Ber(x = 0 | μ)Ber(x = 0 | μ)$
	* **Probability**
		* Known $\mu$, calculating chance of specific outcomes.
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

### Maximum Likelihood Estimation(MLE)
* to find parameter values of a statistical model that maximize the probability of observing the given data.
* 
### Binomial 二项分布
* the sum of n independent Bernoulli trials.
* Formula
	* $P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$
		* $\binom{n}{k} = \frac{n!}{k!(n-k)!}$ is the binomial coefficient
		- $p$ is the probability of success on each trial
		- $n$ is the total number of trials
		- $k$ is the number of successes

## Continuous Distributions
### Gaussian/ Normal  Distribution 高斯/正态分布
* Formula
	* $f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
		* $\mu$ - mean
		* $\sigma$ - standard deviation标准差
* Expected Value (Mean)： $\mathbb{E}[N(x|\mu,\sigma^2)] = \mu$
* Variance：$\text{var}[N(x|\mu,\sigma^2)] = \sigma^2$ 

### Uniform Distribution 均匀分布
* Definition
	* all possible values have equal probability of occurring.
		* Probability of a number being in some interval is well defined

# Sum and Product rule
* Sum rule
	* $P(X) = \sum_Y P(X, Y)$
* Product rule
	* $P(X, Y) = P(Y | X) P(X)$





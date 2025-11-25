---
tags:
- ai
---
* RNN用来处理序列，序列数据是通过时间步输入进去的
# vanilla/Elman RNN
* 早期的RNN，通过引入隐藏状态（hidden state）来保留过去的信息，从而在时间维度上对信息进行建模。
## 结构
* 对于每个时间步$t$, 输入$x_t$，输出$y_t$，公式如下
	* $h_t= tanh(W_{xh}x_t+W_{hh}h_{t-1}+b_h)$
	* $y_t=W_{hy}h_t+b_y$
*  $h_t$: 当前时间步的隐藏状态，表示了过去信息的“记忆”
* tanh： 激活函数，也可以用ReLU等函数
* $b_h$:偏置项
* $W_{xh}$: 输入到隐藏层的权重矩阵
	* 把输入$x_t$映射到隐藏层维度
* $W_{hh}$:上一个隐藏状态到当前隐藏状态的权重（体现了循环）
	* 连接时间上前一个时刻的隐藏状态$h_{t-1}$到当前的$h_t$
	* 就是RNN的循环记忆，让模型可以记住之前的信息
* 这两个权重矩阵：1.可以匹配维度；2.可以矩阵式模型在训练中学习的参数
* ![image.png](https://img.dirtsai.work/astro-blog/2025/04/93478d0eaf4b9409194bb90b84cabaf3.png)
* 输入
* 隐藏状态
	* 即$h_t$,从当前输入和上一步结果计算得到
* 输出

# 场景
### 文本生成
* **训练步骤**
	* 每个时间步，模型输出一个softmax分布$o_t$,表示下一个token是谁的概率分布。由于我们已经有真实的目标字符$y_t$（ground truth）,我们计算 $loss_t=-logP(y_t|o_t)$,越小说明预测的越好。这里是**负对数似然（Negative Log-Likelihood, NLL）损失**
	* 每个时间步都会有loss， 一句话的loss 加起来取平均
	* 我们一般是minibatch样本，也就是很多句话。每一句都有loss，加起来取平均/求和就是最终的训练损失
	* 使用反向传播+优化器，根据loss 更新模型参数
	* **总结**：字符级的RNN，用滑动窗口的方式，在token序列上提取输入/输出对，训练模型学会下一个token的预测
# 训练挑战
* vanishing gradient(梯度消失)
	* 在RNN进行反向传播时，序列越长，越往前面的时间步的梯度就会越小最终趋于0
	* 结果就是无法学习早期的信息，长期依赖学不到
* prediction drift预测漂移
	* 模型用前一步预测结果当作下一步的输入（自回归生成）
	* 一旦前面预测错了 ，后面就会越来越偏
## 解决梯度消失
* **LSTM**Long short-term memory
	* 增加了一个记忆单元c
	* 流程
		* 给定输入 $x_t$​ 和上一状态 $h_{t-1}, c_{t-1}$
		- 计算 3 个门：$f_t, i_t, o_t$,分别是遗忘门，输入门，输出门
			- $f_t = \sigma(W_f x_t + U_f h_{t-1} + b_f)$
			- $i_t = \sigma(W_i x_t + U_i h_{t-1} + b_i)$
			- $o_t = \sigma(W_o x_t + U_o h_{t-1} + b_o)$
		- 用 $i_t$​ 和 $\tilde{c}_t$ 决定新内容写入多少, 用 $f_t$​ 决定旧记忆保留多少，更新 $c_t​$
			- $\tilde{c}_t = \tanh(W_c x_t + U_c h_{t-1} + b_c)$ (候选记忆)
				- 当前时间步的新信息提案
			- $c_t=f_t\circ c_{t-1}+i_t\circ \tilde{c}_t$
			- 反向传播时有，$\frac{\partial L}{\partial c_{t-1}} = \frac{\partial L}{\partial c_t} \circ f_t$
			- 模型需要学习的是生成$f_t$的参数，即上面计算的。
		- 最后用$o_t$​ 控制多少记忆“露出来”，作为当前输出 $h_t$
			- $h_t=o_t\circ tanh(c_t)$

* **GRU** Gated Recurrent Unit
	* 两个门
		* 更新门update gate
			* 控制保留多少旧信息
		* 重置门reset gate
			* 控制丢弃多少旧信息
$$
\begin{aligned}
z_t &= \sigma(W_z x_t + U_z h_{t-1} + b_z) \\
r_t &= \sigma(W_r x_t + U_r h_{t-1} + b_r) \\
\tilde{h}_t &= \tanh(W_h x_t + U_h (r_t \circ h_{t-1}) + b_h) \\
h_t &= (1 - z_t) \circ h_{t-1} + z_t \circ \tilde{h}_t
\end{aligned}
$$
# Sequential data序列数据
* 应用场景
	* Sentiment analysis
	- Personality insights from text 从文本分析性格
	- Image captioning图像描述
	- Language translation
		- sequence-to-sequence
	- Text to speech
	- Text generation
	- Stochastic processes随机过程建模
		- 数学物理中的时间序列建模
		- 股票、马尔可夫过程。。
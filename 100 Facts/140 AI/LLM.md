---
tags:
  - ai
---

## 分词Tokenization

* BPE byte pair Encoding 字节对编码
	* 在文本中找到相邻且频率最高的两个基本单元，把他们合并成一个新的、更长的单元并不断迭代
	* UTF-8 编码可以把任何 text 拆成 256 个基础字节流，在该 256 个基础字节流基础上运行 BPE
* 分词器是完全独立于 LLM 的组件，有自己单独的训练数据和训练过程
* 产出：词表
## 无监督Pre-training
* core： next-token continuation；计算模型预测的 token 和真实预料中下一个 token 之间的交叉熵误差
* LLM 多是基于[[Transformer]]架构的
* 流程
	* token embedding
		* 该过程将分词阶段产出的词表映射成一个浮点向量表
	* 注入位置编码
		* 传统：绝对位置编码
		* 现在带：旋转位置编码RoPE
	* Transformer 层
		* 归一化(RMSNorm)
		* Self-Attention：物理上加一个 Mask，强制模型在计算注意力时只能看到当前和过去的 token，不能看未来的token
	* 前馈神经网络 FFN/MLP
		* 在 LLM 语境下 FFN 和 MLP 指的都是同一件事情。只要是数据从前向后单向流动，没有形成循环回路就可以成为 FFN
		* MLP（Multi-Layer Perceptron 多层感知机）
			* 多层感知机一般由输入层、多个隐藏层（带非线性激活函数）和输出层组成的全连接网络。输入层接受原始特征；隐藏层的每个神经元 1是对上一层所有输出做加权求和，2 是通过[[激活函数]]引入非线性

		* ![image.png](https://img.eryi.me/astro-blog/2026/05/7b83b667483481af61227f60a8a9fada.png)

		* 现代 MLP 中，ReLU 函数通常被替换为 SwiGLU，使用更复杂的数学运算提升模型逻辑能力
	* lm_head
		* 模型输出对下一个 token 的预测和真实预料 token 的误差，若算出误差，优化器（如 AdamW）启动反向传算法，一路更新每一层的 Transformer block 中的 MLP，Attention、RMSNorm，直到 Embedding
## SFT 监督微调 Supervised Fine-Tuning
## Alignment 与 RL
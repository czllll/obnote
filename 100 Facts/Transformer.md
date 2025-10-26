
| domain | layer         | type  |
| ------ | ------------- | ----- |
| #ai    | #transformer  | #fact |

# Attention
* 
## Multi-head Attention
* Run attention h times on different linear mappings of Q, K, V
* 每个头都用不同的投影矩阵学习不同的关系
* ![image.png](https://img.dirtsai.work/astro-blog/2025/05/5f12bb928d2b62623ee51d0f225130d0.png)
## Position encoding
* Attention 是全并行的， 看不到词的顺序

## 结构
### encoder
* 自注意力
	* 关注单个序列内部的不同位置，更准确表示输入序列
* 前馈神经网络
### decoder
* 
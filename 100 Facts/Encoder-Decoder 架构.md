
| domain | layer        | type  |
| ------ | ------------ | ----- |
| #ai    | #transformer | #fact |

# What
* Usually used for tasks where input and output are both sequences, but possibly of different lengths or structures.(指输入输出)
* Originated in sequence-to-sequence(*Seq2Seq*) model for tasks like translation
* ![image.png](https://img.dirtsai.work/astro-blog/2025/05/ee5f6359f6199ff5a1620a0846d574a6.png)

# Why
## Why we use
* input and output have different lengths or formats
* we can have step-by-step or structured output.
# How
## How it works
* Encoder
	* The encoder takes a variable-length sequence as input and transforms it into a state with a fixed shape.
* Decoder
	* The decoder maps the encoded state of a fixed shape to a variable-length sequence.

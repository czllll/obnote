
| domain | layer | type  |
| ------ | ----- | ----- |
| #java  | #juc  | #fact |

* 内存屏障
	* volatile 有四种内存屏障
	* 在对一个volatile变量读时，会在其之后插入LoadLoad和LoadStore两个屏障
		* 两个屏障保证volatile变量后面的读/写操作不会重排序到volatile变量前面去
			* 为什么：因为我不希望出现重排序，后续的读写操作可能是基于 volatile 变量来的；
			* 注意先读后写是 cpu 的自然行为，JMM 规范要求 `LoadStore` 屏障的存在以保证跨平台的一致性，只是它在某些硬件（如x86）上不会产生实际的指令
				* PS：*在 **x86 TSO** 模型里确实如此，因为 TSO 保证了 Load→Store 的顺序。但在 **ARM / PowerPC** 这样的弱内存模型里，不加屏障，写是可能被乱序到读前的。*
	* 在对一个变量进行写的时候，会在其之前插入 StoreStore 屏障，在其之后插入 StoreLoad 屏障
		* StoreStore 屏障的作用是防止 volatile 变量写之前的普通写重排序到 volatile 变量写之后
			* 为什么：在发布 volatile 写操作之前，保证之前的所有普通写操作全部到位，防止写操作乱序；
			* 目的：保证有序性
		* StoreLoad 的作用是防止volatile 变量写之后的度写操作重排序到 volatile 变量写之前
			* 作用
				* 1. 禁止**之后**的读/写操作重排序到 `volatile` 写**之前**。
				* 2. 强制将 `volatile` 写及之前所有普通写的缓存刷新到主存。
#定长滑动窗口
* 套路
	* **入**：下标为 i 的元素进入窗口，更新相关统计量。如果 i<k−1 则重复第一步。
	* **更新**：更新答案。一般是更新最大值/最小值。
	* **出**：下标为 i−k+1 的元素离开窗口，更新相关统计量。

| 题号url                                                                                                       | 日期             |
| ----------------------------------------------------------------------------------------------------------- | -------------- |
| [1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/) | [[2024-08-24]] |
| [2269. 找到一个数字的 K 美丽值](https://leetcode.cn/problems/find-the-k-beauty-of-a-number/)                          | [[2024-08-24]] |
| [1984. 学生分数的最小差值](https://leetcode.cn/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)  | [[2024-08-27]] |
| [643. 子数组最大平均数 I](https://leetcode.cn/problems/maximum-average-subarray-i/)                                 | [[2024-08-27]] |
|                                                                                                             |                |

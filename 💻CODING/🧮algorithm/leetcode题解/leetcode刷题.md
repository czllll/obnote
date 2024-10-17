星号（* ）代表看解析做出来的
# 滑动窗口
## 定长滑动窗口
* 套路
	* **入**：下标为 i 的元素进入窗口，更新相关统计量。如果 i<k−1 则重复第一步。
	* **更新**：更新答案。一般是更新最大值/最小值。
	* **出**：下标为 i−k+1 的元素离开窗口，更新相关统计量。

| 题号url                                                                                                                                       | 日期             | 序号  |     |
| ------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | --- | --- |
| [1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)                                 | [[2024-08-24]] | 1   |     |
| [2269. 找到一个数字的 K 美丽值](https://leetcode.cn/problems/find-the-k-beauty-of-a-number/)                                                          | [[2024-08-24]] | 2   |     |
| [1984. 学生分数的最小差值](https://leetcode.cn/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)                                  | [[2024-08-27]] | 3   |     |
| [643. 子数组最大平均数 I](https://leetcode.cn/problems/maximum-average-subarray-i/)                                                                 | [[2024-08-27]] | 4   |     |
| [1343. 大小为 K 且平均值大于等于阈值的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/) | [[2024-08-28]] | 5   |     |
| [2090. 半径为 k 的子数组平均值](https://leetcode.cn/problems/k-radius-subarray-averages/)                                                             | [[2024-08-28]] | 6   |     |
| [2379. 得到 K 个黑块的最少涂色次数](https://leetcode.cn/problems/minimum-recolors-to-get-k-consecutive-black-blocks/)                                   | [[2024-08-29]] | 7   |     |
| [1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/)                                                                      | [[2024-08-29]] | 8   |     |
| [2841. 几乎唯一子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-almost-unique-subarray/)                                                    | [[2024-09-01]] | 9   |     |
| [2461. 长度为 K 子数组中的最大和](https://leetcode.cn/problems/maximum-sum-of-distinct-subarrays-with-length-k/)                                       | [[2024-09-01]] | 10  |     |
| [1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/)                                                    | [[2024-09-03]] | 11  |     |
| [2653. 滑动子数组的美丽值](https://leetcode.cn/problems/sliding-subarray-beauty/)                                                                    | [[2024-09-03]] | 12  | *   |
| [2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/)                                          | [[2024-09-06]] | 13  |     |
| [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)                                                                          | [[2024-09-06]] | 14  |     |
| [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/)                                               | [[2024-09-07]] | 15  |     |
## 不定长滑动窗口（求最长/最大）
* 固定一遍，调整另一边

| [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/)                                                                     | [[2024-09-07]]        | 16  | *        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | --- | -------- |
|                                                                                                                                                                               | [[2024-09-08]]开始用英文刷题 |     |          |
| [1493.Longest Subarray of 1's After Deleting One Element - LeetCode](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/submissions/1400472580/) | [[2024-09-24]]        | 17  |          |
| [2730.Find the Longest Semi-Repetitive Substring - LeetCode](https://leetcode.com/problems/find-the-longest-semi-repetitive-substring/submissions/1400487864/)                | [[2024-09-24]]        | 18  |          |
| [904. 水果成篮 - 力扣（LeetCode）](https://leetcode.cn/problems/fruit-into-baskets/description/)                                                                                      | [[2024-10-05]]        | 19  |          |
| [1695. 删除子数组的最大得分 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-erasure-value/description/)                                                                            | [[2024-10-05]]        | 20  |          |
| [2958. Length of Longest Subarray With at Most K Frequency - LeetCode](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/)                    | [[2024-10-05]]        | 21  | [[2958]] |
| [2779.Maximum Beauty of an Array After Applying Operation - LeetCode](https://leetcode.com/problems/maximum-beauty-of-an-array-after-applying-operation/description/)         | [[2024-10-05]]        | 22  | [[2779]] |
| [2024. 考试的最大困扰度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximize-the-confusion-of-an-exam/)                                                                              | [[2024-10-17]]        | 23  | [[2024]] |
| [1004. 最大连续1的个数 III - 力扣（LeetCode）](https://leetcode.cn/problems/max-consecutive-ones-iii/description/)                                                                       | [[2024-10-17]]        | 24  |          |

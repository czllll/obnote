# 8.5 Tue
| 时间段           | 内容               | 分点                                                               | 实际             |
| ------------- | ---------------- | ---------------------------------------------------------------- | -------------- |
| 07:00 - 08:00 |                  |                                                                  |                |
| 08:00 - 09:00 |                  |                                                                  |                |
| 09:00 - 10:00 |                  |                                                                  |                |
| 10:00 - 11:00 |                  |                                                                  |                |
| 11:00 - 12:00 | 论文第五章            | ~~5.1~~                                                          | 5.1            |
| 12:00 - 13:00 |                  | ~~5.2 5.3~~                                                      | 5.1            |
| 13:00 - 14:00 |                  | ~~5.4 5.5~~                                                      | 5.2 + part 5.3 |
| 14:00 - 15:00 | 论文第三章            | ~~3.1 3.2~~                                                      | 5.3 5.4        |
| 15:00 - 16:00 |                  | 3.3 3.4                                                          | 删除5.5          |
| 16:00 - 17:00 |                  | 3.5 3.6                                                          |                |
| 17:00 - 18:00 | 论文 第二章加参考文献到35左右 | 第二章                                                              |                |
| 18:00 - 19:00 | 休息；修复插图和表        |                                                                  | 吃饭             |
| 19:00 - 20:00 | 论文第六章            |                                                                  |                |
| 20:00 - 21:00 | codeup           | 56. 合并区间  <br>160. 相交链表  <br>42. 接雨水  <br>72. 编辑距离  <br>         |                |
| 21:00 - 22:00 | codeup           | 124. 二叉树中的最大路径和  <br>93. 复原IP地址<br>1143. 最长公共子序列<br>142. 环形链表 II | 3.1 3.2        |
|               |                  |                                                                  |                |
# 8.6 Wed
| 时间段           | 内容                   | 分点          | 实际      |
| ------------- | -------------------- | ----------- | ------- |
| 07:00 - 08:00 |                      |             |         |
| 08:00 - 09:00 |                      |             |         |
| 09:00 - 10:00 |                      |             |         |
| 10:00 - 11:00 |                      |             |         |
| 11:00 - 12:00 |                      |             |         |
| 12:00 - 13:00 | ~~论文第三章~~            | ~~3.3 3.4~~ | 3.3 3.4 |
| 13:00 - 14:00 |                      | ~~3.5 3.6~~ | 3.5 3.6 |
| 14:00 - 15:00 | ~~论文 第二章加参考文献到35左右~~ | ~~第二章~~     | 第二章     |
| 15:00 - 16:00 | 论文第六章                |             | 论文第六章   |
| 16:00 - 17:00 | 论文修改老师提的意见           | 3           | 打电话     |
| 17:00 - 18:00 |                      | 6           | 打电话     |
| 18:00 - 19:00 |                      | 9           |         |
| 19:00 - 20:00 |                      | 12          |         |
| 20:00 - 21:00 |                      | 15          |         |
| 21:00 - 22:00 |                      |             |         |
|               |                      |             |         |




* 图表问题
	* error bar
	* column 的横轴和纵轴保持一致
	* 图片字体
* 图片4.8 左右：when talking of averages would be good to give std deviation or variance?
* 4.1.3 上面： -were you able to look at the number of reasoning tokens consumed, and how that varied with successful hints/guesses? (I’ve found that reasoning models can use relatively more tokens when they get the answer wrong.. 
* 探究抽象性
	* You might say why you investigated  this, i.e. that one might hypothesise that more abstract words could be harder to describe?
* 拓展实验：POS
	* Do adjectives have relatively more synonyms than adverbs, which might make it easier?
Is it possible, to, at least qualitatively, compare the PoS of the hints vs the PoS of the target word? Do models always try to use the same PoS in their hint as the target word?
* 多语言实验：Unless you have done this in an earlier chapter, then perhaps justify the interest of a multilingual test and why Chinese (of course you are native language speaker), but also Chinese has some interesting differences compared to western language such as English. 
* on original Chinese task procedures 说明是啥
* it may guide models to construct more discriminative clues, thereby improving overall collaborative efficiency. 做一些分析？

| 修改点      | 位置                   | 是否定量 | 是否已有数据支持       |
| -------- | -------------------- | ---- | -------------- |
| 分析模型表现差异 | Discussion           | 否    | 可从参数/输出风格等出发推测 |
| 统计失败词交集  | Results + Discussion | 是    | 待补充分析          |
| 插入失败样例   | Results / Appendix   | 否    | 有原始日志即可        |
| 上传数据     | GitHub 并在论文中引用       | 否    | 可操作            |

| #   | 修改点                        | 章节/位置                | 是否已有数据                                | 老师批注                                                                                                                                                                                                                                                                     |
| --- | -------------------------- | -------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | 图表添加 error bar             | 所有柱状图、均值折线图          | ✅ 有                                   | “I recommend trying to include error bars so as to justify that one model’s performance is genuinely better than another’s.”                                                                                                                                             |
| 2   | 图表坐标轴统一                    | 所有柱图                 | ✅ 有                                   | _未明确指出，但为通用图表规范，横轴排序 & 纵轴一致性是基本要求_                                                                                                                                                                                                                                       |
| 3   | 图表字体优化                     | 所有图表                 | ✅ 有                                   | _未明确指出，但图表美观性影响可读性，默认执行_                                                                                                                                                                                                                                                 |
| 4   | 补充均值的标准差/方差                | 图 4.8 附近             | ✅ 有                                   | “When talking of averages would be good to give std deviation or variance?”                                                                                                                                                                                              |
| 5   | 分析 reasoning token 与成功率的关系 | §4.1.3               | ⚠️ 需提取 token 日志                       | “Were you able to look at the number of reasoning tokens consumed, and how that varied with successful hints/guesses? (I’ve found that reasoning models can use relatively more tokens when they get the answer wrong...)”                                               |
| 6   | 抽象性研究动机说明                  | §4.2.1 开头            | ✅ 有                                   | “You might say why you investigated this, i.e. that one might hypothesise that more abstract words could be harder to describe?”                                                                                                                                         |
| 7   | 词性拓展实验：hint 与 target 对比    | §4.2.3 拓展实验          | ⚠️ 可定性人工标注                            | “Is it possible, to, at least qualitatively, compare the PoS of the hints vs the PoS of the target word? Do models always try to use the same PoS in their hint as the target word?”                                                                                     |
| 8   | 分析词性影响难度（adj vs adv 同义词数）  | §4.2.3 拓展实验          | ⚠️ 可定性分析                              | “Do adjectives have relatively more synonyms than adverbs, which might make it easier?”                                                                                                                                                                                  |
| 9   | ~~多语言任务动机说明（为何选中文~~        | 中文实验 §4.4 开头         | ✅ 有                                   | “Unless you have done this in an earlier chapter, then perhaps justify the interest of a multilingual test and why Chinese (of course you are native language speaker), but also Chinese has some interesting differences compared to western language such as English.” |
| 10  | 原始中文任务流程补充说明               | 中文方法部分               | ✅ 有                                   | “On original Chinese task procedures: 说明是啥”                                                                                                                                                                                                                              |
| 11  | 联合提示能否提升协作效率：分析一下          | §4.5 或 Discussion    | ⚠️ 可通过 token 数 / success rate 或结构特征观察 | “It may guide models to construct more discriminative clues, thereby improving overall collaborative efficiency. 做一些分析？”                                                                                                                                                 |
| 12  | 模型表现差异解释                   | Discussion           | ✅ 可结合参数、策略等分析                         | “Can you say anything more about why one model does better than another? I guess it might just be down to model size? But is there anything else?”                                                                                                                       |
| 13  | 模型失败词交集分析                  | Results + Discussion | ⚠️ 需对比失败词                             | “When models fail to guess a word, is that constant across the models? I.e. are there particular target words which all models find hard, or does it vary across models?”                                                                                                |
| 14  | 插入典型失败样例                   | Results / Appendix   | ✅ 有原始日志                               | “It might be nice to provide some sample interactions between models, picking out some interesting failure cases?”                                                                                                                                                       |
| 15  | ~~上传数据并引用~~                | 方法或附录开头              | ✅ 可操作                                 | “In any case make all data available, e.g. in GitHub.”                                                                                                                                                                                                                   |
# Thu 8.7
| 时间段           | 内容  | 分点                            | 实际                       |
| ------------- | --- | ----------------------------- | ------------------------ |
| 07:00 - 08:00 |     |                               |                          |
| 08:00 - 09:00 |     |                               |                          |
| 09:00 - 10:00 |     |                               |                          |
| 10:00 - 11:00 | 项目  | 修复项目部署错误                      | 解决                       |
| 11:00 - 12:00 |     | 梳理和简历差异点并反思做法                 | 已经梳理                     |
| 12:00 - 13:00 |     | 加一个chunck上传书功能试一下1            | 先放弃                      |
| 13:00 - 14:00 |     | 加一个chunck上传书功能试一下2            | 先放弃                      |
| 14:00 - 15:00 | 刷题  | codeup 3道                     | 学了一下MCP相关的知识             |
| 15:00 - 16:00 |     | codeup 3道                     |                          |
| 16:00 - 17:00 |     | codeup3道                      | 在学习分布式知识                 |
| 17:00 - 18:00 |     | 美团一篇；滴滴一篇                     |                          |
| 18:00 - 19:00 |     |                               |                          |
| 19:00 - 20:00 | 改论文 | 改图片20张                        |                          |
| 20:00 - 21:00 |     | 改图片15张                        |                          |
| 21:00 - 22:00 |     | 第4点补充均值的标准差和方差；补充抽象性研究动机;第10点 | 学了一下分布式事务输出一篇笔记[[分布式事务]] |
|               |     |                               |                          |
项目描述**:** 基于 Spring Cloud 构建的算法社区平台，在传统技术讨论基础上新增小组协作、团队刷题打卡、进度

展示等差异化功能，解决传统 OJ 平台缺乏团队管理和协作学习的痛点。

• **Feed** 流推拉混合架构：设计推拉混合策略解决性能瓶颈，大 V 用户(粉丝>1 万)采用 Redis ZSet 预计算

Timeline，普通用户实时拉取，通过 Kafka 异步推送，Feed 刷新 P99 响应时间 100ms 内。

• **IM** 消息可靠投递机制：基于幂等消费设计+Kafka 分区保序构建可靠投递架构，Redisson 分布式锁解决并发冲突，ACK 确认机制保证持久化，端到端投递成功率 99.9%，支持离线消息批量推送。

• 评论系统 **LLM** 智能回复：设计 Prompt 模板聚合上下文，通过 LLM API 生成智能回复，Guava RateLimiter 实现用户维度限流，OkHttp 异步调用避免接口阻塞，集成优雅降级机制。

• 团队管理 **TCC** 分布式事务：采用 Seata TCC 模式解决跨服务数据一致性，Try-Confirm-Cancel 三阶段处理团队解散场景，支持网络异常和业务校验失败回滚。

• 多平台刷题数据增量同步：基于策略模式构建平台适配器处理 GraphQL/REST API 差异，统一数据模型兼容各

平台标准，@Scheduled 实现用户可配置定时同步，结合增量对比机制保证数据一致性。

• 事件驱动通知中台：设计统一事件总线，业务服务通过 Kafka 发布领域事件触发通知生成，支持通知聚合和批

量处理，分页查询+已读状态管理优化性能，事件处理延迟 200ms 内。

# 8.8 Fri
| 时间段           | 内容  | 分点                            | 实际  |
| ------------- | --- | ----------------------------- | --- |
| 07:00 - 08:00 |     |                               |     |
| 08:00 - 09:00 |     |                               |     |
| 09:00 - 10:00 |     |                               |     |
| 10:00 - 11:00 |     |                               |     |
| 11:00 - 12:00 | 项目  | 修复bug                         |     |
| 12:00 - 13:00 |     |                               |     |
| 13:00 - 14:00 |     |                               |     |
| 14:00 - 15:00 | 刷题  | codeup 3道                     |     |
| 15:00 - 16:00 |     | codeup 3道                     |     |
| 16:00 - 17:00 |     | codeup3道                      |     |
| 17:00 - 18:00 |     | 美团一篇；滴滴一篇                     |     |
| 18:00 - 19:00 |     |                               |     |
| 19:00 - 20:00 | 改论文 | 改图片20张                        |     |
| 20:00 - 21:00 |     | 改图片15张                        |     |
| 21:00 - 22:00 |     | 第4点补充均值的标准差和方差；补充抽象性研究动机;第10点 |     |
|               |     |                               |     |
bug清单：
* ~~点赞和收藏重新进入没有保持原来的点赞和收藏，但是点击以后的逻辑是正确的；例如我对一个post点赞，点击一下点赞成功，点赞数从0变成1，但当我从其他标签页回来，点赞是1但是没有我点赞的标记，我再次点击前端依旧显示点赞成功，但实际上调用的是取消点赞的借口，我再刷新一下点赞数就成0了~~
* ~~别人点赞，收藏，评论或者回复评论我 都要进行通知，目前后端已经有notification服务，前端的header上也有个小铃铛。
* 很多页面都是第一次请求会500
* ~~评论没有显示出条数
* ~~点击关注了以后，返回关注成功，但当我重新进入该用户主页后显示没有关注上。
* ~~post的日期显示格式问题
* 
feature清单：
* ~~评论功能添加
* AI点数添加
* AI聊天添加
	* 兑换券系统添加


# 8.9 Sat
| 时间段           | 内容  | 分点                            | 实际                   |
| ------------- | --- | ----------------------------- | -------------------- |
| 07:00 - 08:00 |     |                               |                      |
| 08:00 - 09:00 |     |                               |                      |
| 09:00 - 10:00 |     |                               |                      |
| 10:00 - 11:00 |     |                               |                      |
| 11:00 - 12:00 | 项目  |                               |                      |
| 12:00 - 13:00 |     |                               |                      |
| 13:00 - 14:00 |     | AI相关功能跑通                      |                      |
| 14:00 - 15:00 | 刷题  | codeup 3道                     |                      |
| 15:00 - 16:00 |     | codeup 3道                     |                      |
| 16:00 - 17:00 |     | codeup3道                      | 完善项目，AI功能跑通，先放弃Seata |
| 17:00 - 18:00 |     |                               |                      |
| 18:00 - 19:00 |     |                               |                      |
| 19:00 - 20:00 | 改论文 | 改图片20张                        |                      |
| 20:00 - 21:00 |     | 改图片15张                        |                      |
| 21:00 - 22:00 |     | 第4点补充均值的标准差和方差；补充抽象性研究动机;第10点 |                      |
|               |     |                               |                      |
# 8.10 Sun
| 时间段           | 内容  | 分点  | 实际  |
| ------------- | --- | --- | --- |
| 07:00 - 08:00 |     |     |     |
| 08:00 - 09:00 |     |     |     |
| 09:00 - 10:00 |     |     |     |
| 10:00 - 11:00 |     |     |     |
| 11:00 - 12:00 | 项目  |     |     |
| 12:00 - 13:00 |     |     |     |
| 13:00 - 14:00 |     |     |     |
| 14:00 - 15:00 | 刷题  |     |     |
| 15:00 - 16:00 |     |     |     |
| 16:00 - 17:00 | 八股  |     |     |
| 17:00 - 18:00 |     |     |     |
| 18:00 - 19:00 |     |     |     |
| 19:00 - 20:00 | 改论文 |     |     |
| 20:00 - 21:00 |     |     |     |
| 21:00 - 22:00 |     |     |     |
|               |     |     |     |

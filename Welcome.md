```dataviewjs
// 获取当前日期
const today = new Date();

// 获取今年的开始和结束日期
const startOfYear = new Date(today.getFullYear(), 0, 1);
const endOfYear = new Date(today.getFullYear() + 1, 0, 1);

// 计算当前日期是今年的第几天
const dayOfYear = Math.floor((today - startOfYear) / (1000 * 60 * 60 * 24)) + 1;

// 计算总天数和进度百分比
const totalDays = (endOfYear - startOfYear) / (1000 * 60 * 60 * 24);
const progress = (dayOfYear / totalDays) * 100;

// 格式化日期
const formattedDate = today.toLocaleDateString('zh-CN', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    weekday: 'long'
});

// 输出当前日期的天数、日期和模拟的绿色进度条
dv.paragraph(`今天是 ${formattedDate}，是今年的第 ${dayOfYear} 天。`);
dv.paragraph(`Year Progress: ${progress.toFixed(2)}%`);

// 创建绿色的自定义进度条
dv.paragraph(`
<div style="background-color: #e0e0e0; width: 100%; height: 20px; border-radius: 10px;">
    <div style="background-color: green; width: ${progress.toFixed(2)}%; height: 100%; border-radius: 10px;"></div>
</div>
`);

```
```dataviewjs
let ftMd = dv.pages("").file.sort(t => t.cday)[0]
let total = parseInt([new Date() - ftMd.ctime] / (60*60*24*1000))
let totalDays = " 您已使用 *Obsidian* "+total+" 天，"
let nofold = '!"misc/templates"'
let allFile = dv.pages(nofold).file
let totalMd = "共创建 "+
	allFile.length+" 篇笔记"
let totalTag = allFile.etags.distinct().length+" 个标签"

dv.paragraph(
	totalDays+totalMd+"、"+totalTag+""
)
```
📚博文阅读在这里：[[Blog Overview]]
📁TODO仓库在这里：[[TODO repository]]
📝**最近更新**文档：
```dataview
table file.mtime as "Update Time" from "" 
where file.name != "Welcome"
sort file.mtime desc limit 8
```
📝**todo**文档：
```dataview 
table file.mtime as "Update Time" from #todo
sort file.mtime desc
```
### ⏳正在进行

| 标签       | 事项                                                         | DDL            | 进度    | 资源                                       |
| -------- | ---------------------------------------------------------- | -------------- | ----- | ---------------------------------------- |
| #学习      | [机器学习吴恩达](https://www.coursera.org/learn/machine-learning) | [[2024-09-15]] | 0     |                                          |
| #reading | 机器学习周志华                                                    |                |       |                                          |
|          | [[leetcode刷题记录]]                                           | 每天一小时          |       | [题单](https://huxulm.github.io/lc-rating) |
| #proj    | [[easyvidshare]]                                           | 2024-8-31      | MVP完成 |                                          |
### ⏰将要进行

| 标签    | 事项                                                                |
| ----- | ----------------------------------------------------------------- |
| #学习   | 🌹                                                                |
|       | [深度学习吴恩达](https://www.coursera.org/specializations/deep-learning) |
| #proj | cuan-机器学习？                                                        |
|       | temp-email                                                        |
|       | chatbox                                                           |
暂缓进行
学习：
* [[Javascript基础学习]] ---基本学完，除了网络还有前面有一些没看
* #cs61a  --- 还有后面两个星期没看


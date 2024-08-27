📅今天是`=dateformat(date(today),"DD")`，`=date(today).year` 年已经过去了 `=(date(today)-date(date(today).year + "-01-01")).days` 天，约占`=floor((date(today) - date(date(today).year + "-01-01")).days / 365 * 100) + "%"`
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
📁等待被学习的代码仓库在这里：[[TODO repository]]
📝**最近**文档：
```dataview
table file.ctime as "Creation Time" from "" sort file.ctime desc limit 5
```
📝**todo**文档：
```dataview 
table file.ctime as "Creation Time" from #todo
sort file.ctime desc
```
### ⏳正在进行

| 标签    | 事项                 | DDL       | 进度  | 资源                                                   |
| ----- | ------------------ | --------- | --- | ---------------------------------------------------- |
| #学习   | [[Javascript基础学习]] | 2024-8-31 | 30% | [js现代教程](https://zh.javascript.info/object-copy?map) |
|       | #cs61a             | 2024-8-29 | 65% |                                                      |
|       | [[leetcode刷题记录]]   | 每天一小时     |     | [题单](https://huxulm.github.io/lc-rating<br>)         |
| #proj | [[easyvidshare]]   | 2024-8-31 | 20% |                                                      |
### ⏰将要进行

| 标签    | 事项                                                                |
| ----- | ----------------------------------------------------------------- |
| #学习   | [机器学习吴恩达](https://www.coursera.org/learn/machine-learning)        |
|       | [深度学习吴恩达](https://www.coursera.org/specializations/deep-learning) |
| #proj | cuan-机器学习？                                                        |
|       | temp-email                                                        |
|       | chatbox                                                           |



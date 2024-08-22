---
banner: "![[nebula.jpg]]"
banner_y: 0.5
---
##### 📅今天是`=dateformat(date(today),"DD")`，`=date(today).year` 年已经过去了 `=(date(today)-date(date(today).year + "-01-01")).days` 天，约占`=floor((date(today) - date(date(today).year + "-01-01")).days / 365 * 100) + "%"`
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
### 正在进行

| 标签    | 事项               | DDL       | 进度  |
| ----- | ---------------- | --------- | --- |
| #学习   | [[Javascript]]学习 | 2024-8-31 | 10% |
|       | #cs61a           | 2024-8-29 | 55% |
| #proj | [[easyvidshare]] | 2024-8-31 | 20% |

### ⏰将要进行

| 标签    | 事项                                                                |
| ----- | ----------------------------------------------------------------- |
| #学习   | [机器学习吴恩达](https://www.coursera.org/learn/machine-learning)        |
|       | [深度学习吴恩达](https://www.coursera.org/specializations/deep-learning) |
| #proj | cuan                                                              |





 
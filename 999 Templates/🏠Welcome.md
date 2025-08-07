```dataview
TABLE file.mtime as "最后更新时间"
FROM ""
WHERE file.name != "🏠Welcome"
SORT file.mtime desc
LIMIT 8 
```
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
```contributionGraph
title: Contributions
graphType: default
dateRangeValue: 360
dateRangeType: LATEST_DAYS
startOfWeek: 1
showCellRuleIndicators: true
titleStyle:
  textAlign: center
  fontSize: 15px
  fontWeight: normal
dataSource:
  type: PAGE
  value: ""
  dateField: {}
fillTheScreen: false
enableMainContainerShadow: false
cellStyleRules:
  - id: default_b
    color: "#9be9a8"
    min: 1
    max: 2
  - id: default_c
    color: "#40c463"
    min: 2
    max: 5
  - id: default_d
    color: "#30a14e"
    min: 5
    max: 10
  - id: default_e
    color: "#216e39"
    min: 10
    max: 999
cellStyle:
  borderRadius: ""
  minHeight: 9px

```
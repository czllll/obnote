
| domain | layer | type  |
| ------ | ----- | ----- |
| #db    | #es   | #fact |


## 数据结构

### 基本数据类型

* 文本
  * `text`: 用于全文搜索；支持分词和倒排索引
    * 标题
  * `keyword`:用于精确匹配，不参与分词
    * 状态值，分类
* 数值
  * 整型
    * integer 32
    * long 64
  * 浮点型
    * float 单精度
    * double
* 布尔
  * boolean
* 日期
  * `date`
    * 默认格式strict_date_optional_time`，例如 `2024-11-16T15:30:00Z

### 复杂数据类型

* 对象类型

  * `object` 存JSON对象，可以直接访问其对象。将数组中的每个对象存储为一个独立的子文档

  

* 嵌套类型
  * `nested`: 存数组中的复杂对象



### 地理数据类型

* 地理点类型
  * `geo_point`: 存经纬度
* 地理形状类型
  * `geo_shape`:存地理图形

### 特殊数据类型

* ip地址
  * `ip`: v4 或 v6
* 范围类型
  * `range`: 存储数值，日期或IP的范围
  * 包括：`integer_range`、`float_range`、`long_range`、`date_range`、`ip_range`




## 我们需要es干什么？

* 多维度地搜索笔记/用户
  * 关键词，相关笔记
  * 按条件搜索
    * 规则排序
    * 笔记类型
    * 发布时间
  * 关键词，相关用户

## 同步方式

* 全量同步
  * logstash
  * 过程：
    * 构建笔记/用户es索引
    * 使用logstash-input-jdbc插件从mysql构建es索引
* 增量同步
  * MQ通知
  * Canal
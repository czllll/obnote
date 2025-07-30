#java #jdk #todo
# 演进
* 总体上`Arrays.sort()`方法对基本数据类型和包装类型采取不同的排序策略。
* JDK6及更早，Java对基本数据类型使用经典单轴快排，对对象数组使用的是归并排序
* JDK7对基本数据类型引入了双轴快排，对对象数组使用的是TimSort（归并 + 插排）
* JDK14对sort进行了重构，对具体的排序策略进行了一定优化
## JDK6及更早
## JDK14
* https://bugs.openjdk.org/browse/JDK-8226297
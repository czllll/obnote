
| domain | layer | type  |
| ------ | ----- | ----- |
| #ai    |       | #fact |

* 将一个任务上学到的知识迁移到另一个相关任务上。
* 分类：
	* same domain, different task
	* different domain, same task
	* PS
		* domain: 领域 输入数据的分布和特征空间
		* task：任务 模型要完成的目标，比如图像分类vs图像检测
## 微调fine tuning 
* 微调是迁移学习的主要形式
* 三种主要策略
	* Train entire model
	* freeze some layers
	* freeze all base layers, only train FC
* 根据数据集相似性和数据集大小来进行判断进行冻结还是训练
	* 底层特征：通用性强，更容易被冻结
	* 高层特征：专用性强，更易训练
	* 数据越多，任务越新
#system-design 
* 问题
	* 用户刷朋友圈，如果有新的朋友发了朋友圈，那么缓存中却不是最新的，如何解决？
		* 用户是否在线？
			* 不在线：每隔五分钟更新缓存
			* 在线：给通知，然后用户重新fetch
* fanout机制
	* pull
		* 好处：僵尸粉不会占用系统资源，没有明星hotkey issue
		* 坏处：
			* 不刷新就还是旧数据
			* 没有那么多post，一直刷新就会返回null 占用资源
	* push
		* 帖子写出来马上生成feed
		* 需要long polling/ ws
		* 好处：不用自己fetch
		* 坏处：hotkey

* 在项目中的设计
	* 项目DAU很小，目前使用pull模型，直接从db捞数据；改成push模型
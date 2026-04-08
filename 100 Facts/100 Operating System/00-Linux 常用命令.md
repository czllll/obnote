
* netstat -nap
	* n 代表数字 标识使用数字标识和端口，避免解析域名造成的延迟
	* a 代表所有链接和监听端口，默认只有 established 状态的，不显示 listen 的
	* -p 代表进程信息，显示每个连接的 PID
* top 
* 磁盘 df -h
	* h 代表 human 可读
* rm
	* -f 代表强制删除
	* -r 代表循环
	* rmdir 删除目录
* find 查找命令
	* `find [目录] [参数]`
	* -name 按名称
	* -name 不区分大小写
	* -type 按类型
		* -f 表示文件
		* -d 表示目录
* ls -la
	* -l 表示使用长格式输出，包括权限，所有者，文件大小，修改时间等详细信息
	* -a 显示所有文件

## 文本操作
* 输入重定向
	* `echo Hello World > puppy.txt`
		* 输出重定向到文件里，`>`覆盖
	* `echo Hello World >> puppy.txt`
		* `>>`追加
* 输出重定向
	* `cat < puppy.txt > banana.txt`
* 管道
	* 使用`tee`命令输出写入两个不同的流
* `cut`
	* -f 表示分割符
	* -c 表示第几个字符
	* -d 指定字段分割符号
	* `cut -f 1 -d ";" sample.txt` 即输出以；分割的第1 个字段
* paste
	* `-s` 将每一行的内容串联为一行
* head 
	* `-n`行数
	* `head -n 15 /var/log/syslog`
* tail
	* `-n`行数
	* `-f` 代表 follow ，实时新增
* join
	* 通过一个公共字段将多个文件连接在一起
	* `join -1 2 -2 1 file1.txt file2.txt`
	* -1 代表第一个文件
	* -2 代表第二个文件
* tr
	* `tr`小写转大写
* uniq 删除重复项
	* -c 每一样出现次数
	* -u 只获取唯一值
	* -d 只获取重复值
	* 限制：只有在重复行相邻时才会检测到，可以使用 sort 解决
* wc
	* 分别显示行数，单词数和字节数
	* 分别是-l -w -c
* grep
	* -i 不区分大小写

## VIM
 * dw 
	 * 从光标出删除到下一个单词开头
	 * 2dw 删除两个单词
 * ZZ = :wq

## 用户管理
* su
	* 切换用户，默认切换 root
	* `su czl`
	* `su -` 切换到 root 并重新加载root 的环境配置
* useradd
* userdel
* passwd \[用户]

## 权限
分四部分
`d | rwx | r-x | r-x`
* 第一部分代表文件类型
	* d 目录
	* - 普通文件，可能是文本/二进制/图像文件
	* l 符号连接，是纸箱另一个文件的快捷方式
	* s 套接字，表示一个 socket 用于进程间通信
* 其他部分
	* 前三位用户权限， 中间组权限，最后是其他权限（其他用户 所有人）
	* r 可读
	* w 可写
	* x 可执行
	* s 当一个文件设置了这个权限时，允许启动该程序的用户获得文件所有者的权限以及执行权限
		* `sudo chmod u+s myfile`
		* `sudo chmod 4755 myfile` SUID 用 4 表示
		* `sudo chmod g+s myfile`
		* `sudo chmod 2555 myfile`
	* -空
* chmod
	* 字母修改
		* 给 u 用户，g 用户组，o 其他用户+-权限
		* `chmod u+x myFile`
		* `chmod ug+x myFile`
	*  数字修改
		* 4: 读取权限
		- 2: 写入权限
		- 1: 执行权限
* chown 修改所有者
	* `sudo chown patty myfile`
	* `sudo chown patty:whales myfile` 同时修改所有者/组
* chgrp 修改所有组
	* `sudo chgrp whales myfile`
* umask 更改创建文件的默认权限
	* umask 021 就是不给什么权限
		* 给用户所有的
		* 不给用户组写入
		* 不给其他用户执行
* 粘滞位
	* 只有所有者或者用户才能删除或修改文件
	* `drwxrwxrwx+t`
	* `sudo chmod +t mydir `
	* `sudo chmod 1755 mydir`

## Reference
[Linux Journey: 免费 Linux 教程与课程，学习 Linux](https://labex.io/zh/linuxjourney)
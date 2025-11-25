---
tags:
- python
- blog-reading insight
---
## 要点
* 虚拟环境
	* 运行`virtualenv myenv`会复制一个新的 Python 解释器到`myenv/bin`下，
	* 并创建好`myenv/lib`，`myenv/lib/pythonX.Y/site-packages`等目录（`venv`模块不是用的复制，但结果基本一样）
	* 执行`source myenv/bin/activate`以后会把`myenv/bin`塞到`PATH`前面，让这个复制出来的 Python 解释器最优先被搜索到。
	* 这样，后续安装包时，`$path_prefix`就会是`myenv`了，从而实现了安装路径的隔离。
* python找包
	* 当导入模块时，python会按照如下顺序搜索模块即查找`sys.path`
		* 当前目录
		* 环境变量 pythonpath 目录
		* python 标准库目录
	* 举例：
```python
/test
├── main.py         
└── proj/
	├── __init__.py # 使 proj 成为一个包     
    ├── a.py        
    └── b.py        

# main.py 
from proj import b

# b.py
import a

# a.py
print ("a")
```

* **直接**运行python文件
	* 当运行`python proj/b.py`时，当前目录`/proj`会添加到`sys.path`中，则可以找到a.py不会报错
	* 当运行 `python main.py`,当前目录`/test`会添加到`sys.path`中,`/test`并无a.py文件，所以报错`ModuleNotFoundError: No module named 'a'`
* 使用**模块**方式运行
	* `python -m proj.b`，会将`/proj`目录添加到`sys.path`中,找不到a.py，所以会报错`ModuleNotFoundError: No module named 'a'`
	* 将b.py中的导入语句换成`from proj import a`,Python 会根据模块的包结构来查找 `a.py`。在这种情况下，`proj` 是一个包，Python 会在 `sys.path` 中查找 `proj` 目录,所以不会报错
## 思考


# Reference
[[你的 Python 包都装到哪了？]]
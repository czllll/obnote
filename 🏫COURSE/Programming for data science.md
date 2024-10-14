# Discussion
>Conduct your own research to find out more details of the work of Babbage and Lovelace.
  * What other details can you find out about the development of the Analytical Engine and the program Lovelace wrote?
* About Analytical Engine:
	* The major innovation was that the Analytical Engine was to be programmed using punched cards, which gave the machine the ability to use the results of preceding computations as input.
	* This machine also was designed to employ some features which were used in modern computers like sequential control, branching and looping.
* About the program Lovelace wrote:
	* After Babbage invented the Analytical Engine, Ada Lovelace translated an article about it. She appended a set of notes which included a algorithm for calculating a sequence of Bernoulli numbers using the Analytical Engine. Because of her work on using the machine for calculation, she is considered by many to be the first computer programmer.
>Conduct you own research to find out more details of early computing machines and of Alan Turing’s life and work.
  * What other examples of early computing machines can you find? What other achievements is Alan Turing famous for?
* examples:
	* Z1: The **Z1** was a motor-driven mechanical computer designed by German inventor Konrad Zuse from 1936 to 1937. It was a binary, electrically driven, mechanical calculator, with limited programmability, reading instructions from punched celluloid film. It employed Boolean logic for computations, which is fundamental to modern computers.
	* Atanasoff–Berry computer (ABC): It was the first automatic electronic digital computer. Its unique contribution was to make computing faster by being the first to use vacuum tubes to do the arithmetic calculations.
* achievements of Turing:
	* Turing test: It is a test of a machine's ability to exhibit intelligent behavior equivalent to, or indistinguishable from, that of a human.
	* Automatic Computing Engine (ACE): It was a British early electronic serial stored-program computer design by Alan Turing.
>Our general definition characterised a program as a "collection of commands". Consider the following:
  Do all programs consist of commands?
  Can definitions, questions or hypotheses be parts of a program?
  What about data? Is that part of a program?

1. I do not think so. Some declarative languages like sql or logic languages like prolog just focus on describing what should be done.
2. I think yes. Definitions are already in the modern programming languages like in C or Python. In my view, the conditional statement like if ... else and switch is actually questions and hypothesis.
3. In my view, external data like the data set in machine learning is not typically considered part of the program. However, the definition of the data structure and embedded data like the initial value which the program cannot run without are parts of a program.


>Although the effect of running the code of either **Example 1** or **Example 2** is the same, most programmers would find **Example 2** far less appealing. Why is this?

* Because the variable names of example 1 are more intuitive than those in example 2. The variable names in Example 2 lack clear meaning and are difficult to understand. If we obey the same variable naming rules, the situation in example2 basically will be not appear. 


## outline
### 2. the role of data in computer programming
* function
* OOP
* manipulating in files
* the development of data structure and data

### 文件操作
#### 常规操作
*  `f = open('my_example_file.txt', 'x')  # 创建文件`
	* x 排他性的创建文件，若文件存在则会引发错误
#### csv库
* csv.reader() 读取
```python
with open('names.csv', 'r') as f:
    reader = csv.reader(f, delimiter=',')
    
    i = 0
    for row in reader:
        print(row)
        
        i += 1
        if i == 10:
            break
```
* csv.writer()
```python
with open('output.csv', 'w') as write_file:
    csv_writer = csv.writer(write_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)

```
* quotechar-- 指定CSV文件中包裹字段的字符，通常包含特殊字符（换行符或分隔符）
* quoting=csv.QUOTE_MINIMAL --控制哪些字段需要用quotechar包裹，这个值表示仅在必要时才添加，即特殊字符才添加
	* `csv.QUOTE_ALL`：所有字段都会被引号包裹。
	- `csv.QUOTE_MINIMAL`：仅在需要时（例如字段包含特殊字符时）才包裹。
	- `csv.QUOTE_NONNUMERIC`：所有非数字的字段都会被引号包裹。
	- `csv.QUOTE_NONE`：不使用引号包裹字段，甚至会抛出错误，要求避免任何字段中的特殊字符。
* csv.DictReader(f) 转python字典
```python
with open('passwords.csv', mode='r') as f:
    dict_reader = csv.DictReader(f)
    for row in dict_reader:
        print(row)
```
* csv.DictWrriter(f, fildnames = headers)
	* header 是 自定义的头部 通常是可迭代对象，列表或tuple

### matplotlib.pyplot
```python
# 使用 matplotlib 绘制散点图
# gdppc 是 x 轴数据 (GDP 百分比变化)，mmpc 是 y 轴数据 (孕产妇死亡率百分比变化)
# alpha=0.5 表示点的透明度，0 为完全透明，1 为完全不透明
# 前两个参数是可迭代的数据类型
plt.scatter(gdppc, mmpc, alpha=0.5)

# 设置 y 轴标签为 'Maternal Mortality % Change'
plt.ylabel('Maternal Mortality % Change')

# 设置 x 轴标签为 'GDP % Change'
plt.xlabel('GDP % Change')

# 设置图表的标题为 'Maternal Mortality vs GDP Change'
plt.title('Maternal Mortality vs GDP Change')

# 显示绘制的图表
plt.show()


```
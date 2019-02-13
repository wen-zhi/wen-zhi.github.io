---
layout: post
title: 文之的 Python Notes
mathjax: true
comments: true
excerpt: >- 
  记录日常使用 Python 中的一些思考与知识点。
---

    - 记录日常使用 Python 中的一些思考以及知识点
    - 最好加入代码示例
    - 尽量简洁
    - 早期条目较少的时候不必进行分类

* TOC
{:toc}

### 1. 队列类型 `deque`

`collections` 库中有一个队列结构 `deque`，比使用 `list` 作为队列更方便。

```python
from collections import deque
queue = deque(["Eric", "John", "Michael"])
queue.append("Terry")           # Terry arrives
queue.append("Graham")          # Graham arrives

>>> queue.popleft()
'Eric'
>>> queue
deque(["John", "Michael", "Terry", "Graham"])
```	

### 2. 序列的判空操作
	
判断一个队列为空，可以直接使用 `if queue:`。判断 Sequence 类型的数据结构都可以这么做。

[不推荐使用](https://www.python.org/dev/peps/pep-0008/#programming-recommendations) `if len(queue)`。 

### 3. Python 中的数字类型

Python 中并没有 `int32`, `float32` 这样的数字类型……

也不像 C 语言中的数字类型那么繁杂，比如只一个整型就包括 `int`, `long int`, `long long int`...

它只有**整型**、**浮点型**、**复数**这三种数字类型（这里不考虑标准库和第三方库中的数字类型），即 `int`, `float`, `complex` 。其中 `int` 型是无限精度的，`float` 型使用 C 语言中的 `double` 型来实现。这种设计思路还是非常优雅的。

如果想要严格控制数字类型所占用的字节数，可以使用 `NumPy` 这样的第三方库。

### 4. 内存上更加高效的生成器表达式

生成器表达式 (Generator Expressions) 和列表理解式 (List Comprehensions) 的语法几乎一致，只不过前者使用的是括号：

```python
iterator = (i for i in range(3))
```

也因此可以直接作为函数的参数，句法上更加简洁，并且比列表理解式在[内存的使用上更加高效](https://dbader.org/blog/python-generator-expressions)：

```python
sum(i*i for i in range(10)) 
```

因为它是一次性的，迭代一遍后，就无法重头再来：

```python
>>> iterator = (i for i in range(4))
>>> next(iterator) # 使用 next() 函数获取 iterator 中的值
0
>>> next(iterator)
1
>>> for i in iterator:
	print(i)
2
3
```

### 5. 使用 `max(), min()` 将取值限定在一定区间内

有时我们想把取值结果限定在一个取值区间 `[MIN, MAX]` 内，如果使用判断语句来实现这一功能会比较啰嗦：

```python
if res > MAX:
    res = MAX
if res < MIN:
    res = MIN
```

嵌套使用 `max()` 和 `min()` 则更加 Pythonic：

```python
res = max(MIN, min(MAX, res))
```

或许看上去有些 tricky，但实际上很简单：`min()` 限定结果小于 `MAX`，`max()` 限定结果大于 `MIN`。即可将 `min` 看作**小于**号，`max` 看作**大于**号。这样就可以直接将上行代码读作：`res` 小于 `MAX`，大于 `MIN`。这恰好就是对 `res` 的取值范围的定义。

当然 `max()` 也可以写在里层：

```python
res = min(MAX, max(MIN, res))
```

### 6. 便于统计频率的 `Counter` 类

统计一个东西的出现频率（比如统计一个文本中每个单词的词频），是非常常见的任务。Python 在标准库 `collections` 中专门定义了一个字典 (dict) 的子类 `Counter` 来处理这类事情。

```python
>>> from collections import Counter
>>> Counter('hello') # 从一个 iterator 中初始化
Counter({'h': 1, 'e': 1, 'l': 2, 'o': 1})
>>> Counter(['You', 'are', 'are', 'beautiful']) # 从一个 iterator 中初始化
Counter({'You': 1, 'are': 2, 'beautiful': 1})
```

它就是一个特殊的字典类，特殊体现在键值对中的值 (value) 默认是数值类型、除了继承自字典的方法外还有一些独有的面向频率统计的方法 (method) 、同时也是一个多集 (multiset) ，支持一些集合操作等等。具体细节可翻阅[官方文档](https://docs.python.org/3/library/collections.html#collections.Counter)。

<br>
--EOF--
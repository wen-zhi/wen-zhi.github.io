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

{: class="table-of-content"}
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
	
判断一个队列为空，可以直接使用 `if queue:`。判断 Sequence 类型的数据结构都可以这么做。[不推荐使用](https://www.python.org/dev/peps/pep-0008/#programming-recommendations) `if len(queue)`。 

### 3. Python 中的数字类型

Python 中并没有 `int32`, `float32` 这样的数字类型……

也不像 C 语言中的数字类型那么繁杂，比如只一个整型就包括 `int`, `long int`, `long long int`...

它只有**整型**、**浮点型**、**复数**这三种数字类型（不包括标准库和第三方库中的数字类型），即 `int`, `float`, `complex` 。其中 `int` 型是无限精度的，`float` 型使用 C 语言中的 `double` 型来实现。这种设计思路还是非常优雅的。

如果想要严格控制所占用的字节数的数字类型，可以使用 `NumPy` 这样的第三方库。

### 4. 内存上更加高效的生成器表达式

生成器表达式 (Generator Expressions) 和列表理解式 (List Comprehensions) 的语法几乎一致，只不过使用的是括号：

```python
iterator = (i for i in range(3))
```

也因此可以直接作为函数的参数，句法上更加简洁，比列表理解式在[内存上更加高效](https://dbader.org/blog/python-generator-expressions)：

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
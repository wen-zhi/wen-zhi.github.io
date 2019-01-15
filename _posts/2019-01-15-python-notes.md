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

1. 队列类型

	`collections` 库中有一个队列结构 `deque`，比使用 `list` 作为队列更方便。

	```
	from collections import deque
	queue = deque(["Eric", "John", "Michael"])
	queue.append("Terry")           # Terry arrives
	queue.append("Graham")          # Graham arrives

	>>> queue.popleft()
	'Eric'
	>>> queue
	deque(["John", "Michael", "Terry", "Graham"])
	```	

2. 判空操作
	
	判断一个队列为空，可以直接使用 `if queue:`。判断 Sequence 类型的数据结构都可以这么做。不推荐使用 `if len(queue)`。 

3. 数字类型

	Python 中并没有 `int32`, `float32` 这样的数字类型……它只有 `int`, `float` 这样的类型，其中 `int` 型是无限精度的，`float` 型则是 C 语言中的 `double` 型。这种设计思路还是非常优雅的。

	如果想要精准的数字类型，可以使用 `NumPy` 这样的第三方库。

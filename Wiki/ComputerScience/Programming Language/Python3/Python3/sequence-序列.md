# sequence(序列)

> 一个有序的集合，成员都是有序排列，并且可以通过下标偏移量访问它的一个或几个成员，也就是可以进行索引访问、索引切片；

- list：Built-in **mutable sequence**.
- tuple：Built-in **immutable sequence**.
- range：Built-in **immutable sequence of numbers**

Python3 通过 C 实现了丰富的序列类型：

容器序列存放的是它们所包含的任意类型的对象的引用，而扁平序列
里存放的是值而不是引用。换句话说，扁平序列其实是一段连续的内存
空间。由此可见扁平序列其实更加紧凑，但是它里面只能存放诸如字
符、字节和数值这种基础类型；

序列分类

- 容器序列：更加灵活，
　　list、tuple 和 collections.deque 这些序列能存放不同类型的数据。
- 扁平序列：体积更小，速度更快
　　str、bytes、bytearray、memoryview 和 array.array，这类序列只能容纳一种类

- 可变序列
- 不可变序列

## Common Sequence Operations

序列的通用操作

### sequence 切片

s[i] --> s 的第 i 项，起始为 0
s[i:j] --> s 从 i 到 j 的切片
s[i:j:k] -->s 从 i 到 j 步长为 k 的切片

## sequence 拼接

`+` 和 `*` 都会重新产生一个 sequence ，不会对原有的 sequence 产生影响；

- 拼接不可变序列总是会生成新的对象。 这意味着通过重复拼接来构建序列的运行时开销将会基于序列总长度的乘方。 想要获得线性的运行时开销，你必须改用下列替代方案之一：

- 如果拼接 str 对象，你可以构建一个列表并在最后使用 str.join() 或是写入一个 io.StringIO 实例并在结束时获取它的值

- 如果拼接 bytes 对象，你可以类似地使用 bytes.join() 或 io.BytesIO，或者你也可以使用 bytearray 对象进行原地拼接。 bytearray 对象是可变的，并且具有高效的重分配机制

- 如果拼接 tuple 对象，请改为扩展 list 类


1. + 拼接
	+ 拼接的两边都需要是相同类型的数据，比如 list 和list ，str 和 str，如果是不同类型的，会报错 `TypeError`；

2. * 拼接

3. 创建多维列表（数组）
	**这个很重要**
python-3.8.5-docs-html/python-3.8.5-docs-html/faq/programming.html#faq-multidimensional-list

## sequence 元素判断
1. in
	x in s -- >如果 s 中的某项等于 x 则结果为 True，否则为 False
	```python
	a = 'test'
	if 't' in a:
    	print('True')
	```

2. not in
	x not in s -- >如果 s 中的某项等于 x 则结果为 False，否则为 True
	```python
	a = 'test'
	if 't' not in a:
    	print('False')
	```
	
## sequence 其他通用操作
- len(s) --> s 的长度
- min(s) --> s 的最小项
- max(s) --> s 的最大项
- s.index(x[, i[, j]]) --> x 在 s 中首次出现项的索引号（索引号在 i 或其后且在 j 之前）,j超过 sequence 长度，也不会报错的
	- 这个操作相当于 s[i:j].index(x)，如果没有出现会出现 Valueerror 错误；
- s.count(x) -->x 在 s 中出现的总次数




## 工厂函数

虽然他们看上去有点像函数，实质上他们是类。当你调用它们时，实际上是生成了该类型的一个实例，就像工厂生产货物一样。

list(),tuple(),str() 这些都是工厂函数，
```
a = tuple(i for i in range(5))
print(type(a))  # tuple

b = list(a)  # 传入的是 tuple type，通过 list(),变成了 list type
print(type(b))  # list
```



## 关于序列的关系

![Sequence_relation](Sequence_relation.png)



## 推导式（comprehension）

### 1. list comprehension
列表推导式提供了一个更简单的创建列表的方法。常见的用法是把某种操作应用于序列或可迭代对象的每个元素上，然后使用其结果来创建列表，或者通过满足某些特定条件元素（判断，if 一句，比如 if > 0 ）来创建子序列。

> list comprehension 本质上就是 for 循环表达式，只是比 for 循环表达式更加简洁和易读；


`i for i in range(5)`不存在变量泄漏的问题，i 还能表达原来的变量，不会把 i 变成了 5；

通常的原则是，只用列表推导来创建新的列表，并且尽量保持简短。如果列表推导的代码超过了两行，你可能就要考虑是不是得用 for 循环重写了；


### 2. tuple comprehension


### 3. set comprehension

### 4. dict comprehension


## 生成器表达式（generator expression）




## range function

range() function 返回一个 range object，这个对象会生产整数的序列。

```python
# Return an object that produces a sequence of integers from start (inclusive) to stop (exclusive) by step. 
```






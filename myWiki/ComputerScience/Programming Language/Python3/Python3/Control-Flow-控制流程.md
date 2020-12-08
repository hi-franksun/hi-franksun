# control flow

控制流程几乎是每种编程语言里很重要的部分，也是靠着流程控制才能实现了大多数的算法，绝大多数的业务代码都会涉及到这部分，这也是基本功的一部分，可以说写好流程控制才能写出好的代码；

## 控制流程一般分成三类

1. 条件语句
Python 中只提供了 if 语句，没有 switch/case 语句，但是提供了 elif(else if缩写)，来进行多种状态的判断，避免了大量的缩进 ；
2. 循环语句
Python 中提供了 while 和 for 两种循环，for 循环不同于其他变成语言，使用更加方便一些，多数情况下，for 起到了遍历的作用，而不是简单的循环；
3. 中断控制流程语句
break 和 contiue 语句，完善了流程控制中一些特殊情况；

## statements 分成两类

1. simple statements

2. compound statements

> 英语：
expression: 表达式
statements: 语句、声明、陈述、说法
---

## 1.1. if statements(if 语句)

`if expression: suite
(elif expression: suite)*
else:`
注意 else 后边是没有 expressions 的，只是最后一种可能性；

```python
a = 2
if a == 0:
    print(f'{a} == 0')
elif a == 1:
    print(f'{a} == 1')
elif a == 2:
    print(f'{a} == 2')
else:
    print(f'{a} == 3')
```

### 判断条件省略用法

不推荐这种用法，条件判断应该是显性的，而不是隐性的，这是一种规范；

| 数据类型 | 结果 |
| -- | --- |
| string | 空字符串解析为 False |
| int | 0 解析为 False |
| boolean | Flase 解析为 False |
| list/tuple/dict/set | iterable 为空解析为 False |
| Object | None 解析为 False |


## 1.2. while statements(while 语句)

### 1.2.1. while

`while expression：statements`

当 expression 为 true 时候，会执行 statement 内容，一旦 expression 为 false 时候，就会终止这个 loop。

```python
i = 0
while i < 5:
    print(i)
    i += 1
# 0
# 1
# 2
# 3
# 4
```

### 1.2.2. while else

`while expression：statements else: statements`

当 experssion 为 false 时候，会执行 else statements。

```python
i = 0
while i < 5:
    print(i)
    i += 1
else:
    print(f'{i}, while is ending')
# 0
# 1
# 2
# 3
# 4
# 5, while is ending
```

---

## 1.3. for statements(for 语句)

### 1.3.1 for

`for target_list in expression_list: suit`

```python
a = [1, 2, 3, 4, 5]
for i in a:
    print(i)
# 1
# 2
# 3
# 4
# 5
```

### 1.3.2 for else

`for target_list in expression_list: suit
else: suit`

>else 子语句其实属于整个 for statements 中，也就是符合条件会一直执行的，但是 break 和 continue 使用时候要注意，不要忽略这个现象，else 是一体的

expression_list 会被创建成一个迭代器，然后每个迭代的的项都会赋值给 target_list，然后子语句进行执行，直到迭代器迭代完成，比如序列为空、迭代器引发 Stopiteration 异常，结束 for 语句，这个时候如果 else statement 为真时候，会继续执行 else，执行完整个 for statements 结束；

```python
a = [1, 2, 3, 4, 5]
for i in a:
    print(i)
else:
    print('for loop is end')
# 1
# 2
# 3
# 4
# 5
# for loop is end
```

---

## 1.4. break and contiue statements(break 和 continue 语句)

### break continue in while

break 在 while 语句中，如果 break 执行了，则会终止 while 语句，**同时如果存在 else，也不会执行 else；**

```python
i = 0
while i < 5:
    print(i)
    i += 1
    break
# 0
```

```python
a = 0
while a < 3:
    print(a)
    a += 1
    break
else:
    print(a)
# 0
```

continue 在 while 符合语句中，不会执行她后边的语句，**但是会执行 else；**

```python
a = 0
while a < 3:
    print(a)
    a += 1
    continue
    print('this is statements after continue')
else:
    print(a)
# 0
# 1
# 2
# 3
```

### break continue in for

```python
a = [1, 2, 3, 4, 5]
for i in a:
    if i == 4:
        break  # break 之后会跳出循环，同时 else 也不会执行的；
    print(i)
else:
    print('for loop is end')

# 1
# 2
# 3
```

```python
a = [1, 2, 3, 4, 5]
for i in a:
    if i == 4:
        continue  # a==4，不会执行print(4)，但是还是会继续循环，同时 else 还是会执行的；
    print(i)
else:
    print('for loop is end')
# 1
# 2
# 3
# 5
# for loop is end
```

---

## 1.5. pass statements(pass 语句)

place-holder：占位符

属于语法糖，通常在 class/function/statements 中表示占位，主要用来保持抽象能力，不会因为这一部分没写，没法继续抽象，可以先实现更高层的代码；

## 1.6 技巧

- 减少循环次数，善用 break 和 continue 减少循环次数；
- 循环的 for 和 while 选择，在已知循环的对象时候，使用 for 更加简洁，但是不知道的时候，或者没有规律的时候，推荐使用 while，while 循环中一定会涉及到计数器，也就是类似 `i += 1`，类似这种，很多时候，可以采用 range() 函数配合 for loop 来增加性能，range() 是 c 写的，速度要快一些；
- 一句话的循环 + 判断形式，看起来很简洁，但是在工程中不一定合适，规范很重要，可以顶个规范，一层嵌套的可以写，但是牵扯到两层了，还是用常规的办法；
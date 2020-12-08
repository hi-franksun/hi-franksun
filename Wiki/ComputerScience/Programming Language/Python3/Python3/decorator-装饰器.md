# decorator-装饰器

装饰器是在 python 中使用最多的进阶知识，modelues and framework 中大量使用了装饰器，所以不了解装饰器很难读懂源码，哪怕 coding 中我们不使用，但是这么好的一种设计，为什么不使用？


## 装饰器的定义
装饰器是可调用的对象，其参数是另一个函数（被装饰的函数）。 装饰器可能会处理被装饰的函数，然后把它返回，或者将其替换成另一个函数或可调用对象。

装饰器只是语法糖。装饰器可以像常规的可调用对象那样调用，其参数是另一个函数，装饰器提供一种很好的 AOP 设计模式，也就是面向切面编程；

## 基础装饰器的实现

```python
import time
# 第一种方式，函数调用
def wrapper():
    print("this is function wrapper")

def decorator(func):
    print(time.time())
    func()

decorator(wrapper)
# 1602149516.932965
# this is function wrapper

# 第二种方式，闭包的实现(也就是装饰器模式)
def decorator(func):
    def wrapper():
        print(time.time())
        func()
    return wrapper


def wrapper_test():
    print("this is function wrapper_test")

result = decorator(wrapper_test)
result() # 这一步才是真正的调用

# 1602149731.143252
# this is function wrapper_test
```

上边第二种方式其实就是装饰了，只不过调用没有使用装饰这个语法糖，使用装饰器语法糖就可以了

```python
def decorator(func):
    def wrapper():
        print(time.time())
        func()
    return wrapper


@decorator
def wrapper_test():
    print("this is function wrapper_test")
    print("this is f1 function")

wrapper_test() # 上边的这个语法糖其实就是 decorator(wrapper_test)() 其实就是执行了 wrapper_test() 函数，只不过添加了一个打印时间的功能
```

## 带参数的装饰器


```python
# 一个参数
def decorator(func):
    def wrapper(name):
        print(time.time())
        func(name)
    return wrapper


@decorator
def wrapper_test(name):
    print("this is function wrapper_test")
    print(name)

wrapper_test('frank')
# 1602150262.5122933
# this is function wrapper_test
# frank

如果是多个参数
```

## 带自定义参数的装饰器


## 类装饰器


## 装饰器的嵌套


## 装饰器的使用案例

### 装饰器优点

1. 代码的稳定性：
    装饰器是我们对一个封装的单元(比如函数、类)进行修改，可以不去改动这个封装的具体实现，而通过装饰器的形式去改变这个封装单元的**行为**；


2. 代码的复用性：
   不修改代码的实现情况下，增加新功能，有很多函数都需要添新功能，那么这个功能必须抽象成一个函数或者类去复用，但是函数/类的功能又无法去实现这个要求，实现之后还必须调用，非常复杂，装饰器就完美解决了这个问题；


# Python3.X

## 内置数据类型

- [ ] [Development environment and tools](./Python3/Development-environment-and-tools)
- [ ] [string-字符串](./Python3/string-字符串)
- [ ] [binary-sequence-二进制序列](binary-sequence-二进制序列)
- [ ] [list-列表](./Python3/list-列表)
- [ ] [tuple-元组](./Python3/tuple-元组)
- [ ] [dict-字典](./Python3/dict-字典)
- [ ] [set-集合](./Python3/set-集合)
- [ ] [sequence-序列](./Python3/sequence-序列)
- [ ] [collections-容器数据类型](./Python3/collections-容器数据类型)

---

- [ ] [Control-Flow-控制流程](./Python3/Control-Flow-控制流程.md)
- [ ] [Looping-Notice-循环注意事项](./Python3/Looping-Notice-循环注意事项.md)
- [ ] [Errors-and-Exceptions-错误和异常](./Python3/Errors-and-Exceptions-错误和异常.md)
- [ ] [Built-in-Functions-内置函数1](./Python3/Built-in-Functions-内置函数1)
- [ ] [Built-in-Functions-内置函数2](./Python3/Built-in-Functions-内置函数2)
- [ ] [Defining-Functions-自定义函数](./Python3/Defining-Functions-自定义函数)
- [ ] [Lambda-Expressions-Lambda表达式](./Python3/Defining-Functions-自定义函数)
- [ ] [If-Statements-if语句](./Python3/control-flow-控制流)

## CPython3 实现

- [ ] [read-write-file-文件读写](./Python3/read-write-files-文件读写.md)
- [ ] [CPython3-list](./CPython/CPython3-list.md)
- [ ] [CPython3-tuple](./CPython/CPython3-tuple.md)
- [ ] [CPython3-dict](./CPython/CPython3-dict.md)

## 专题
- [ ] [slice-切片](./Python3/slice-切片.md)

## Standard Library

1. Text Processing Services(文本处理)
2. File and Directory Access(目录和文件访问)
    - [ ] [os](./Standard-Library/file-and-directory/os.md)
    - [ ] [shutil](./Standard-Library/file-and-directory/shutil.md)

## Web Application Framework

## The Zen of Python

The Zen of Python

```python

import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

```

## about Python

- 易于学习功能强大的编程语言
- 优雅的语法
- 提供高效的数据结构
- 支持面向对象
- 动态类型，开发速度快
- 跨平台写脚本和快速开发应用

1. 比脚本语言 shell 这些功能更加强大，且可以跨平台；比编译语言更加方便，因为是解释型语言，少了一些语法的操作和编译过程，并且提供了更加高级的数据结构和内置模块，并且再开发过程中可以使用解释器进行一些 demo，非常方便，总结就是：比脚本语言更加强大，比编译语言更加方便，快速；

2. Python 速度问题，解释型语言的通病，速度慢，官方好像没有计划去提升性能，可能和创始人有关，官方推荐使用 C 拓展的方式提升性能，并且编程语言其实不是速度的瓶颈，网络速度、IO 操作这些才是性能的瓶颈；（当然如果数据量很大，牵扯到高并发，Python 确实不适合，就必须使用性能更好的编程语言，但是世界上没有多少公司有这个数据量，初创公司更加讲究开发速度）


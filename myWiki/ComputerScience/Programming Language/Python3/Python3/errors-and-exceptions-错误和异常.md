# 错误和异常处理

invalid：无效
syntax：语法
except：除了，除非，除..之外
block：块
Traceback：追溯

异常 != 错误
异常是在出现错误时采取正常控制流之外的动作；
异常处理的一般流程是：检测到错误，引发异常；对异常进行捕获的操作；
编写程序时候，用户随意输入数值和一些非法字段，都会导致程序崩溃，无法执行下去；

> finally是一个异常处理后的清理工作，一般用来释放文件连接、网络连接，一般也就是和外界进行通讯的时候，用来最最后的处理工作，不管是不是执行都会进行最后的精力工作，保证代码的安全

```python
try:
    监控异常
except Exception[,reason]:
    <异常处理代码>
finally:
    <无论异常是否发生都会执行>
```

```python
try:
    year = int(input("import year:"))
    print(year)
except (ValueError,KeyError):
    print("请输入数字格式年份")
finally:
    print("输入的名字格式正确")
```

```python
try:
    year = int(input("import year:"))
    print(year)
# 打印出详细的异常信息
except ValueError as c:
    print("请输入数字格式年份，错误信息是{}".format(c))
finally:
    print("输入的名字格式正确")
```

## 常见错误

1. 内置语法错误
    - NameError：命名错误，比如未定义变量
    - SyntaxError：语法错误，写了一些不符合语言规范的东西，比如少加了引号括号之类；
    - IndexError：索引超出范围，序列索引到2，索引 4，会报错
    - AttributeError：属性错误，比如某个对象没有响应的方法和属性，数字没有 append 属性

2. 内置类型错误
    - ValueError：值错误，比如传入字符串，结果传入了数字

## 异常层次结构

```python
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning
```

## 未知错误

很多时候，我们不知道会遇到什么错误，所以，每次都写 except，很麻烦，可以放到一个元组里，这样子就能更加方便一些

```python
a = 'input your string'
b = []
try:
    a[1000]
    print(b)
except (IndexError, KeyError) as e:
    print(e)
finally:
    print('program is end')

# string index out of range
# program is end
```

except 元组中一般都是都是我们知道的异常，一般最后不知道的时候，会加一个总的异常处理，Exception，这个是所有错误的基类，理论上可以捕获所有的错误

```python
except Exception as err:
    print('Other error: {}'.format(err))
```

## 用户自定义异常

## 异常使用的场景和注意事项

异常处理时候，是程序在运行过程中遇到了错误，而退出程序，我们异常处理是为了让程序能正常的实行下去。

一般异常处理使用在无法确定的时候，比如连接数据库、读取文件返回数据、用户输入数据，很多时候，我们对这个异常没有先天知道，就要进行异常处理，也就是说异常处理更多的不是程序的本身，而是程序和外界的交互上；

正常的 flow-control 逻辑，不要使用异常处理，直接用条件语句解决就可以了。

一定要避免滥用使用异常；

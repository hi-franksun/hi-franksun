# string

string 是一个编程语言中最常见和最常用到的类型，日志的打印、程序的注释、数据库的访问、变量的增删改查操作...都会用到字符串；

## create string

python 中有三种表达方式：单引号、双引号、三引号，单引号和双引号一般表示 string，有两种方式是为了相互的嵌套表示；三引号一般用来类和函数的注释；

## escape character（转义字符）

编程语言中都支持转义字符，通过某种约定俗称的格式表现现实中的情况，比如换行、制表等，这种字符算是一个长度，**在用 len() 函数时候算一个**；

| 转义字符 | 说明 |
| -- | -- |
| \newline | 接下一行 |
| \n | 换行 |
| \t | 制表符 |
| \\ | 表示 \ |
| \' | 表示 ' |
| \" | 表示 " |
| \b | 退格 |
| \v | 纵向制表符 |

如果不希望转义字符影响原有字符，使用 r，忽略转义字符；

```python
print('C:\some\name')  # here \n means newline!
# C:\some
# ame
print(r'C:\some\name')  # note the r before the quote
# C:\some\name
```

## format string

> 将某种格式的字符串转化成另一种格式，推荐在 Python 中使用 f-string 方法进行格式化字符串

1. format

    ```python
    a = 'this is {}'.format('python3') # this is python3
    ```

2. f-string

    f-string 格式化字符串，以f 开头，字符串中的表达式用 {}，将变量和表达式结果填入 {}；

    ```python
    name = 'python3'
    a = f'this is {name}' # this is python3
    ```

3. %
    % 占位符，这个不推荐在 python 中使用，不优雅，了解一下即可，方便能看懂别人使用此方式写的源码

## string methods

1. str1 += str2 字符串拼接
    str 是 immutable 类型，所以是不能在内部进行修改的，也就是只能创建 new str，每次都会有较大的消耗；

    ```python
    a = 'python2'
    b = 'python3'

    a += b
    print(a) # python2python3
    print(b) # python3
    ```

2. str.join(iterable)-字符串拼接
    高效构建字符串，因为字符串是不可变对象，新的字符串只能手动创建，利用传入的 iterable 可以高效的创建字符串，很多时候，iterable 会用 list 代替。

    官方文档也有说明：
    > 拼接不可变序列总是会生成新的对象。 这意味着通过重复拼接来构建序列的运行时开销将会基于序列总长度的乘方。 想要获得线性的运行时开销，你必须改用下列替代方案之一：
    >如果拼接 `str` 对象，你可以构建一个列表并在最后使用 `str.join()`

    ```python
    a = ['python2', 'python3']
    a = ' '.join(a) # 注意使用 join 的时候，前面的字符很重要，一般会使用一个空格/其他符号进行分割
    print(a) # python2 python3
    ```

3. str.join(iterable) or str1 += str2
    使用哪个主要是性能问题，官方推荐  join ，同时因为 join 还能传入一个 str 更加灵活一点，但是一般需要构建 list 进行，尽量还是用官方推荐的吧，但是 str1 += str2 这种形式性能也不差，因为做了优化，不会在每个新字符串生成时候就去申请内存，而是做一个检测，官方优化了这个点，都可以使用；

4. str.split(separator, maxsplit)
    字符串的分割方法，这个非常常用，一般用来分隔日志文件、路径截取等等；maxaplit 表示分割的次数

    return list

    ```python
    a = '/home/test/home/date_txt'
    file_name = a.split('/')[4] # ['', 'home', 'test', 'home', 'date_txt']
    print(file_name) # date_txt

    file_name = a.split('/', maxsplit=2) # ['', 'home', 'test/home/date_txt']
    print(file_name[1]) # home
    ```

5. 字符串替换
    str 是不可变字符，字符串替换不会改变原来的字符，所以可以重新赋值新的变量进行接收。

    ```python
    str.replace(oldstr, newstr, [count]) # 替换新的字符和次数

    a = '/home/test/home/date_txt'
    b = a.replace('home', 'myhome')
    print(a)
    print(b)
    ```

6. 首尾字符串处理
    一般用在处理数据、日志文件中，快速的处理相关字符串，比如文件开头结尾都有空格，可以进行处理；

    ```python
    string.strip(str)，表示去掉首尾的 str 字符串；
    string.lstrip(str)，表示只去掉开头的 str 字符串；
    string.rstrip(str)，表示只去掉尾部的 str 字符串
    ```

7. 字符串判断
    判断字符串是必要的，以下函数判断字符串，并返回 bool 值；

    ```python
    str.isalnum() # 是否全是字母、数字
    str.isalpha() # 是否全是字母
    str.isdigit() # 是否全是数字
    str.islower() # 字母全是小写
    str.isupper() # 字母全是大写
    ......
    ```

8. count()
    统计字符串中某个字符出现的次数，可选参数是搜索的起始位置；

    return int

    ```python
    str.count(x:Text, start: 0, end: len(string))
    a = 'test'
    result1 = a.count('t') # 2
    result = a.count('t', 0, 1) # 1
    ```

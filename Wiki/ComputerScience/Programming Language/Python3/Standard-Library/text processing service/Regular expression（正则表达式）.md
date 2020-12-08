#Python3 


[Wikipedia 正则表达式 ](https://zh.wikipedia.org/wiki/正则表达式)
[正则表达式 JavaScript 在线练习](https://mzh.io/regex/)

## 正则表达式（Regular expression）

正则表达式是一个特殊的字符序列，是判断一个字符串是否是否于我们所设定的字符序列匹配；

可以实现一些检索文本，实现一个文本替换的操作，

检测一串数字是否是电话号码
检测一个字符串是否是邮箱
将文本中指定的单词替换成另一个单词

> re.findall("pattern(正则表达式)", "String(待匹配字符串)")

返回一个 List ，finall 是查找所有的匹配值组成一个列表，如果判断，可以加上 len()，判断 List 的长度

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
a = "C|C++|Python|Java|JavaScript"
result = a.index("Python") > -1
print(result)
result1 = re.findall("Python", a)
if len(result1) > 0:
    print("True")
```

## 原字符于普通字符
正则表达式就是由一些普通字符和原字符组成的

- 普通字符："Python"
- 元字符："/d"
- 普通字符和元字符混合："/dpython"

常用的元字符
/d 匹配一个数字字符 [0-9]
/D 匹配一个非数字字符 [^0-9]


找出所有的数字，可以直接使用 for 循环

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
b = "C0C++2Python3Java4JavaScript"
li = []
for i in a:
    if i.isdigit():
        li.append(i)
print(li)
```
但是不推荐这样的方式，性能和繁琐，不够直接，可以使用`正则表达式`

想要查找 0,1,2,3,4,5,6 这些数字，可以抽象出来进行处理
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
# \d:匹配数字
li1 = re.findall("\d",b)
print(li1)
# ['0', '2', '3', '4']

# \D:匹配非数字
li2 = re.findall("\D",b)
print(li2)
# ['C', 'C', '+', '+', 'P', 'y', 't', 'h', 'o', 'n', 'J', 'a', 'v', 'a', 'J', 'a', 'v', 'a', 'S', 'c', 'r', 'i', 'p', 't']
```

## 字符集

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import re
a = "abc,acc,adc,aec,afc,ahc"
```

想匹配出 `adc` 或者`afc`，就可以用 []，这种字符集进行筛选

[bf] 这里边的关系是 `或`，可以匹配出，包含 d，f 的所有符合要求的字符串
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
result = re.findall("a[bf]c",a)
# ['abc', 'afc']
```

匹配出非 `bf`，前面加上 `^`
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
result1 = re.findall("a[^bf]c",a)
# ['acc', 'adc', 'aec', 'ahc']
```

如果是匹配一段有规律的字符串，比如 `a-c` 中所有的，那么就可以直接写 [a-c]
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
result2 = re.findall("a[b-e]c",a)
['abc', 'acc', 'adc', 'aec']
```

## 概括字符集

概括字符集可以理解成字符集的一种，就是讲一些写的比较繁琐的概括一下；

比如：/d 可以写成 [0-9]，/D 可以写成 [^0-9]

一般常用的是以下 6 种：
| 概括字符集 | 字符集表示 | 表示意义 |
| -- | -- | -- |
| \d | [0-9] | 数字 |
| \D | [^0-9]| 非数字 |
| \w | [a-zA-Z0-9_] |单词字符(数字、普通字符和下划线)（**不包括特殊字符**）|
| \W | | 非单词字符（包括空格、&、*，制表符、换行符等等特殊符号）|
| \s | [" ","\t","\n","\r"]| 空白字符 |
| \S | [^"_", ^"\t", ^"\n", ^"\r"] | 非空白字符|
| . | [^\n]| 可以**匹配除换行符之外任意一个字符** | 

## 数量词
以上的匹配模式，都会在字符中匹配出单个字符，然后组成列表返回，当我们想要匹配多个字符的时候，就不符合我们的要求了；

所以我们可以在匹配的过程中通过数字的数量匹配出我们想要的指定字符串；比如 [a-z]{3}，会匹配 3 个字符

找出指定字符数量的字符
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
d = "12www34"
# 匹配 3 个a-z字符
print(re.findall("[a-z]{3}", d))
["www"]
d = "12www34"
# 匹配 2 个a-z字符
print(re.findall("[a-z]{2}", d))
["ww"]
# 匹配 1 个a-z字符
print(re.findall("[a-z]{1}", d))
["w","w","w"]
```

找出一个范围的字符，
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
d = "python java php c c++"
print(re.findall("[a-z]{1,6}", d))
# ['python', 'java', 'php', 'c', 'c']
```

## 匹配 0 次 1 次或者无限次

0 次、1 次、无限次都是属于数量词匹配

`*` 可以匹配 0 次、1 次和无限次
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
d = "pytho0python1pythonn2"
print(re.findall("python",d))
# ['python', 'python']
print(re.findall("python*",d))
# ['pytho', 'python', 'pythonn']
```

`+` 可以匹配 1 次和无限次
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
d = "pytho0python1pythonn2"
print(re.findall("python",d))
# ['python', 'python']
print(re.findall("python+",d))
# ['python', 'pythonn']
```

`?`匹配 0 次或者 1 次

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
d = "pytho0python1pythonn2"
print(re.findall("python?",d))
# ['pytho', 'python', 'python']
```

匹配 0 次或者 1 次，使用 `?`，可以用来去重，这个比字符串截取更加的方便；


## 贪婪于非贪婪
贪婪和非贪婪模式是导致产生 bug 的主要原因，很多时候处理数据因为这两种模式的存在，会导致一些特殊情况被特殊处理，产生 bug ；

### 贪婪模式
当我们用数量词进行匹配时候，在数量词的范围内，会用范围内最大的那个数量进行匹配，指导某个字符不满足要求再返回，

比如`{1,3}`，在 `"p1py2pyt"` 中，当匹配到 `p` 会继续匹配，但是遇到了 `1`，不符合要求，就会停止匹配，最后那个会匹配到 `pyt` 

### 非贪婪模式
当使用非贪婪模式时候，`{2,5}?`，会有限匹配 2，也就是会有匹配 2 个字符，然后继续寻找下一个字符，`"pyt"` 使用非贪婪模式，就只能匹配出 `"py"`，当匹配两个字符 `"py"`，之后，再去寻找第二个2个字符的字符串，但是 `"t"`，并不符合要求，所有不会匹配

也就是加个 `?` 会形成一个非贪婪的匹配模式；

**一定要注意这两种模式**


## 边界匹配符

判断一个字符串的整体是否符合某个规则，而不是某一段是否符合

- `^`从头开始匹配
- `$`直到末尾位置

这个两个符号，就可以解决匹配整个字符串的问题，比如验证某个字符串是否符合电话号码，不用在判断之前先判断这个字符串的位数，而是直接匹配

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
num1 = "100000000"
re.findall("^\d{4,9}$",num1)
["1000000000"]
```

当放在开头的位置时候，就相当于占位符，从头开始匹配，下边这种情况，就不能匹配到任何的字符串
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
num1 = "1000"
print(re.findall("^000",num1))
```

当挡在结尾的时候，也相当于在结尾的占位符，到末尾结束，这种情况下是能匹配到的，但是如果结尾不是 0，就匹配不到；
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
num1 = "1000"
print(re.findall("000$",num1))
[000]
```

## 组

很多时候我们想要匹配多个时候，这个字符串可以用 `()`，进行包裹起来，表示成一个 组，那么进行匹配的时候，会按照组的概念，进行匹配；

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
num1 = "PythonPythonPythonPython"
print(re.findall("(Python){1}",num1))
# 匹配一组，这个时候就会出现四个元素的列表['Python', 'Python', 'Python', 'Python']

print(re.findall("(Python){2}",num1))
# ['Python', 'Python']
print(re.findall("(Python){3}",num1))
# ['Python']
print(re.findall("(Python){4}",num1))
# ['Python']
```

注意 `()` 和 `[]` 区别，一个代表`且`一个代表`或`

## 匹配模式参数

> findall(pattern,string,flags=0)

最后的 flag 选项就是匹配模式， 

flags=re.I：忽略大小写模式
flags=re.S：. 号将忽略，他会匹配所有的字符



## re.sub 正则替换

替换的参数
> sub(pattern, repl, string, count=0, flags=0)

count=0，默认参数，会无限制的替换
count=1，能替换的最大次数，也就是只能替换第一个参数

字符串是不可变的，所以不管是怎么替换，都不会对原来的变量产生变化，除非重新赋值；

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import re
a = "Python"
result = re.sub("python","Java",a))
# Java
print(a)
# Python
```


## 把函数作为参数传递


re.sub() 方法中 repl 参数可以传递函数进去，毕竟在python 中一切皆对象，所以可以传入，这在处理复杂逻辑的时候非常有用，比如模糊匹配时候，针对不同的匹配值进行不同的替换操作就非常有用，子样子复杂的逻辑都会被封装在函数中；

比如匹配 python 替换成 java，匹配 go 替换成 java，写入函数中，对匹配值进行判断，替换不同的值，这样子写起来更加符合逻辑，也更加保证了完整性；


<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import re

a = "A8C3721D86"

def conver(value):
    matched = value.group()
    if int(matched) >=6:
        # 只能返回字符串，因为替换的原本就是str，不能传回 int
        return "9"
    else:
        return "0"

result = re.sub("\d", conver, a)
print(result)
# A9C0900D99
```


## search 和 mathch 参数

一般这两个函数很少用，findall 函数基本上能实现所有的需求，除非不够用了，才会使用这两个方法；

这两个方法有两个特征：
- 返回一个对象，这个对象可以使用 group()和span()，进行解析，获取更多内容；
- 只要搜索到了，就不会继续搜索了，直接返回搜索的内容；

所以推荐使用 findall() 函数

re.search():从整个字符串中进行搜索，



re.mathch()：从字符串的开头开始匹配，如果开头就没有匹配上，那么不会返回任何结果；如果匹配到，会返回一个对象；

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
a = "A8C3721D86"
result = re.match("\d",a)
# None

a = "8C3721D86"
result = re.match("\d",a)
# <_sre.SRE_Match object; span=(0, 1), match='8'>
```


## group 分组

group 分组，需要配合 match和search函数使用，他们比 findall 会多输出位置参数，这个在特殊场景是有用的；
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import re

a = "life is short, i use python"
result = re.search("life(.*)python",a)
# group 是对分组内容的获取
print(result.group(1))
```

findall 函数也可以返回特定的值，只要加了括号
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
a = "life is short, i use python"
result = re.findall("life.*python",a)
result1 = re.findall("life(.*)python",a)
# 区分以下加不加括号
print(result)
# ['life is short, i use python']
print(result1)
# [' is short, i use ']
```


对于 () 这个选项的表达，只要加了 () 就会优先输出这个匹配的 ()，也就是会输出 () 中的内容，如果多个 ()，会用索引的方法提取出指定的字符串
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import re

a = "life is short, i use python"
result1 = re.findall("life(.)(.*)python",a)
# 添加多个分组，会返回一个包含 tuple 的列表
print(result1[0])
['life is short, i use python']
print(result1[0][1])
# is short, i use
print(type(result1[0]))
<class 'tuple'>
```


## 正则表达式的学习

1. 可以参考往上已经写好的东西，拿来使用，这样子更加能够提升效率；
2. 很多内置的字符串处理函数能够代替正则表达式，这个就是取舍了，很多时候这两种方式带来的处理思维是不一样的，哪一种更好的服务代码的逻辑和可读性就是用哪种；
<<<<<<< HEAD
=======
=======



## 正则表达式

正则表达式是一个特殊的字符序列，是判断一个字符串是否是否于我们所设定的字符序列匹配；

可以实现一些检索文本，实现一个文本替换的操作，

检测一串数字是否是电话号码
检测一个字符串是否是邮箱
将文本中指定的单词替换成另一个单词

> re.findall("pattern(正则表达式)", "String(待匹配字符串)")

返回一个 List ，finall 是查找所有的匹配值组成一个列表，如果判断，可以加上 len()，判断 List 的长度

```python
a = "C|C++|Python|Java|JavaScript"
result = a.index("Python") > -1
print(result)
result1 = re.findall("Python", a)
if len(result1) > 0:
    print("True")
```

## 原字符于普通字符
正则表达式就是由一些普通字符和原字符组成的

- 普通字符："Python"
- 元字符："/d"
- 普通字符和元字符混合："/dpython"

常用的元字符
/d 匹配一个数字字符 [0-9]
/D 匹配一个非数字字符 [^0-9]


找出所有的数字，可以直接使用 for 循环

```python
b = "C0C++2Python3Java4JavaScript"
li = []
for i in a:
    if i.isdigit():
        li.append(i)
print(li)
```
但是不推荐这样的方式，性能和繁琐，不够直接，可以使用`正则表达式`

想要查找 0,1,2,3,4,5,6 这些数字，可以抽象出来进行处理
```python
# \d:匹配数字
li1 = re.findall("\d",b)
print(li1)
# ['0', '2', '3', '4']

# \D:匹配非数字
li2 = re.findall("\D",b)
print(li2)
# ['C', 'C', '+', '+', 'P', 'y', 't', 'h', 'o', 'n', 'J', 'a', 'v', 'a', 'J', 'a', 'v', 'a', 'S', 'c', 'r', 'i', 'p', 't']
```

## 字符集

```python
import re
a = "abc,acc,adc,aec,afc,ahc"
```

想匹配出 `adc` 或者`afc`，就可以用 []，这种字符集进行筛选

[bf] 这里边的关系是 `或`，可以匹配出，包含 d，f 的所有符合要求的字符串
```python
result = re.findall("a[bf]c",a)
# ['abc', 'afc']
```

匹配出非 `bf`，前面加上 `^`
```python
result1 = re.findall("a[^bf]c",a)
# ['acc', 'adc', 'aec', 'ahc']
```

如果是匹配一段有规律的字符串，比如 `a-c` 中所有的，那么就可以直接写 [a-c]
```python
result2 = re.findall("a[b-e]c",a)
['abc', 'acc', 'adc', 'aec']
```

## 概括字符集

概括字符集可以理解成字符集的一种，就是讲一些写的比较繁琐的概括一下；

比如：/d 可以写成 [0-9]，/D 可以写成 [^0-9]

一般常用的是以下 6 种：
| 概括字符集 | 字符集表示 | 表示意义 |
| -- | -- | -- |
| \d | [0-9] | 数字 |
| \D | [^0-9]| 非数字 |
| \w | [a-zA-Z0-9_] |单词字符(数字、普通字符和下划线)（**不包括特殊字符**）|
| \W | | 非单词字符（包括空格、&、*，制表符、换行符等等特殊符号）|
| \s | [" ","\t","\n","\r"]| 空白字符 |
| \S | [^"_", ^"\t", ^"\n", ^"\r"] | 非空白字符|
| . | [^\n]| 可以**匹配除换行符之外任意一个字符** | 

## 数量词
以上的匹配模式，都会在字符中匹配出单个字符，然后组成列表返回，当我们想要匹配多个字符的时候，就不符合我们的要求了；

所以我们可以在匹配的过程中通过数字的数量匹配出我们想要的指定字符串；比如 [a-z]{3}，会匹配 3 个字符

找出指定字符数量的字符
```python
d = "12www34"
# 匹配 3 个a-z字符
print(re.findall("[a-z]{3}", d))
["www"]
d = "12www34"
# 匹配 2 个a-z字符
print(re.findall("[a-z]{2}", d))
["ww"]
# 匹配 1 个a-z字符
print(re.findall("[a-z]{1}", d))
["w","w","w"]
```

找出一个范围的字符，
```python
d = "python java php c c++"
print(re.findall("[a-z]{1,6}", d))
# ['python', 'java', 'php', 'c', 'c']
```

## 匹配 0 次 1 次或者无限次

0 次、1 次、无限次都是属于数量词匹配

`*` 可以匹配 0 次、1 次和无限次
```python
d = "pytho0python1pythonn2"
print(re.findall("python",d))
# ['python', 'python']
print(re.findall("python*",d))
# ['pytho', 'python', 'pythonn']
```

`+` 可以匹配 1 次和无限次
```python
d = "pytho0python1pythonn2"
print(re.findall("python",d))
# ['python', 'python']
print(re.findall("python+",d))
# ['python', 'pythonn']
```

`?`匹配 0 次或者 1 次

```python
d = "pytho0python1pythonn2"
print(re.findall("python?",d))
# ['pytho', 'python', 'python']
```

匹配 0 次或者 1 次，使用 `?`，可以用来去重，这个比字符串截取更加的方便；


## 贪婪于非贪婪
贪婪和非贪婪模式是导致产生 bug 的主要原因，很多时候处理数据因为这两种模式的存在，会导致一些特殊情况被特殊处理，产生 bug ；

### 贪婪模式
当我们用数量词进行匹配时候，在数量词的范围内，会用范围内最大的那个数量进行匹配，指导某个字符不满足要求再返回，

比如`{1,3}`，在 `"p1py2pyt"` 中，当匹配到 `p` 会继续匹配，但是遇到了 `1`，不符合要求，就会停止匹配，最后那个会匹配到 `pyt` 

### 非贪婪模式
当使用非贪婪模式时候，`{2,5}?`，会有限匹配 2，也就是会有匹配 2 个字符，然后继续寻找下一个字符，`"pyt"` 使用非贪婪模式，就只能匹配出 `"py"`，当匹配两个字符 `"py"`，之后，再去寻找第二个2个字符的字符串，但是 `"t"`，并不符合要求，所有不会匹配

也就是加个 `?` 会形成一个非贪婪的匹配模式；

**一定要注意这两种模式**


## 边界匹配符

判断一个字符串的整体是否符合某个规则，而不是某一段是否符合

- `^`从头开始匹配
- `$`直到末尾位置

这个两个符号，就可以解决匹配整个字符串的问题，比如验证某个字符串是否符合电话号码，不用在判断之前先判断这个字符串的位数，而是直接匹配

```python
num1 = "100000000"
re.findall("^\d{4,9}$",num1)
["1000000000"]
```

当放在开头的位置时候，就相当于占位符，从头开始匹配，下边这种情况，就不能匹配到任何的字符串
```python
num1 = "1000"
print(re.findall("^000",num1))
```

当挡在结尾的时候，也相当于在结尾的占位符，到末尾结束，这种情况下是能匹配到的，但是如果结尾不是 0，就匹配不到；
```python
num1 = "1000"
print(re.findall("000$",num1))
[000]
```

## 组

很多时候我们想要匹配多个时候，这个字符串可以用 `()`，进行包裹起来，表示成一个 组，那么进行匹配的时候，会按照组的概念，进行匹配；

```python
num1 = "PythonPythonPythonPython"
print(re.findall("(Python){1}",num1))
# 匹配一组，这个时候就会出现四个元素的列表['Python', 'Python', 'Python', 'Python']

print(re.findall("(Python){2}",num1))
# ['Python', 'Python']
print(re.findall("(Python){3}",num1))
# ['Python']
print(re.findall("(Python){4}",num1))
# ['Python']
```

注意 `()` 和 `[]` 区别，一个代表`且`一个代表`或`

## 匹配模式参数

> findall(pattern,string,flags=0)

最后的 flag 选项就是匹配模式， 

flags=re.I：忽略大小写模式
flags=re.S：. 号将忽略，他会匹配所有的字符



## re.sub 正则替换

替换的参数
> sub(pattern, repl, string, count=0, flags=0)

count=0，默认参数，会无限制的替换
count=1，能替换的最大次数，也就是只能替换第一个参数

字符串是不可变的，所以不管是怎么替换，都不会对原来的变量产生变化，除非重新赋值；

```python
import re
a = "Python"
result = re.sub("python","Java",a))
# Java
print(a)
# Python
```


## 把函数作为参数传递


re.sub() 方法中 repl 参数可以传递函数进去，毕竟在python 中一切皆对象，所以可以传入，这在处理复杂逻辑的时候非常有用，比如模糊匹配时候，针对不同的匹配值进行不同的替换操作就非常有用，子样子复杂的逻辑都会被封装在函数中；

比如匹配 python 替换成 java，匹配 go 替换成 java，写入函数中，对匹配值进行判断，替换不同的值，这样子写起来更加符合逻辑，也更加保证了完整性；


```python
import re

a = "A8C3721D86"

def conver(value):
    matched = value.group()
    if int(matched) >=6:
        # 只能返回字符串，因为替换的原本就是str，不能传回 int
        return "9"
    else:
        return "0"

result = re.sub("\d", conver, a)
print(result)
# A9C0900D99
```


## search 和 mathch 参数

一般这两个函数很少用，findall 函数基本上能实现所有的需求，除非不够用了，才会使用这两个方法；

这两个方法有两个特征：
- 返回一个对象，这个对象可以使用 group()和span()，进行解析，获取更多内容；
- 只要搜索到了，就不会继续搜索了，直接返回搜索的内容；

所以推荐使用 findall() 函数

re.search():从整个字符串中进行搜索，



re.mathch()：从字符串的开头开始匹配，如果开头就没有匹配上，那么不会返回任何结果；如果匹配到，会返回一个对象；

```python
a = "A8C3721D86"
result = re.match("\d",a)
# None

a = "8C3721D86"
result = re.match("\d",a)
# <_sre.SRE_Match object; span=(0, 1), match='8'>
```


## group 分组

group 分组，需要配合 match和search函数使用，他们比 findall 会多输出位置参数，这个在特殊场景是有用的；
```python
import re

a = "life is short, i use python"
result = re.search("life(.*)python",a)
# group 是对分组内容的获取
print(result.group(1))
```

findall 函数也可以返回特定的值，只要加了括号
```python
a = "life is short, i use python"
result = re.findall("life.*python",a)
result1 = re.findall("life(.*)python",a)
# 区分以下加不加括号
print(result)
# ['life is short, i use python']
print(result1)
# [' is short, i use ']
```


对于 () 这个选项的表达，只要加了 () 就会优先输出这个匹配的 ()，也就是会输出 () 中的内容，如果多个 ()，会用索引的方法提取出指定的字符串
```python
import re

a = "life is short, i use python"
result1 = re.findall("life(.)(.*)python",a)
# 添加多个分组，会返回一个包含 tuple 的列表
print(result1[0])
['life is short, i use python']
print(result1[0][1])
# is short, i use
print(type(result1[0]))
<class 'tuple'>
```


## 正则表达式的学习

1. 可以参考往上已经写好的东西，拿来使用，这样子更加能够提升效率；
2. 很多内置的字符串处理函数能够代替正则表达式，这个就是取舍了，很多时候这两种方式带来的处理思维是不一样的，哪一种更好的服务代码的逻辑和可读性就是用哪种；
>>>>>>> a52c4f4cb02295b3428fd12870734803728a471b:03-编程语言/01-Python/基础与进阶/05-标准库/re正则表达式.md
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
3. 正则表达式，只要是长时间不使用，一定会忘记的；


## 序列化（serialization）和反序列化（deserialization）

### 概念
- 序列化：在数据储存与发送的部分是指将一个对象存储至一个存储介质，例如文件或是存储器缓冲等，或者透过网络发送资料时进行编码的过程，可以是字节或是XML等格式。而字节的或XML编码格式可以还原完全相等的对象。这程序被应用在不同应用程序之间发送对象，以及服务器将对象存储到文件或数据库。相反的过程又称为反序列化。
	- 通俗讲：变量在内存中变成可存储和传输的过程中成为序列化，就内存的数据存储到磁盘或者网络上传输到其他机器上，称为`序列化`；
- 反序列化：
	- 通俗讲：将序列化的对象重新读取到内存中进行使用，称为`反序列化`


### 序列化格式：
1. XML：90年代后期开始推动标准序列化的协议：XML（可扩展标记语言）应用于产生人类可读的文字编码。
2. 二进制 XML：作为一种妥协方式，它不能被纯文本编辑器读取，但比一般 XML 更为扎实。在二十一世纪的 Ajax 技术网页中，XML 经常应用于结构化资料在客端和服务端之间的异步传输。
3. JSON 是一种轻量级的纯文字替代，也常用于网页应用中的客端－服务端通信。JSON 肇基于JavaScript 语法所派生，但也广为其它编程语言所支持。
4. YAML 是一个可读性高，用来表达资料序列化的格式


## JSON 介绍
[JSON 标准库官方文档](https://docs.python.org/zh-cn/3/library/json.html)

encode：编码
decode：解码

### JSON定义
JSON定义：**JSON 是一种轻量级的数据交换格式**

JSON = JavaScript Object Notation，也就是 JavaScript 对象标记

字符串是 JSON 的表现形式，也就是 JSON 的载体是字符串；

什么是 **JSON 字符串**：符合 JSON 格式的字符串就是 JSON 字符串；


### JSON 优点
JSON 和 XML：JSON 很多时候代替了 XML ，为什么能取代 XML 取决于以下优点；
当然很多场景依然使用 XML 这种格式；

- 易于阅读
- 易于解析：跨语言交换数据（XML 也能跨语言）
- 网络传输效率高

JSON 的一个特点是：可以和任何一门语言中的一种数据格式进行转换，在python 中，dict 就和 JSON 进行转换；


## json 格式在 python 中的转换

array，很多语言中只有这一种表示组的概念，不像python 中把组的概念，分成了 list 和 dict

json 是一种数据格式，在 python 中是一个 dict，也可以是一个 list，响应的转化过程中比如 bool 值，还会转化成自己的数据类型

json 在python 中操作，其实就是转换成 python 内置数据类型，进行操作

### json 对应的 python 对象
json 中bool 值是小写的 false 和 true，转化的过程中，会成为 False 和 True

对应列表
| json | python |
| -- | -- |
| object | dict |
| array | list |
| string | str |
| number | int |
| true | True |
| false | False |
| null | None |

<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
import json

json_str = '{"name":"sunxian", "age":18}'
student = json.loads(json_str)
print(isinstance(student,dict))
<<<<<<< HEAD

```

```



>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
## 序列化和反序列化

- 序列化：变量在内存中变成可存储和传输的过程中成为序列化，就内存的数据存储到 磁盘或者网络上传输到其他机器上，称为`序列化`；
- 反序列化：将序列化的对象重新读取到内存中，称为`反序列化`

对应的 json 处理：
- 序列化：将 python 的 dict 转化成 json 进行传输；
- 反序列化：将 json 转化成 python 内置类型，进行解析处理；

<<<<<<< HEAD

```python
import json

# 序列化，将 json 格式转化成 str 格式

=======
```py
import json

# 序列化
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
student1 = [
    {"name":"sunxian", "age":18},
    {"name":"sunxian", "age":19}
]
json_str = json.dumps(student1)
print(json_str)
# [{"name": "sunxian", "age": 18}, {"name": "sunxian", "age": 19}]
print(isinstance(json_str,str))
# True


<<<<<<< HEAD
# 反序列化：将 str 格式转换成 json 格式，也就是字典格式；
=======
# 反序列化
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
json_str = '{"name":"sunxian", "age":18}'
student = json.loads(json_str)
print(student)
# {'name': 'sunxian', 'age': 18}
print(isinstance(student,dict))
# True
```

其实可以看到在处理 json 中，`序列化`就是讲 python 内置数据类型转换成 json 字符串，而`反序列化`，就是将 json 转化成 python 内置数据类型；


## 小谈 JSON、JSON对象、JSON字符串

JSON：一种数据交换格式
JSON 对象：
JSON 字符串：符合 JSON 格式的字符串

A 语言数据类型 --> JSON数据格式(中间数据类型) <-- B 语言数据类型

REST API 服务的标准数据格式就是 JSON 数据格式；
<<<<<<< HEAD
=======
=======



# JSON

## JSON 介绍


### JSON定义
JSON定义：**JSON 是一种轻量级的数据交换格式**

JSON = JavaScript Object Notation，也就是 JavaScript 对象标记

字符串是 JSON 的表现形式，也就是 JSON 的载体是字符串；

什么是 **JSON 字符串**：符合 JSON 格式的字符串就是 JSON 字符串；


### JSON 优点
JSON 和 XML：JSON 很多时候代替了 XML ，为什么能取代 XML 取决于以下优点；
当然很多场景依然使用 XML 这种格式；

- 易于阅读
- 易于解析：跨语言交换数据（XML 也能跨语言）
- 网络传输效率高

JSON 的一个特点是：可以和任何一门语言中的一种数据格式进行转换，在python 中，dict 就和 JSON 进行转换；


## json 格式在 python 中的转换

array，很多语言中只有这一种表示组的概念，不像python 中把组的概念，分成了 list 和 dict

json 是一种数据格式，在 python 中是一个 dict，也可以是一个 list，响应的转化过程中比如 bool 值，还会转化成自己的数据类型

json 在python 中操作，其实就是转换成 python 内置数据类型，进行操作

### json 对应的 python 对象
json 中bool 值是小写的 false 和 true，转化的过程中，会成为 False 和 True

对应列表
| json | python |
| -- | -- |
| object | dict |
| array | list |
| string | str |
| number | int |
| true | True |
| false | False |
| null | None |

```python
import json

json_str = '{"name":"sunxian", "age":18}'
student = json.loads(json_str)
print(isinstance(student,dict))

```




对应的 json 处理：
- 序列化：将 python 的 dict 转化成 json 进行传输；
- 反序列化：将 json 转化成 python 内置类型，进行解析处理；


```python
import json

# 序列化，将 json 格式转化成 str 格式

student1 = [
    {"name":"sunxian", "age":18},
    {"name":"sunxian", "age":19}
]
json_str = json.dumps(student1)
print(json_str)
# [{"name": "sunxian", "age": 18}, {"name": "sunxian", "age": 19}]
print(isinstance(json_str,str))
# True


# 反序列化：将 str 格式转换成 json 格式，也就是字典格式；
json_str = '{"name":"sunxian", "age":18}'
student = json.loads(json_str)
print(student)
# {'name': 'sunxian', 'age': 18}
print(isinstance(student,dict))
# True
```

其实可以看到在处理 json 中，`序列化`就是讲 python 内置数据类型转换成 json 字符串，而`反序列化`，就是将 json 转化成 python 内置数据类型；


## 小谈 JSON、JSON对象、JSON字符串

JSON：一种数据交换格式
JSON 对象：
JSON 字符串：符合 JSON 格式的字符串

A 语言数据类型 --> JSON数据格式(中间数据类型) <-- B 语言数据类型

REST API 服务的标准数据格式就是 JSON 数据格式；


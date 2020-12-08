

## os



### os.name（获取操作系统版本）
导入的依赖特定操作系统的模块的名称。以下名称目前已注册: 'posix', 'nt', 'java'
- posix：unix 标准
- nt：windows

```python
import os
# unix 平台
print(os.name) # posix

# windowns 平台
print(os.name) # nt
```

### os.uname()（获取操作系统的识别信息，包含五个 elements tuple）

windows 没有这个属性
```python
import os
os.uname()
# posix.uname_result(sysname='Linux', nodename='sunyunxian-nb', release='4.4.0-18362-Microsoft', version='#836-Microsoft Mon May 05 16:04:00 PST 2020', machine='x86_64')
```




## pathlib

操作目录和文件

pathlib 是 python3.4 版本新加入的标准库，并且称为目录和文件处理官方的主推库，她是 OOP 思维编写的库；

pathlib 库在 Python3.6 版本才继续完善，达到了高可用的程度，所以推荐 Python3.6 以上版本直接使用 pathlib 库；


|描述|os or os.path| pathlib|
-|-|-
| 获取绝对路径 | os.path.abspath | Path.resolve |
| 更改文件属性 | os.chmod | Path.chmod |
|创建文件夹 | os.mkdir| Path.mkdir |
|重命名 | os.rename | Path.rename |
||||
||||
| 是否是目录 | os.path.isdir | Path.is_dir |
| 是否是文件 | os.path.isfile | Path.is_file|
| 是否是链接 |||







```python
from pathlib import Path

# 获取当前目录
p = Path.cwd()
print(p) # C:\Users\TT\Desktop\Learn\learn_python

# Path 获取当前路径的完整路径
p = Path(".")

print(p.resolve(), p.is_dir())
```


## os.path 库操作目录和文件夹

```python
import os.path

# 根据当前路径 `.`，获取绝对路径
abspath = os.path.abspath(".")
print(abspath)  # C:\Users\TT\Desktop\Learn\learn_python

# 根据上一级路径 `.`，获取绝对路径
abspath2 = os.path.abspath("..")
print(abspath2)# C:\Users\TT\Desktop\Learn

# 判断是否是目录/文件/链接等...
isall = os.path.exists("/Users")
isdir = os.path.isdir("/TT")
isfile = os.path.isfile("file.txt")
print(isall, isdir, isfile) # True False True

# 路径拼接
user_path = os.path.join("/Users/TT/", "sunxian/video/", "today.mp4")
print(user_path)
```




<<<<<<< HEAD
=======
=======

## 目录和文件操作


目录 = 文件夹

## pathlib 库操作目录和文件

pathlib 是 python3.4 版本新加入的标准库，并且称为目录和文件处理官方的主推库，她是 OOP 思维编写的库；

pathlib 库在 Python3.6 版本才继续完善，达到了高可用的程度，所以推荐 Python3.6 以上版本直接使用 pathlib 库；


|描述|os or os.path| pathlib|
-|-|-
| 获取绝对路径 | os.path.abspath | Path.resolve |
| 更改文件属性 | os.chmod | Path.chmod |
|创建文件夹 | os.mkdir| Path.mkdir |
|重命名 | os.rename | Path.rename |
||||
||||
| 是否是目录 | os.path.isdir | Path.is_dir |
| 是否是文件 | os.path.isfile | Path.is_file|
| 是否是链接 |||







```python
from pathlib import Path

# 获取当前目录
p = Path.cwd()
print(p) # C:\Users\TT\Desktop\Learn\learn_python

# Path 获取当前路径的完整路径
p = Path(".")

print(p.resolve(), p.is_dir())
```


## os.path 库操作目录和文件夹


```python
import os.path

# 根据当前路径 `.`，获取绝对路径
abspath = os.path.abspath(".")
print(abspath)  # C:\Users\TT\Desktop\Learn\learn_python

# 根据上一级路径 `.`，获取绝对路径
abspath2 = os.path.abspath("..")
print(abspath2)# C:\Users\TT\Desktop\Learn

# 判断是否是目录/文件/链接等...
isall = os.path.exists("/Users")
isdir = os.path.isdir("/TT")
isfile = os.path.isfile("file.txt")
print(isall, isdir, isfile) # True False True

# 路径拼接
user_path = os.path.join("/Users/TT/", "sunxian/video/", "today.mp4")
print(user_path)
```

<<<<<<< HEAD:03-编程语言/01-Python/基础知识和进阶知识/06-进程和线程/021-os-standard-library.md


# os 模块提供处理文件和目录方法

## 内置方法
- __file__：当前文件路径
- __doc__：当前文件描述
- __main__：
- __name__:是否为主文件


## os.chdir()
改变当前工作目录到指定路径，文件也会移动到该指定路径
```py
print(os.path.abspath('.')) # C:\Users\sunyunxian
print(os.chdir('C:/Users/')) # C:\Users
```


# os.getcwd()
返回一个当前工作目录
```py
print(os.getcwd()) # C:\Users\sunyunxian
```


## os.path.join(path0, path1, path2)
文件和目录合并路径
```py
path1 = 'home'
path2 = 'develop'
path3 = 'code'
path4 = path1 + path2 + path3
path5 = os.path.join(path1, path2, path3)
print(path4) # homedevelopcode
print(path5) # home\develop\code
```

## os.path.realpath(path)
返回真实地址
```py
file_path = os.path.realpath(__file__)
print(file_path) # c:\Users\sunyunxian\Desktop\python\Pytest_demo\test_sample.py
```

## os.path.split(path)
将路径分成 dirname 和 basename，返回元组
```py
file_path = 'c:/Users/sunyunxian/Desktop/python/Pytest_demo/test_sample.py'
split_path = os.path.split(file_path)
print(split_path) # ('c:\\Users\\sunyunxian\\Desktop\\python\\Pytest_demo', 'test_sample.py')
print(split_path[0]) # c:\Users\sunyunxian\Desktop\python\Pytest_demo
print(split_path[1]) # test_sample.py
```

## os.path.abspath(path)
返回绝对路径
```py
abs_path = os.path.abspath('.')
print(abs_path) # C:\Users\sunyunxian\Desktop\python\Pytest_demo
```

## os.path.dirname(path)
列出文件的路径
```py
dir_name = os.path.dirname(os.path.realpath(__file__))
print(dir_name) # c:\Users\sunyunxian\Desktop\python\Pytest_demo
```

## os.path.exists(path)
路径是否存在
```py
path_file0 = os.path.exists('demo.py')
path_file1 = os.path.exists('test_sample.py')
print(path_file0) # False
print(path_file1) # True
```

## os.path.expanduser(path)
将 `~` 和 `~user` 转换成用户目录
```py
path_0 = '~/develop/code/python'
path_1 = '/develop/code/python'
user_dir0 = os.path.expanduser(path_0)
user_dir1 = os.path.expanduser(path_1)
print(user_dir0) # C:\Users\sunyunxian/develop/code/python
print(user_dir1) # /develop/code/python
```

## os.path.isdir(path)
## os.path.idfile(path)
判断是否是目录或者文件，绝对路径
```py
path6 = 'C:/Users/'
path7 = 'C:/Users/sunyunxian/useruid.ini'
path_dir = os.path.isdir(path6) 
path_dir1 = os.path.isdir(path7) 
path_file2 = os.path.isfile(path6) 
path_file3 = os.path.isfile(path7) 
print(path_dir) # True
print(path_dir1) # False
print(path_file2) # False
print(path_file3) # True
```

## os.listdir(path)
列出文件下所有的文件夹和文件
```py
dirction = 'C:/Users/'
dirction1 = os.listdir(dirction)
print(dirction1) # ['administrator', 'All Users', 'sunyunxian']
=======


# os 模块提供处理文件和目录方法

## 内置方法
- __file__：当前文件路径
- __doc__：当前文件描述
- __main__：
- __name__:是否为主文件


## os.chdir()
改变当前工作目录到指定路径，文件也会移动到该指定路径
```python
print(os.path.abspath('.')) # C:\Users\sunyunxian
print(os.chdir('C:/Users/')) # C:\Users
```


# os.getcwd()
返回一个当前工作目录
```python
print(os.getcwd()) # C:\Users\sunyunxian
```


## os.path.join(path0, path1, path2)
文件和目录合并路径
```python
path1 = 'home'
path2 = 'develop'
path3 = 'code'
path4 = path1 + path2 + path3
path5 = os.path.join(path1, path2, path3)
print(path4) # homedevelopcode
print(path5) # home\develop\code
```

## os.path.realpath(path)
返回真实地址
```python
file_path = os.path.realpath(__file__)
print(file_path) # c:\Users\sunyunxian\Desktop\python\Pytest_demo\test_sample.py
```

## os.path.split(path)
将路径分成 dirname 和 basename，返回元组
```python
file_path = 'c:/Users/sunyunxian/Desktop/python/Pytest_demo/test_sample.py'
split_path = os.path.split(file_path)
print(split_path) # ('c:\\Users\\sunyunxian\\Desktop\\python\\Pytest_demo', 'test_sample.py')
print(split_path[0]) # c:\Users\sunyunxian\Desktop\python\Pytest_demo
print(split_path[1]) # test_sample.py
```

## os.path.abspath(path)
返回绝对路径
```python
abs_path = os.path.abspath('.')
print(abs_path) # C:\Users\sunyunxian\Desktop\python\Pytest_demo
```

## os.path.dirname(path)
列出文件的路径
```python
dir_name = os.path.dirname(os.path.realpath(__file__))
print(dir_name) # c:\Users\sunyunxian\Desktop\python\Pytest_demo
```

## os.path.exists(path)
路径是否存在
```python
path_file0 = os.path.exists('demo.py')
path_file1 = os.path.exists('test_sample.py')
print(path_file0) # False
print(path_file1) # True
```

## os.path.expanduser(path)
将 `~` 和 `~user` 转换成用户目录
```python
path_0 = '~/develop/code/python'
path_1 = '/develop/code/python'
user_dir0 = os.path.expanduser(path_0)
user_dir1 = os.path.expanduser(path_1)
print(user_dir0) # C:\Users\sunyunxian/develop/code/python
print(user_dir1) # /develop/code/python
```

## os.path.isdir(path)
## os.path.idfile(path)
判断是否是目录或者文件，绝对路径
```python
path6 = 'C:/Users/'
path7 = 'C:/Users/sunyunxian/useruid.ini'
path_dir = os.path.isdir(path6) 
path_dir1 = os.path.isdir(path7) 
path_file2 = os.path.isfile(path6) 
path_file3 = os.path.isfile(path7) 
print(path_dir) # True
print(path_dir1) # False
print(path_file2) # False
print(path_file3) # True
```

## os.listdir(path)
列出文件下所有的文件夹和文件
```python
dirction = 'C:/Users/'
dirction1 = os.listdir(dirction)
print(dirction1) # ['administrator', 'All Users', 'sunyunxian']
>>>>>>> a52c4f4cb02295b3428fd12870734803728a471b:03-编程语言/01-Python/基础与进阶/06-thread-线程-and协程/021-os-standard-library.md
```
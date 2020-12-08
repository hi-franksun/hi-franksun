
# read and write files

## file object

在磁盘上读写文件功能是由操作系统提供的，现代操作系统是不允许普通程序直接操作磁盘的，所以读写文件就是请求操作系统打开一个文件对象（文件描述符），然后通过操作系统提供的接口从文件对象中读取数据和写入数据；

根据其创建方式的不同，文件对象可以处理对真实磁盘文件，对其他类型存储，或是对通讯设备的访问（例如标准输入/输出、内存缓冲区、套接字、管道等等）。文件对象也被称为 文件类对象 或 流。

实际上共有三种类别的文件对象: 原始 二进制文件, 缓冲 二进制文件 以及 文本文件。它们的接口定义均在 io 模块中。创建文件对象的规范方式是使用 open() 函数。

## open function

open 函数负责打开一个 file ，并返回 file object 对象进行处理；

`open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)`

- file：要打开文件的路径
- mode：模式，write/read/append
  - r：读取文件（）默认
  - w：写入文件，会覆盖原有文件
  - a：写入文件，是追加写入

| 字符 | 含义 |
| --- | --- |
| 'r' | 读取（默认）|
| 'w' | 写入，并先截断文件 |
| 'x' | 排它性创建，如果文件已存在则失败 |
| 'a' | 写入，如果文件存在则在末尾追加 |
| 'b' | 二进制模式 |
| 't' | 文本模式（默认）|
| '+' | 打开用于更新（读取与写入）|
默认模式为 'r' (打开用于读取文本，与 'rt' 同义)。 模式 'w+' 与 'w+b' 将打开文件并清空内容。 模式 'r+' 与 'r+b' 将打开文件并不清空内容。

## read small files

1. 内置函数 open() 打开文件文件，并在内存中储存这个文件对象；
2. 调用 read() 方法读取文件对象的内容；
3. 调用 close() 方法关闭文件；

最后必须关闭文件，不然文件对象会始终在内存中占用系统资源；

**打开文件时候，有时候会出现错误，而最后都要关闭文件，读取文件出错后，就不会关闭文件，所以要用 try finally 进行处理，防止读取文件出错；但是一般都会使用 with 语句进行读取，比较方便。**

```python
f = open("file.txt","r")
print(f.read())
f.close()
```

```python
try:
    f = open("file.txt","r")
    print(f.read())
finally:
    if f:
        f.close()
```

```python
with open("file.txt", "r") as f:
    f.read()
```

## read big files

读取普通文件一般不会太大，一次打开读取就可以了，但是遇到大的文件一次读取到内存中处理，需要耗费大量内存，有时候文件过大，内存不够都会出现各种错误，所以读取一部分这种方案就比较合适；

1. read(size)
    10G 文件，一次读取就会导致内存直接爆炸，所以需要控制每次读取的大小；
2. readline()
    分行读取，也就是每次读取一行，这个很少用，因为很少会读一行，一般 for 循环遍历 file object，很多时候会用 readlines 返回一个列表；

    ```python
    fpath = "file.txt"
    with open(fpath) as f:
        for line in f.readline:
            print(line, end='')
    ```

3. readlines()
    分行读取，也就是每次读取一行，返回一个 list，一般用来读取配置文件，配合 for 循环读取配置使用，这个使用的比较多；

    ```python
    fpath = "file.txt"
    with open(fpath) as f:
        result = list(f) # 使用 list 比较常见
        result = f.readlines() # 使用 list 和 readlines 都可以的
    ```

## write file

写入的时候，如果是文本文件，需要先转成字符串格式，如果是二进制模式，先转成字节格式，return 返回值是写入的字符数，

```python
with open(fpath, "a") as f:
    f.write("write file") # 10
```

一定要用 with 语法糖，会忘记 close() 方法，在写入文件时候，如果不加 close() 方法，会存在保存不及时情况，系统不是马上就将文件写入保存，所以要加 close() 方法，这个时候就不会发生文件未被写入问题；

## 操作指针

读取文件中，需要回到文件的制定位置，比如文件的开头，这个时候需要 seek()，操作文件的指针
seek() 函数接受两个参数，也就是偏移量和偏移的坐标，
0：代表已开头为坐标
1：代表当前位置为坐标
2：结尾为坐标
seek(2,0) 以当前位置为坐标，偏移 2 个字符，这个时候读取的就是已第二个字符开始

```python
with open("file.txt", "r") as f:
    # read() 读取文件的几个字符
    print(f.read(1))
    # tell() 现实正在操作文件的指针，这个时候指针是 1
    print(f.tell())
    # read(2) 标识读取了2个字符
    print(f.read(2))
    # 这个时候指针是 3
    print(f.tell())
    # seek 是操作指针，就是又回到文件的开头了
    f.seek(1)
    # 这个时候又回到最开始的位置
    print(f.tell())
```

## 字符编码

编码：open() 函数有一个参数是支持编码的，如果是 gbk 编码，就传入 encoding="gbk"，就可以了，默认是 utf-8

```python
path = "test.txt"
open(path, "r", encoding="gbk")
open(path, "r", encoding="utf-8")
```

## 读取二进制文件

默认是读取 utf-8 文件，如果读取图片、视频等文件，要用 "rb" 模式打开文件，读取时候会输出二进制文件模式

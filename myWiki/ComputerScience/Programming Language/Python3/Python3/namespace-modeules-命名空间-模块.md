

# namespace(命名空间)

> 无声明的情况下，赋值即私有，若外部有相同名称的变量则被遮挡
> 想修改外部变量，需声明（global、nonlocal），或者通过可变对象的内置函数

## 3 种命名空间

- build-in namespace
- global namespace
- local namespace

1. 命名空间的生命周期
命名空间的生命周期取决于对象的作用域，对象执行完成，那么该命名空间的作用域就消失了
## python 目录结构

- 包（也就是文件夹，带__init__.py 文件）
  - 模块（也就是一个文件，包含多个类，也可以包括一些业务函数）
    - 类（一个文件最好一个类，和 Java 类似，最好将 函数和变量都封装到一个类中，不推荐分成很多函数）
      - 函数/变量


## 命名空间

命名空间：包名字和路径，sunyunxian.learn、sunyunxian.health


## 包

一个文件夹下有一个特殊文件 `__init__`，这个文件夹就是一个包，这个包下边的文件，就是模块，`__init__`也是一个模块，只不过 `__init__`，这个模块不是 `sunyunxian.__init__`，而是这个包名，也就是 `sunyunxian`


目录结构
```
sunyunxian
    __init__.py
    learn.py
    health.py
```

learn


## 模块



## 导入模块


```python
from Person import learn
from Person.learn import lear_code
from Person import *
from Person import learn as le 

```

### 解决 * 会导入全部变量的方法

在被调用文件中定义一个  `__all__ = ["a","c"]`，这个时候另一个文件调用这个文件 `*` ,就会直接导入 `["a","c"]`

注意：[] 里面比如是字符串，字符串可以是变量名、类名、函数名

## __all__ 是一个控制导入模块的变量
__all__ 这个**变量**也可以在 __init__ 中使用，它控制哪些包可以导入


## __init__ 文件作用


__init__.py 除了把文件夹变成 package，还可以把包内部的文件互相导入和相对导入，并且可以提前把类、函数、变量，提前导入到 __init__.py 中再在其他文件中导入，可以精简代码；

其实其他模块调用其他包的时候，会自动的执行 __init__ 文件，所以 __init__ 文件中，常会做一些初始化的工作；比如调用 peroson 包下的 student 模块，这个时候 __init__ 文件中有一个 print() 函数，调用的过程中，因为会自动制定 __init__ 文件，所以这个文件的 print() 函数会被调用，打印出相应的文字，比如print("这是我写的库，欢迎使用)，但是一般很少用这个，因为打印一行，也就是打印函数会消耗性能，所以很少用，可以用来玩玩，比较好玩；

`__init__.py` 这个文件名字不是 `__init__`，而是这个 `packagename`，比如 setting 目录下，`__init__.py` 文件，那么这个文件的模块名其实就是 `setting`；

其实其他模块调用其他包的时候，会自动的执行 __init__ 文件，所以 __init__ 文件中，常会做一些初始化的工作；比如调用 peroson 包下的 student 模块，这个时候 __init__ 文件中有一个 print() 函数，调用的过程中，因为会自动制定 __init__ 文件，所以这个文件的 print() 函数会被调用，打印出相应的文字，比如print("这是我写的库，欢迎使用)，但是一般很少用这个，因为打印一行，也就是打印函数会消耗性能，所以很少用，可以用来玩玩，比较好玩

`__init__.py` 这个文件名字不是 `__init__`，而是这个 `packagename`，比如 setting 目录下，`__init__.py` 文件，那么这个文件的模块名其实就是 `setting`


目录结构
- microsoft
  - word
    - docx
      - writer.py

- main.py

writer.py 内容
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
def write():
    print("我是 word-docx下的 writerpy 文件")
```
在 main.py 导入 write.py 应该又一些写法
<<<<<<< HEAD
```python
=======
```py
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4
# 第二种：直接 import 这个文件
import mircosoft.word.docx.writer
mircosoft.word.docx.writer.write()

# 第二种：导入这个文件
from mircosoft.word.docx import writer
writer.write()

# 第三种：导入这个文件的函数
from mircosoft.word.docx.writer import write
write()
```

这样子的写法看起来都需要导入很长的一段，非常不优雅，而且复杂，我们需要用一种方法进行精简；

1. 我们知道导入一个 `__init__.py` 文件会让这个文件夹变成一个包(package)；
2. 我们可以将这个包内的类、函数、变量导入到这个 `__init__.py` 文件中；
<<<<<<< HEAD
3. 这样子，外部就能直接 `from PackageName import 类/函数名` 直接导入了；

## 无视工作区的相对引用

正式工作的时候，我们一些公共的函数，封装的功能会被放到一个文件夹下供其他模块调用，这个时候这个文件夹比较复杂，很容以找不到模块，这个时候就非常要用到 `from .mircosoft.docx.write`，这总形式，这个 `.`，非常重要，代表了想读导入，如果不用这个点，代码量多了，很容易导致找不到模块；


如果一个 package 比较复杂，层级较多；

3. 这样子，外部就能直接 `from PackageName import 类/函数名` 直接导入了

## 无视工作区的相对引用

正式工作的时候，我们一些公共的函数，封装的功能会被放到一个文件夹下供其他模块调用，这个时候这个文件夹比较复杂，很容以找不到模块，这个时候就非常要用到 `from .mircosoft.docx.write`，这总形式，这个 `.`，非常重要，代表了想读导入，如果不用这个点，代码量多了，很容易导致找不到模块


如果一个 package 比较复杂，层级较多




## 关于包的标准使用


这是 request 库的 __init__ 文件；
我们平时调用的时候就是直接 `import requests`；
或者 `from requests import Response`，这种，其实可以看到这个库就是在 __init__.py 文件中使用了相对调用，这样子无论如何放到哪个环境都不会产生导入错误，同时 request 又将常用的类/函数/变量，直接导入了 request(__init__.py) 中，所以可以直接调用了：`from request import code`；

这是 request 库的 __init__ 文件
我们平时调用的时候就是直接 `import requests`
或者 `from requests import Response`，这种，其实可以看到这个库就是在 __init__.py 文件中使用了相对调用，这样子无论如何放到哪个环境都不会产生导入错误，同时 request 又将常用的类/函数/变量，直接导入了 request(__init__.py) 中，所以可以直接调用了：`from request import code`
>>>>>>> f74796369e5c2442cf73a9cdf31757fba47aa2e4

几乎所有的库都是采用了这种方式，弄懂这个看源码非常重要！！！



```python

from . import utils
from . import packages
from .models import Request, Response, PreparedRequest
from .api import request, get, head, post, patch, put, delete, options
from .sessions import session, Session
from .status_codes import codes
from .exceptions import (
    RequestException, Timeout, URLRequired,
    TooManyRedirects, HTTPError, ConnectionError,
    FileModeWarning, ConnectTimeout, ReadTimeout
)
```


## 包和模块的常见错误

### 包和模块的导入时不会被重复导入
比如 t1 和 t2 文件都导入了 os 模块，看似是每执行一个文件都会导入一次，但是实际上不会这样子，只会导入一次，每次都导入，性能会出现问题，所以不存在重复导入的机制，这个是 python 自己决定的；

### 循环导入
两个或者多个文件，来回导入，形成了闭环，这个时候 python 就会报错，也就是循环引入的错误，注意一定是闭环才会导致循环引入；

### 模块导入到模块，就会执行该导入模块的文件
比如 1 导入 2文件， 2文件有一个 print() 函数，在 1 中会直接打印这个函数内容，也就是执行了；

## 入口文件




## python 目录结构

- 包（也就是文件夹，带__init__.py 文件）
  - 模块（也就是一个文件，包含多个类，也可以包括一些业务函数）
    - 类（一个文件最好一个类，和 Java 类似，最好将 函数和变量都封装到一个类中，不推荐分成很多函数）
      - 函数/变量


## 命名空间

命名空间：包名字和路径，sunyunxian.learn、sunyunxian.health


## 包

一个文件夹下有一个特殊文件 `__init__`，这个文件夹就是一个包，这个包下边的文件，就是模块，`__init__`也是一个模块，只不过 `__init__`，这个模块不是 `sunyunxian.__init__`，而是这个包名，也就是 `sunyunxian`


目录结构
```
sunyunxian
    __init__.py
    learn.py
    health.py
```

learn


## 模块



## 导入模块

```python
from Person import learn
from Person.learn import lear_code
from Person import *
from Person import learn as le 
```

### 解决 * 会导入全部变量的方法

在被调用文件中定义一个  `__all__ = ["a","c"]`，这个时候另一个文件调用这个文件 `*` ,就会直接导入 `["a","c"]`

注意：[] 里面比如是字符串，字符串可以是变量名、类名、函数名

## __all__ 是一个控制导入模块的变量
__all__ 这个**变量**也可以在 __init__ 中使用，它控制哪些包可以导入


## __init__ 文件作用


__init__.py 除了把文件夹变成 package，还可以把包内部的文件互相导入和相对导入，并且可以提前把类、函数、变量，提前导入到 __init__.py 中再在其他文件中导入，可以精简代码；

其实其他模块调用其他包的时候，会自动的执行 __init__ 文件，所以 __init__ 文件中，常会做一些初始化的工作；比如调用 peroson 包下的 student 模块，这个时候 __init__ 文件中有一个 print() 函数，调用的过程中，因为会自动制定 __init__ 文件，所以这个文件的 print() 函数会被调用，打印出相应的文字，比如print("这是我写的库，欢迎使用)，但是一般很少用这个，因为打印一行，也就是打印函数会消耗性能，所以很少用，可以用来玩玩，比较好玩；

`__init__.py` 这个文件名字不是 `__init__`，而是这个 `packagename`，比如 setting 目录下，`__init__.py` 文件，那么这个文件的模块名其实就是 `setting`；

目录结构
- microsoft
  - word
    - docx
      - writer.py

- main.py

writer.py 内容
```python
def write():
    print("我是 word-docx下的 writerpy 文件")
```
在 main.py 导入 write.py 应该又一些写法
```python
# 第二种：直接 import 这个文件
import mircosoft.word.docx.writer
mircosoft.word.docx.writer.write()

# 第二种：导入这个文件
from mircosoft.word.docx import writer
writer.write()

# 第三种：导入这个文件的函数
from mircosoft.word.docx.writer import write
write()
```

这样子的写法看起来都需要导入很长的一段，非常不优雅，而且复杂，我们需要用一种方法进行精简；

1. 我们知道导入一个 `__init__.py` 文件会让这个文件夹变成一个包(package)；
2. 我们可以将这个包内的类、函数、变量导入到这个 `__init__.py` 文件中；
3. 这样子，外部就能直接 `from PackageName import 类/函数名` 直接导入了；

## 无视工作区的相对引用

正式工作的时候，我们一些公共的函数，封装的功能会被放到一个文件夹下供其他模块调用，这个时候这个文件夹比较复杂，很容以找不到模块，这个时候就非常要用到 `from .mircosoft.docx.write`，这总形式，这个 `.`，非常重要，代表了想读导入，如果不用这个点，代码量多了，很容易导致找不到模块；


如果一个 package 比较复杂，层级较多；



## 关于包的标准使用

这是 request 库的 __init__ 文件；
我们平时调用的时候就是直接 `import requests`；
或者 `from requests import Response`，这种，其实可以看到这个库就是在 __init__.py 文件中使用了相对调用，这样子无论如何放到哪个环境都不会产生导入错误，同时 request 又将常用的类/函数/变量，直接导入了 request(__init__.py) 中，所以可以直接调用了：`from request import code`；

几乎所有的库都是采用了这种方式，弄懂这个看源码非常重要！！！


```python
from . import utils
from . import packages
from .models import Request, Response, PreparedRequest
from .api import request, get, head, post, patch, put, delete, options
from .sessions import session, Session
from .status_codes import codes
from .exceptions import (
    RequestException, Timeout, URLRequired,
    TooManyRedirects, HTTPError, ConnectionError,
    FileModeWarning, ConnectTimeout, ReadTimeout
)
```


## 包和模块的常见错误

### 包和模块的导入时不会被重复导入
比如 t1 和 t2 文件都导入了 os 模块，看似是每执行一个文件都会导入一次，但是实际上不会这样子，只会导入一次，每次都导入，性能会出现问题，所以不存在重复导入的机制，这个是 python 自己决定的；

### 循环导入
两个或者多个文件，来回导入，形成了闭环，这个时候 python 就会报错，也就是循环引入的错误，注意一定是闭环才会导致循环引入；

### 模块导入到模块，就会执行该导入模块的文件
比如 1 导入 2文件， 2文件有一个 print() 函数，在 1 中会直接打印这个函数内容，也就是执行了；

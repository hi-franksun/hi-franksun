# 函数

函数是对程序逻辑进行结构化的一种编程方式

## 函数特征

1. 功能性
2. 隐藏细节
3. 避免编写重复代码
4. 组织代码

## 函数定义

python 函数名最好用 `小写_小写` 方式，定义时候参数叫做：形参，实际传入的参数叫做实参；

```python
def funcname(parameter_list):
    """
    Documentation Strings：文档字符串，其实可以理解为函数的注释，使用说明
    """
    pass

# 参数列表可以没有
# return value 如果没有返回则返回 None
# return 返回多个值，return x, y 返回的是一个 tuple
```

## 函数返回值

`return` 是一个函数的终止，也就是 `return` 后边的语句是不会执行的；

1. 函数一个返回值
    正常的函数就是一个返回值，也推荐一个返回值

2. 函数无返回值
    `return` 不返回的时候默认返回 `None`

3. 函数返回多个返回值
    直接用逗号隔开，就可以返回多个值，返回多个值会组成一个 tuple(元组)

    ```python
    def test(a,b):
        return a,b

    more_return = test(1,2)
    # 最简单的获取返回值，用下标，但是不推荐
    more_return[0]
    more_return[1]

    # 这种方式接收返回值是最好的方法，也叫序列解包
    more_return1, more_return2 = test(1,2)
    ```

## 函数的变量作用域

在函数内部和函数外边有两个同名变量

函数的变量作用域是向上寻找，如果自身的函数内部没有就会向上一级寻找，函数外没有，可能会到类中寻找，再到类外边寻找，都没有找到，那么会报错；

如果函数的本身有这个变量名，那么变量名就是用函数内部的，也就是局部变量，在函数内修改同名变量，不会影响到外部，如果想要影响到外部，那么可以加上 `global`

```python
var1 = 1
var2 = 2

def test():
    var1 = 3 # 这个时候修改的只是函数内部的同名变量的值，不会影响函数外这个变量名字的值
    print(var1)
    print(var2)

test()
# 3
# 2
print(var1) # 1
print(var2) # 2
```

global 关键字

```python
var1 = 1
var2 = 2

def te():
    global var1  # 添加全局变量的标识符，这时候，var1 就是全局变量了
    var1 = 3
    print(var1)
    print(var2)

te()
# 3
# 2

print(var1) # 3
print(var2) # 2
```

## 函数参数是值传递

其中值始终是对象引用而不是对象的值，这里要有一个**Python variable**的概念，Python 中变量都是引用，同时要了解可更改(mutable)与不可更改(immutable)对象区别；

1. 不可变类型重新赋值
    因为是不可变类型，重新赋值，只能重新开拓一个内存放，也就是指向新的内存地址；

    ```python
    def change_int(a: int): # a --> 100
        a = 10 # a 是不可变的，所以 a --> 10
        return a # 最后返回了 a = 10

    b = 100 # b --> 100
    print(change_int(b)) # 传入的b 其实是复制了 b 的引用值，，也就是传入的 a = 100，而不是传入的 b ，因为函数的执行会引入一个函数局部变量的新符号表 结果是：10
    print(b) # b 一直是指向 100，没有发生任何变化
    ```

2. 可变类型重新赋值
    还是和上边一样的，不会改变，因为重新赋值了

    ```python
    def change_list(a: list):
        a = [10]
        return a

    b = [100]
    print(change_list(b))
    print(b)
    ```

3. 可变类型修改
    没有重新赋值，而是修改了值的内容，即指向的对象修改了内部 Value

    ```python
    def change_list(a: list): # a --> list =[100]
        a[0] = 10 # a -->list = [10]
        return a


    b = [100] # b --> [100]
    print(change_list(b)) # [10]
    print(b) # b --> list = [10]，也就是b发生了变化
    ```

### 默认参数是可变对象时候，只会执行一次

 默认值只会执行一次。这条规则在默认值为可变对象（列表、字典以及大多数类实例）时很重要。比如，下面的函数会存储在后续调用中传递给它的参数:

```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1)) # [1]
print(f(2)) # [1, 2]
print(f(3)) # [1, 2, 3]
```

不想共享默认值时候，则需要借助一个中间量做一个判断

```python
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```

> 默认参数是不可变对象时候，是不会出现这种情况的，比如因为 l 默认指向了 0 内存地址，内部修改后指向了 1，再次调用时候，又会再次指向 0，而可变参数 l 默认指向了 [] 内存地址，内部修改后，还是指向 [] 地址 ，只不过 [] 地址的内部发生了变化，所以这个 l 就发生了变化；

---

## 函数的参数

> argument(实参) -- function 调用时候传入的参数。
> parameter(形参) -- 定义中的命名实体，它指定函数可以接受的一个 argument，或者多个 argument。

## parameter(五种形参形式)

### Positional/default Arguments

最基本的参数，按位置传递就可以了，也可以使用等于号，但是要按照位置参数的位置进行，一般和默认参数一起使用，给定一个默认值，不用传入过多的参数；

默认参数必须是不可变对象，比如字符串，一但是列表等可变对象，很容易出错
，位置参数和默认参数结合，但是默认参数要填在后边，这样解释器不会报错；

位置参数

```python
def test(first, second):
    return first, second

test("python", "java")
test(first='Python', second='Java') # 这两种方式都可以
```

默认参数

```python
def test(first=python, second=java):
    return first, second

test()

def test(first, second=java):
    return first, second

test(golang)
test(first="golang")

```

### 可变参数

如果一个函数传入一个参数，以后拓展性可能不好，不满足需求，比如定义求a*a，但是现在要求 a*a,b*b,c*c，以后可能还有 d*d，那么定义一个参数不能满足以后的要求，还要重新修改，那么可以用 `*`，写成可变参数；

可变参数的默认写法是 `*args`，这个是默认的，可以像下边例子写成 `*number`，这个有个悖论，如果写成*args，其他人能看懂，但是不一定符合逻辑，`*number`，不是默认的推荐，但是能更好的描述这个参数的场景更加的合适；个人推荐使用 `*args`；

传入可变参数，其实本质上这个可变参数就是一个 tuple ，将传入的所有参数保存在一个 tuple 中，再一个个取出来
所以你不传任何参数，也是不会报错的；

```python
def test(a, *number):
    for i in number:
        print(i*i)
    print(a*a)

# 其实上边的参数可以直 接写成下边，a 也不用定义了，
def test(*number):
    for i in number:
        print(i*i)

```

### 关键字函数（Keyword Arguments）

可变参数允许你传入任意参数，包括0个参数，这些可变参数会在调用时候组成一个 **tuple**；

关键字参数和可变参数唯一的不同是，这些参数在调用的时候，会组成一个 **dict**；

关键字参数的意义，在于拓展了函数的功能，比如在 person 中，除了 name、age 传入的参数外，调用者如果希望更多的功能，比如用户注册时候，需要邮箱功能和电话号码，这个时候可变参数就能在不改变原有基础上，进行拓展了；

关键字参数的默认写法是 **kw，不建议写成其他的，注意关键字参数是字典的形式，所以一定是有 key:value 格式参数；

**kw，在函数参数中加星号，调用的时候使用 kw

```python
def person(name, age, **kw):
    # 调用 **kw，使用 kw
    print("name:", name, "age:", age, "other:", kw)

person("suxian", 25)
# name: suxian age: 25 other: {}

person("s", 25, city="nanjing", notion="china",weigt=150)
# name: s age: 25 other: {'city': 'nanjing', 'notion': 'china', 'weigt': 150}

```

### 仅限位置参数

在参数之后传入一个 `/`

这个一般很少用到但是还是有时候还是有一些用处的，这种语法糖就只能传入位置参数，不能乱传，比如传入关键字参数就是不行的；

```python
def test(a, /):
    print(a)

test('this is a') # 只能传入位置参数
test(a='this is a') # 传入关键字参数是不被允许的，会报错
```

### 仅限关键字参数

在参数之前输入一个 `*`

在关键字参数中，我们可以没有限制的传入各种参数，这个导致暴露给使用者的权限过大，需要限制以下，比如 person 中关键字参数只能接受制定的关键字，比如：city、job，这就需要给出规定；**其实主要作用就是限制输入，不要乱传进来东西，导致程序出错**

使用方法就是在位置参数前加上 一个 *，或者是*args，但是这种调用必须要传入参数名，因为前面的 *，是可变参数，就是解释器不知道你穿的关键字参数到底是哪个；

命名关键字参数也可以由默认值

```python
def person(name, age, */*args, city, job):
    print(name, age, city, job)

person("sun",25, city="nanjing", job="coder")
# sun 25 nanjing coder

def person1(name, age, *args, city, job):
    print(name, age, city, job)
    for i in args:
        print(i)

person1("sun",25, 100, 200, city="nanjing", job="coder")
# sun 25 nanjing coder
# 100
# 200
```

### 组合参数

就是上述的很多参数的形式，进行组合但是先后顺序是
必选参数 -- 默认参数 -- 可变参数 -- 命名关键字参数 -- 关键字参数

下边这个例子最好：

```python
def test(a, b, c=100, *args, d, e, **kw):
    print(a, b, c, args, d, e, kw)


test(1, 2, 3, 4, 5, 6, d='d', e='e', y='y', z='z')
```

```python
def test(a, b, c=100, *args, **kw):
    print("a=",a,"b=",b, "c=",c, "args=", args, "kw=", kw)

def test(a, b, c=100, *, d, **kw)
    print("a=",a,"b=",b, "c=",c, "args=", args, "kw=", kw)
```

```python
def test(*args, **kwargs):
    print(args)
    print(kwargs)

if __name__ == '__main__':
    test('a', 'b', 'c', city='shanghai', age=18)
# ('a', 'b', 'c')
# {'city': 'shanghai', 'age': 18}
```

## arguments(2种实参形式)

实参有两种形式：

- 关键字参数
- 位置参数

在调用函数传递实参的时候，一般就是两种状态：位置参数：`a, b, [1, 2]` 和关键字参数`city='nanjing', age=25`

所以传递实参的时候就两种形式而已，不要和形参搞混了；

## Documentation Strings(文档字符串)

文档注释字符串也是有一定的约定俗成的，第一行基本上要简约，已大写开始，句号结尾，简要的描述该对象的作用，简要描述；

> beautiful doc string

**第二行一般是空行**，用来更好的显示注释，因为文档字符串第一行之后的第一个非空行确定整个文档字符串的缩进量，也就是在第一行（带三个引号哪一行）之后的第一行确定了缩进量，所以好的注释表现形式一般是下边的三种形式，这三种形式打印出来的文档注释才好看；

我推荐使用第二种方式，比较符合常理，同时第一种也不错

```python
def test():

    """this is test document

    this is second line document

    """
    pass
# this is test document

#     this is second line document

def test():
    """
    this is test document

        this is second line document
        this is three line document
    """
    pass

def test():
    """ this is test document """
    pass
```

## 函数标注（Function Annotations）

函数标注的内存存在于 FunctionName.__annotations__ 属性中，其实也就是类型提示，便于后续做处理，这个也就是在大型项目中处理动态语言太过于动态的一种方法，更多的是从开发和协作和便利性上；

解释器在执行的时候不会进行任何检查，会直接忽视掉，所有在执行的时候没有任何作用，更多的价值是体现在文档上，方便其他人理解；

```python
def test(name: str, city: str, age: int = 25) -> None:
    print(name, city, age)

print(test.__annotations__)
# test(name: str, city: str, age: int = 25) -> None
```

## 
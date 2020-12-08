# Classes

## Names and Objects

Python 所有的数据都是 Object

多个 names 可以绑定到一个 object，name 在其他语言中也叫别名，其实更容易理解的是指针，name 绑定到任意的 object，和对象之间的关系更像是指针，当 name 赋值给别的 object，name 就会和原来的 object 解除绑定，重新绑定到新赋值的 object，这个也更加符合计算机语言中对变量的定义，这一点和很多静态语言有很大的不同；

### Object三个特征

- id(标识)：对象在内存中的地址，使用 id(object) 查看；
- type(类型)：任何一个对象都有一个类型，type 的类型也是 type，这个类型是不可改变的，int、str、list、dict 一旦确定了就是不能更改的，但是 value 可能会改变；
- value(值)：a = 1，1 就是一个值，1 是一个 int 进行封装的对象，a 是变量，a 指向 1，value 分成 mutable 和 immutable ，可变和不可变在 python 中非常重要，不然会有很多错误；
- Python 是强类型语言，虽然不需要声明数据类型这个语法，但是依然是强类型语言；

```python
a = 1
id(a)
# 1998553200
a = {}
id(a)
# 1521683028296
```

### name

> 不可变类型是安全的，当改变了对象的值，会在内存重新申请内存存储新的对象，而可变对象就不是的，比如 `a=b=[1, 2]`，当 `a.appen(3)`，时候，b 的 value 也会产生改变，因为 name a、b 保存的是 [1, 2] object 的内存地址，但是 list 属于 mutable 类型，在内部方法操作的时候，是不会重新申请内存的，还是保留原来绑定的内存地址(如果动态的不够，重新申请内存，那么原来的绑定对象也会重新更新地址，还是一样的)；

1. `a = 1`
在内存中申请 1 内存，将 name a 绑定到 1 这个对象上，其实 name 就是存储了 1 的内存地址，这个就是指针的概念；
![variable a=1](.\Images\Python-variable-a=1.png)

2. `a = 2`
在内存中申请 2 内存，将 name a 原来绑定到 1 这个对象去掉，重新绑定到 2 上；
![variable a=2](.\Images/Python-variable-a=2.png)

3. `a = b`
`a = b`，其实是赋值语句，也就是a 存储的 2 内存地址赋值给 b，这样子，a、b 都绑定了 2；
![variable](.\Images\Python-variable-a=b=2.png)

4. `a = b = [1, ]`
    可变类型的性质就会导致很多不安全的问题，使用时候一定要注意；

    ```python
    a = b = [1, ]
    print(b) # [1, ]
    a.append(2)
    print(b) #  [1, 2]
    ```

## Class Definition Syntax(类定义)

类定义语法

```python
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

1. 类和函数一样，只有在被调用的时候才会生效，相应的**类可以被定义在 statement 中或者 function 中**；
2. 进入类定义的代码时候，将会重新创建一个新的 name spaces，也就是这个类可以堪称一个容器，这个容器里是新的 name spaces，当类调用完成，会恢复原来的 name spaces；

## class object(类对象)

类对象，类对象支持两种操作：属性引用和实例化

### 类对象的属性引用

语法：object.name
name，可以是数据属性可以是方法，其实都是属性引用，类属性可以被赋值，不管是属性还是方法，都可以被重新赋值，这也就意味着原来的类对象被改变了，后续再使用或者实例化这个类对象时候，这个对象的属性都有可能被更改了；

```python
class Test(object):

    a = 100

    def test(self):
        print('class object Test')


if __name__ == '__main__':
    def test():
        print('this is function')

    print(Test.a) # 100
    print(Test.test)

    Test.a = 1000 # 数据属性重新赋值
    Test.test = test # 方法重新赋值

    print(Test.a) # 1000
    print(Test.test)
    Test.test() # this is function

    a = Test() # 重新实例化时候，已经被更改了
    print(a.a) # 1000
```

### 类对象的实例化

语法：object()，这样的形式就是实例化，如果 __init__(self) 有参数就传入初始化参数，这样子就是实例化一个对象；

## Instance Objects(实例对象)

实例对象做什么？ **实例对象理解的唯一操作是属性引用**。 有两种有效的属性名称：数据属性和方法。
实例对象可以对数据属性进行赋值，类似 `person_one.city = 'shanghai'`，这个数据属性可以在类变量和实例变量中定义过，也可以没有定义过；
实例对象的方法，其实就是类中的函数；

## method Object(方法对象)

我们通常实例化的时候，都会调用方法，比如 person_one.age() ，其实在定义的时候如果没有其他参数，应该是：

```python
class Person()

    def age(self):
        pass
```

但是在调用的时候，却不用加一个 self 参数，这是因为 self 就是实例化的本身，其实 persopn.age() == Person.age(person_one)

## 类和实例变量

> 类变量是类所有的实例共享的属性和方法，实例变量是每个实例的唯一数据；

但是在 mutable 数据时候，就会出现很大问题，一旦某个实例或者内部方法修改了类变量，这个类变量就更改了，所有实例共享的变量发生了变化，这很危险的，如果有场景需要这样子使用，可以使用，但是一般情况下，都要尽量避免，尽量把 mutable 数据定义在实例变量中，这样子是安全的；

```python
class Student():

    person = True # 类变量一般都是要定义不可变类型，这样子实例修改了，不会影响其他实例

    def __init__(self, name, age):
        # 实例.属性 = 传入的参数
        self.name = name
        self.age = age
        self.city_list = [] # 可变类型的变量使用实例变量的方式

    def stay_city(self, city):
        self.city_list.append(city)
```

1. 类属性和实例属性的查找顺序
实例的查找顺序也可以解释为什么类变量要使用 immutable 数据类型，因为实例修改了类属性，属于自身的类属性，查找的时候会先从自身进行查找，修改后就直接找到了修改后的值，而其他实例没有修改的时候，还是找的上一个值，可变的数据类型就不一样了，比如 city = []，进行了修改，city 还是保存原来的内存地址，但是这个内存地址的 value 发生了变化，所以类变量自然也就产生了变化；
__dict__ 可以查看某个对象的所有的属性，返回一个字典
如果不存在实例属性，那么应该打印空，为什么打印了类属性，这就是往上查找，实例没有会往类中查找，类中没有会往父类进行查找，直到查找不到才会抛出异常；

2. 在实例方法中访问实例变量和类变量

```python
class Student():
    sum1 = 0
    def __init__(self,name1,age):
        self.name = name
        self.age = age
        # 实例方法中访问类属性
        # 第一种方式
        print(Student.sum1)
        # 第二种方式
        print(self.__class__.sum1)
```

## 类函数可以调用类属性/函数

类的函数，可以使用 self，对类内的属性/函数进行调用

```python
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)
```

## Private variable(私有变量)

- 公开：外部能直接访问
- 私有：外部不能直接访问，只允许类本身进行访问
- 受保护：是能自身和子类进行访问

以单下划线开头的成员变量或方法，是设计者不想暴露给外部的API，使用者不应该进行访问，只有类实例和子类实例能访问到这些变量，这个是默认的但是还是可以访问的，所以这种写法更多的是约定俗成，在 Python 程序员中遵循这个规范，至于要强制不能访问，还要通过其他强制手段进行；

Python 中没有 public、private 概念，外部的调用可以随便的修改内部的属性，类对象自己能访问，连子类对象也不能访问到这个数据，这样是不安全的，所以就有一个私有变量的概念，如果属性名字前加`__`，那么就变成了一个私有变量，**只有内部可以访问，外部不能访问，如果直接访问会报错，但是还是可以访问到`_Person__age`，这个属性可以访问到，但是不建议这样做，但是也客观说明了，只要是想修改还是可以修改的；**在 Python 中可以实现(name mangling这种形式实现的)，但是是不安全的；

### name mangling(名称改写)

Python 中支持私有的实现方法是 name mangling(名称改写)，也就是让原来 classname.method 调用变成了 _classname__method，是一种不安全的实现，只是表面上提供了一种方式进行私有的支持；

如果需要访问属性值，可以使用 method，比如 getName() method，返回一个属性值

如果需要修改属性值，可以使用 method，如果 setName() method，进行修改，返回也可以不返回属性值，这个方法，**可以添加一些逻辑进行参数的校验和处理，增加了安全性**，如果修改前获取的值存在，那么需要再重新获取一遍；

示例

```python
class Person():
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    def getAge(self):
        return self.__age

    def getName(self):
        return self.__name

    # 增加了参数的校验
    def setAge(self, age):
        if age > 20:
            self.__age = age
            return self.__age
        else:
            return "请输入大于 20 的岁数"

    def setName(self, name):
        self.__name = name

tom = Person("tom", 25)
tom_age = tom.getAge()
tom_name = tom.getName()
tom.setAge(30)
print(tom.getAge())
tom.setName("James")
print(tom.getName())
```

## dir() 函数

dir(object):获取一个对象的所有属性和方法，并返回一个包含字符串的 list，

随便一个字符串，dir() 函数，会写出所有的字符串的属性值，哪怕没有定义

```python
print(dir("a"))
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']

class A():
    pass
print(dir(A))
# 可以看到虽然是一个新建的类，没有任何属性，但是会带有一些自带的默认的方法  
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

## 静态方法、类方法和实例方法

### 静态方法@staticmethod

静态方法是类中的函数，不需要实例。静态方法可以被实例调用，也可以被类调用。

静态方法一般用来储存一些和逻辑型代码，不会涉及到类中的属性和方法操作，静态方法是一个**独立、单纯**的函数；

```python
class TestTime():
    # 该类你传入什么时间就显示什么时间
    def __init__(self):
        pass

    # 和类没啥关系，比如这个可能会和输入时间做对比，但是和类本身没关系
    @staticmethod
    def showtime():
        return time.strftime("%H:%M:%S",time.localtime)
```

### 类方法@classmethod

类方法第一个参数必须是当前类对象，一般是 `cls`，通过她来传递类属性和方法(不能传输实例方法和属性)，可以被类和实例调用；

一般使用的时候就是在逻辑上调用类本身作为对象调用最为合理，就可以定义这个方法是类方法；

### 实例方法

实例方法第一个参数比如是实例对象，一般是 `self`，通过她来传递实例的属性和方法，只能被实例对象调用，不能被其他调用，这个也是最常用的；

## 继承

继承是面向对象设计中很重要的一个概念；

### 继承父类

OOP 程序中，定义一个类，可以从现有的 class 中继承，新的类称为`子类（subclass）`，而被继承的类称为`父类也叫基类（baseclass）`，最顶端的类称作`超类(superclass)`

Peroson 是父类，Jiangsu 是子类，继承于 Person，所以哪怕是写了一个 pass，那么Jiangsu 这个类也是有父类的**全部功能**，包括属性(实例属性和类属性)和方法；

### 多重继承

多重继承是一个比较难理解的概念，这里暂时先不处理；

## @property 动态属性

property: 属性

简单的理解就是：计算属性，

### 作用

1. Python 的类属性是会被轻易修改的，当我们需要对属性添加一些逻辑操作的时候，这个没有办法操作，但是在 method 中就可以添加大量的操作，但是这个时候需要调用 method 不方便，添加 @property 就可以直接当作属性来调用了！

2. Python 的类属性，很多时候是不安全的，很多设计会用类似：get_name/set_name 这种 method 进行限制，但是每个属性这样子写会很麻烦，这个时候就需要装饰器了，@property 就可以对类属性进行校验和访问，同时还能使用属性的方式进行访问；其实功能都是一样的，只是处理的更加优雅；

总结：@property 广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性。

### 使用

setter：读写属性
getter：其实这个不需要定义，因为直接调用之前 @property 的就可以了，不定义setter方法就是一个只读属性；

```python
class User(object):
    def __init__(self, name: str, age: int):
        self._name = name
        self.age = age

    def __str__(self):
        return "name: {}, age: {}".format(self._name, self.age)

    # 注意这三个方法的名字都是一样的
    @property
    def name(self):
        if self._name == "James":
            print("姓名不能是 James")
        else:
            return self._name

    @name.setter  # 读写属性
    def name(self, set_name: str):
        if set_name.isdigit():
            return "设置错误"
        self._name = set_name


if __name__ == '__main__':
    my = User("Tom", 26)
    print(my)
    my.name = 'James'
    print(my) # 姓名不能是 James
```

## __slots__ 限制实例属性

Python 是一个动态语言，所以在运行的时候允许动态绑定属性和方法

1. 动态绑定属性；
2. 动态绑定方法；

如果不想让类和实例在运行过程中动态的绑定任何方法，那么就需要限制，在定义 class 的时候，列出来哪些属性可以绑定，其他没有列出的就不能绑定！__slots__ 其实相当于白名单的作用！

优点

1. 作为一个封装的工具，防止用户对属性进行随意的添加；
2. 节省内存和加快性能，__slots__ 使用后，__dict__ 方法就会失效，以往的实例创建，实例的信息是通过 __dict__ 来进行存储的，现在不会进行，这样子可以加快实例化已经占用的空间，但是会牺牲很多特性，需要谨慎使用！
3. 你应该只在那些经常被使用到的用作数据结构的类上定义slots (比如在程序中需要创建某个类的几百万个实例对象)！

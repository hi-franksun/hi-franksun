

Python 目录：

- Include：该目录包含了python所提供的的所有头文件，如果用户需要自己使用C或者C++来编写自定义模块扩展python，那么就需要用到这里的头文件，（可以简单理解基本上就是 Cpython 源码）

- Lib：该目录包含了python自带的所有标准库，Lib中的库基本上都是使用python编写的；
- Tools：工具，一些demo、国际化、脚本


## PyObject

PyObject的定义，python 中一切皆为 PyObject

```c
//object.h
typedef struct _object {
    _PyObject_HEAD_EXTRA  
    Py_ssize_t ob_refcnt;  //引用计数器，和内存回收有关
    PyTypeObject *ob_type; //定义Python对象的元类信息
} PyObject;

```




```c
//object.h
typedef struct {
    PyObject ob_base;  // 可以简单的理解为 PyObject，继承了她的结构体
    Py_ssize_t ob_size; // 变长对象，对象中容纳了多少个元素，其实就是 len() 走的捷径
} PyVarObject;
```







像str、tuple、list之类的实例对象，它们有一个共同的特点，就是它们都具有长度的概念，都可以使用len方法计算长度。这些实例对象可以看成是一个容器，里面可以容纳元素。因此像这样的对象，我们称之为变长对象，那么在底层就还需要一个变量，来专门维护变长对象的长度。而这个维护长度的变量和 `PyObject` 组合起来就是 `PyVarObject`。

PyVarObject




PyObject


PyObject_HEAD

PyObject_HEAD就是一个PyObject实例


PyVarObject
```c
typedef struct {
    //一个PyObject
    PyObject ob_base;
    //一个ob_size,变长对象的长度
    Py_ssize_t ob_size; 
} PyVarObject;
```

PyObject_VAR_HEAD

PyObject_VAR_HEAD是一个PyVarObject实例，实例的名字都叫ob_base。另外在C中，一个Py_ssize_t占8个字节，一个指针也占8个字节，所以一个变长对象的大小至少占24个字节，至于具体占多少，则由其它的结构体成员决定了。


PyTupleObject
tuple 真正的实现



resize：调整大小


```python
tup = ()
print(tup.__sizeof__())  # 24 byte

tup1 = (1, )
print(tup1.__sizeof__())  # 32 byte

tup1 = (1, 2)
print(tup1.__sizeof__())  # 40 byte
```

其实也就是 tuple element 多一个，就会多 8 byte

那么原始的 24 byte 是什么：

Py_ssize_t：8 byte，维护长度

| 48    | tuple      |  +8 per additional item
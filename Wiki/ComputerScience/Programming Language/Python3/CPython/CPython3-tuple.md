#CPython #Python3 #tuple


CPython 中，tuple 和 list 相似，本质也是一个 array，但是空间大小固定。不同于一般 array，Python 的 tuple 做了许多优化，来提升在程序中的效率。

## Cpython 中定义的 tuple 结构
```c
typedef struct {
    PyObject_VAR_HEAD
    PyObject *ob_item[1];
} PyTupleObject;
```

![Cpython tuple 结构](CPython_tuple.png)


## tuple 的内存大小
空 tuple 是 24 Byte，每个 item 是 8 Byte，所以是 24 + 8*item

```python
a = ()
a.__sizeof__()  # 24
b = (1, )
b.__sizeof__()  # 32
```

tuple 初始化占用内存比 list 少，性能会好不少，而且因为不可变，不存在扩充后，重新开辟内存的操作，整体性能要比 list 快的；


## free list(缓冲池) 的机制

tuple 和 list 不同是没有没有 over_allocate 加载的结构，但是有一个 free list 的缓冲池概念（了解即可）。

举个例子，当 tuple 的大小不超过 20 时，Python 就会把它缓存在内部的一个 free list 中。这样，如果你以后需要再去创建同样的 tuple，Python 就可以直接从缓存中载入，提高了程序运行效率。








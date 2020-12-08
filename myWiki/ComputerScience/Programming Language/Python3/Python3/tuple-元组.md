# tuple

## what tuple

tuple 可以通过索引进行访问，是[[sequence 序列]]，又可以容纳不同类型的 elements，所以是容器序列类型;

## why tuple

tuple 是 immutable 类型，本质上和 list 都是属于 array，在 CPython 的实现中，也是用的 Array 方式实现的，所以本质的区别就是 immutable，当然性能比 list 好也是优点，特别是数据量非常大的时候；

## create tuple

实现 tuple 语法是 `,`，而不是 `()`，括号只是一个可选项；

1. () 创建；
    1.1 0 个元素，直接用 () 创建或者 tuple() 常见；
    2.2 1 个元素，要加个 (1, )，逗号创建，不然会被误认为计算中的括号；

2. 元组打包；

3. 嵌套元组元素；  
    3.1 元组作为序列容器可以容纳各种类型，所以可以嵌套各种元素；

    ```python
    a = (1, 2, 3)
    b = a, (4, 5, 6)  # ((1, 2, 3), (4, 5, 6))
    ```

4. 元组变元组；
  4.1 元组是不可变类型（如果内部元素是可变的，只是这个元素可以发生变化，不代表这个元组发生了变化，元组还是那个数组，只是内部可变 element 的指针发生变化，），所以不能修改元组，只能是重新建立一个元组；

  ```python
  a= 1, 2
  print(a)
  b = 1, 'a'
  print(b)
  c = 1234, '1234', (1, 2, 3, 4), [1, 2, 3, 4], {1: '2', 3: '4'}
  print(c)
  d = (1,)
  print(d)
  tup = tuple()
```

> 创建空 tuple 建议直接使用 a = ()，而不是 a=tuple()，因为牵扯到调用 tuple() 函数，所以会慢一点；

```bash
sunyunxian@sunyunxian:/mnt/c/Users/sunyunxian$ python3 -m timeit 'a = ()'
100000000 loops, best of 3: 0.012 usec per loop
sunyunxian@sunyunxian:/mnt/c/Users/sunyunxian$ python3 -m timeit 'b = tuple()'
10000000 loops, best of 3: 0.0796 usec per loop
```

## tuple methods

1. count(item)
    获取当前值出现的次数，如果不存在会返回 0

    ```python
    count(item) -->int

    a = (1, 2, 3, 4, 4)
    a.count(4)  # 2
    ```

2. index(item, start, stop)
    获取 item 的 index，默认返回第一个 index，获取第一个匹配的值的索引，如果不存在会  `Raises ValueError if the value is not present`

    ```python
    index(item, start, stop) -->int
    Raises ValueError if the value is not present

    a = (1, 2, 3, 4, 4)
    print(a.index(4, 1, len(a)-1))  # 3 index 是 3，返回第一个匹配的值
    ```

3. del tuple
    tuple 是不可变的，所以只能用 `del`，删除整个 tuple；

4. modify tuple
tuple 是不可变的，所以不能进行内部增删改（item是可变对象是可以修改的）的操作，一般新建一个 tuple，就是在原来 tuple 基础上，使用相同的变量进行

    ```python
    a = (1, 2)  # a = (1, 2)
    a = a + (3, )  # a = (1, 2, 3)
    ```

## sort tuple

sorted(tuple) 会返回一个 new sorted tuple

## tuple be used

- tuple is immutable list（这句话就说明了主要用途，同时也是最重要的说明，immutable 意味线程安全，可以被 hashable，这一点非常重要）
- list 通常用来存放相同类型的 elements，tuple 常被用来存放不同类型 elements；
- tuple 是 immutable，在 Cpython 设计中，内存占用较少，性能会好一些；
- tuple 有记录功能，tuple 中每个字段位置是固定的，当赋予每个字段的信息时候，这个 tuple 用途就非常广泛了；
- tuple 一般是解包/索引进行访问，list 一般通过迭代进行访问，如果使用 namedtupe 则意味这可以用属性进行访问（通过名称访问相比通过索引访问更清晰，当元素很多的时候，使用 namedtuple 更加方便点，因为清晰明了，但是本质上和使用 tuple 是一样的）；

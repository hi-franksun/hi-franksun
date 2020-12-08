# list

## what list

list（列表）是 python 内置的一种数据结构，是内置可变序列类型（Built-in mutable sequence），可以随时添加删除其中的元素；

## create list

支持直接创建，这种创建方式也是速度最快的；

```python
city = ["beijing", "shanghai", "nanjing"]
city = []
city = list() # 这个会调用 list 函数，并没有直接的快
```

## list function

- cmp(list1,list2):比较两个 list 元素
- len(list1):list 元素个数
- max/min(list1):返回列表元素最大/小值
- list(seq):将 tuple 转化成 list

## list methods

insert、append、remove等都不会有返回值，而是返回 None，只是修改了原来的 list，这是 python 中可变数据结构的设计原则；

1. append(item)
    在列表的末尾添加新的对象，无返回值，会修改原来的列表(Append object to the end of the list)，时间复杂度是 O(1)；

    ```python
    list.append(object) -> None:
    a = [1, 2, 3]
    a.append(4) # [1, 2, 3, 4]
    a.append("1") # [1, 2, 3, 4, '1']
    ```

2. insert(index, item)
    将指定对象插入列表的指定位置，可以创建一个空列表，对两个列表进行一下插入操作的重新排序，一般不会插入到 tail，因为有 append ；

    ```python
    insert(index: int, object) -> None
    Raises ValueError if the value is not present.

    a = [1, 2, 3, 1]
    a.insert(0,"test") # ['test', 1, 2, 3, 1]
    a.insert(0,["test0","test1"]) # [['test0', 'test1'], 'test', 1, 2, 3, 1]
    ```

3. extend(iterable)

    在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）,可迭代对象会进行遍历以此添加，其实就是相当于 append 多个 element，相当于 a[len(a):] = iterable，只是这样子更加方便了；

    ```python
    list.extend(iterable) -> None

    a = [1, 2, 3]
    a.extend([4, 5, 6]) # [1, 2, 3, 4, 5, 6]
    a.extend('a') # [1, 2, 3, 4, 5, 6, 'a']
    a.extend("bcd") # [1, 2, 3, 4, 5, 6, 'a', 'b', 'c', 'd']
    ```

4. remove(item)
    移除列表中某个值的第一个匹配项，只会删除第一个匹配项，如果不存在，会报错 `ValueError: list.remove(x): x not in list`，需要做异常处理；

    ```python
    list.remove(item) -> None
    Raises ValueError if the value is not present.
    a = [1, 2, 3, 1]
    a.remove(1) # [2, 3, 1]
    a.remove(5) # ValueError: list.remove(x): x not in list
    a = [[1, 2], 1, 2, 3, 1] # 支持所有对象
    a.remove([1, 2] #[1, 2, 3, 1]
    ```

5. count(item)

    统计列表中元素出现的次数，返回整型（Return number of occurrences of value），没有 element 返回 0；

    ```python
    count(item) -> int

    a = [[1, 2], 1, 2, 3, 1]
    result = a.count(1) # 2
    result = a.count([1, 2]) # 1
    ```

6. index(item, start, end)
    找出第一个匹配项的索引，返回整型（Return first index of value.   Raises ValueError if the value is not present.）；

    ```python
    index(object, start: int =, stop: int =) -> int

    a = [1, 2, 3, 1]
    result = a.index(1) # 0
    result = a.index(1, -1) # 3
    result = a.index(1, 1, len(a)) # 3
    ```

7. pop(index)
    删除某个索引的元素，并返回该删除元素的值，默认删除最后一个，index is empty or
     index is out，Raises IndexError；

    ```python
    pop(index) -> element of index
    Raises IndexError if list is empty or index is out of range.

    a = [1, 2, 3, 1]
    a.pop() # [1, 2, 3] default last
    a.pop(1) # [1, 3]
    ```

8. clear()
    delete all elements 相当于 `del a[:]`

9. copy()
    返回一个浅拷贝，等价于 `a[:]`

10. reverse()
    对 list 元素进行反向排序；

    ```python
    list.reverse(self) -> None

    a = [1, 2, 3]
    a.reverse() # [3, 2, 1]
    ```

11. sort(key=None, reverse=False)
    对原列表进行排序，如果指定参数，则使用比较函数指定的比较函数，默认是升序；

    ```python
    def sort(*, key: Optional[Callable[[_T], Any]] = ..., reverse: bool=False) -> None
    # cmp -- 可选参数, 如果指定了该参数会使用该参数的方法进行排序。
    # key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
    # reverse -- 排序规则，reverse = True 降序， reverse = False 升序（默认）

    a = [1, 2, 3]
    a.sort(reverse=True) # [3, 2, 1]

    ```

    > list.reverse() 和 list.sort() 分别表示原地倒转列表和排序（注意，元组没有内置的这两个函数)。
    > reversed() 和 sorted() 同样表示对列表 / 元组进行倒转和排序，但是会返回一个倒转后或者排好序的新的列表 / 元组。

## 使用 array 和 pickle

list 虽然灵活，但是内存消耗教大，且速度很慢，比如 list 存储一个浮点数，

## 使用 deque

当频繁的使用 FIFO 时候，应该优先使用 deque，速度会很快；

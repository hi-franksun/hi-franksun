# set

set（集合），是一组 key 的集合，不存储 value，所以 key 不能重复，可以用来检测 list 是否有重复元素；

常见的用途包括成员检测、从序列中去除重复项以及数学中的集合类计算，例如交集、并集、差集与对称差集等等

> 因为是无序的集合，所以不会记录 element 的位置和 insert 顺序，所以很多index、slice、common sequence 操作都不能通用在 set 上。

## create set

set 接收一个 list 对象，将 list 中相同的元素删除，形成一个 set，set 是用花括号进行表示的。

```python
set1 = set([1,2,3])
print(set)
# {1,2,3}
```

### create empty set

```python
set1 = {}
# 其实是个 dict 类型
set1 = set()
# 这样子才是一个 set 类型
```

## set methods

0. set.pop()
    **随机删除**一个 element，
    return：delete element/KeyError

    ```python
    a = {'age', 'name', 'city'}
    print(a.pop()) # age
    ```

1. set.copy()
    return 浅拷贝

2. set.add(element)
    增加 key 元素，相同的不会添加
    return None

    ```python
    a = {'age', 'name', 'city'}
    a.add('element1')
    ```

3. set.remove(element)
    删除 key 元素
    return KeyError/None

    ```python
    a = {'age', 'name', 'city'}
    a.remove('age'))
    print(a) # {'name', 'city'}
    ```

4. set.discard(element)
    删除 element，没有 element 则不进行任何操作；
    return None

    ```python
    a = {'age', 'name', 'city'}
    a.discard('age'))
    print(a) # {'name', 'city'}
    a.discard('element') # element 不存在，不会有任何提示
    print(a) # {'name', 'city'}
    ```

5. set.clear()
    delete all element，会变成空的 set
    return None

## 数学意义（很重要）

1. 求两个集合的差值：
    {1,2,3,4,5} - {3,4} 就能求出差值 {1,2,4,5}

2. 求两个集合的交集
    {1,2,3,4,5} & {2,4} 就能求出交际 {3,4}

3. 求两个集合的并集：
    {1,2,3,4,5,6} | {3,4,7} 就能求出并集 {1,2,3,4,5,6,7}

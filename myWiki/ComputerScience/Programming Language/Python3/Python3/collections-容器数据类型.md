# collections-容器类型

实现了特定目标的容器，以提供Python标准内建容器 dict/list/set/tuple 的替代选择。

## namedtuple()：命名元组

命名元组赋予每个位置一个含义，提供可读性和自文档性。它们可以用于任何普通元组，并添加了通过名字获取值的能力，通过索引值也是可以的。

> 也就是命名元组获取值时候，又三种方式：索引、属性（）、解包；

创建命名元组子类的工厂函数

```python
namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)

# typename:类名
# field_names:字段参数：多个字符串组成的可迭代对象/一个正字符串已空格分开

from collections import namedtuple

City = namedtuple('City', 'name country number')  # 空格分隔的字符串
city0 = City('beijing', 'China', '010')  # City(name='beijing', country='China', number='010')

City1 = namedtuple('City', ['name', 'country', 'number'])  # 多个字符串对象组成的可迭代对象
city1 = City1('nanjing', 'China', '021')) # City(name='nanjing', country='China', number='021')

# 访问方式，
print(city0[0])  # 序列访问
print(city0.name)  # 属性访问，更加符合当初设计的需求

```


命名元组尤其有用于赋值 csv sqlite3 模块返回的元组
```python
EmployeeRecord = namedtuple('EmployeeRecord', 'name, age, title, department, paygrade')

import csv
for emp in map(EmployeeRecord._make, csv.reader(open("employees.csv", "rb"))):
    print(emp.name, emp.title)

import sqlite3
conn = sqlite3.connect('/companydata')
cursor = conn.cursor()
cursor.execute('SELECT name, age, title, department, paygrade FROM employees')
for emp in map(EmployeeRecord._make, cursor.fetchall()):
    print(emp.name, emp.title)
```

是他常用方法和属性

_make(iterable)类方法：其实就是传值，
_asdict()实例方法 ：返回一个  OrderedDict 类型，可以利用 dict api 对元素进行操作；
_fields 类属性：返回所有的属性，tuple 形式返回

```python
City = namedtuple('City', 'name country number')
city_data = ('shanghai', 'china', '025')
city = City._make(city_data)  # City(name='shanghai', country='china', number='025')
# 以上这个操作等同于：city = City(*city_data)

print(city2._asdict())  # OrderedDict([('name', 'shanghai'), ('country', 'china'), ('number', '025')])

for key, value in city2._asdict().items():
    print(f'{key} : {value}')
# name : shanghai
# country : china
# number : 025
```


转换一个字典到命名元组，使用 ** 两星操作符 :

```python
d = {'x': 11, 'y': 22}
Point(**d)
Point(x=11, y=22)
```


## Counter 
dict 子类，提供可哈希对象的计数功能


## deque

双端队列，类似 list 的容器，实现两端快速添加（append）和弹出（pop）
deque 继承了多数的 list 方法，但是优化的首尾添加和弹出操作pop(0) 和 insert(0, v) 的开销为 O(1)，导致从中间删除元素会慢一些，而且索引访问也是 O(n) 复杂度，对随机访问频繁的操作，最好使用 list。

append 和 popleft 是原子级别操作，所以先进先出的 slack 时候，可以保证线程安全，不用担心资源锁问题；

```python
deque([iterable[, maxlen]]) --> deque object
# iterable：迭代类型
# maxlen：最大长度，默认是没有最大长度的；
# 当超过最大长度的时候，数据会从另一端弹出
```

### deque methods

1. append(x)
2. appendleft(x)
3. pop()
4. popleft()
5. extend(iterable)
6. extendleft(iterable)
7. rotate(n)
    循环 n step，（default n=1)，正整数是向右循环，负数时向左循环；

    ```python
    from collections import deque
    # 正数
    dq = deque(range(6))
    print(dq)  # deque([0, 1, 2, 3, 4, 5])
    dq.rotate(2)
    print(dq)  # deque([4, 5, 0, 1, 2, 3])
    # 负数
    dq = deque(range(6))
    print(dq)  # deque([0, 1, 2, 3, 4, 5])
    dq.rotate(-2)
    print(dq)  # deque([2, 3, 4, 5, 0, 1])
    ```

8. maxlen：可读属性，deque 最大尺寸，没有限定就是 None
clear()：删除元素

## defaultdict

字典的子类，提供了一个工厂函数，为字典查询提供一个默认值

## OrderedDict

有序字典，3.7 已实现了 dict 的有序，所以此模块用的较少，但在一些插入排序上性能还是比较好的；

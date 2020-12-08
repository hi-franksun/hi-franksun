


[Python 数据类型时间复杂度分析](https://wiki.python.org/moin/TimeComplexity)


## list
list 是有序且可变的集合，适合用作堆栈和队列，底层的实现是动态 array，具体实现可见 [[Python3 list 实现]]


## collections.deque

| 操作 | Api | 复杂度|
| -- | -- | -- |
| get length获取长度 | len() | O(1) |
| insert tail 尾部插入 | append() | O(1) |
| Pop last 尾部删除 | pop() | O(1) |
| Get Item 获取元素 | list[0]| O(1) |
| Set Item 修改元素| list[0] = element | O(1) |
| Insert item 插入元素 | insert() | O(n) |
| Delete Item 删除元素 | | O(n) |
||||||||||||||||||||||||||

| **Operation** | **Average Case** | **Amortized Worst Case** |
| -- | -- | -- |
| append | O(1) | O(1) |
| appendleft | O(1) | O(1) |

pop

O(1)

O(1)

popleft

O(1)

O(1)

extend

O(k)

O(k)

extendleft

O(k)

O(k)

rotate

O(k)

O(k)

remove

O(n)

O(n)

Copy
O(n)

O(n)

## set


## dict



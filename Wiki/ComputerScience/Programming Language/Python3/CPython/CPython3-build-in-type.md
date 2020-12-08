## Python3 中内置数据类型的大小

根据 Python 3 的文档，各内置类型的内存大小如下：

| Bytes | type       |  scaling notes|
| -- | -- | -- |
| 28    | int        |  +4 bytes about every 30 powers of 2
| 37    | bytes      |  +1 byte per additional byte
| 49    | str        |  +1-4 per additional character (depending on max width)
| 48    | tuple      |  +8 per additional item
| 64    | list       |  +8 for each additional
| 224   | set        |  5th increases to 736; 21nd, 2272; 85th, 8416; 341, 32992
| 240   | dict       |  6th increases to 368; 22nd, 1184; 43rd, 2280; 86th, 4704; 171st, 9320
| 136   | func def   |  does not include default args and other attrs
| 1056  | class def  |  no slots
| 56    | class inst |  has a __dict__ attr, same scaling as dict above
| 888   | class def  |  with slots                                                                         |
| 16    | __slots__  |  seems to store in mutable tuple-like structure first slot grows to 48, and so on. |

上述文档中没有提到对象引用。对象引用占用的内存大小取决于操作系统，在 64 位系统中为 8 字节；

了减小每次增加 / 删减 操作时空间分配的开销，Python 每次分配空间时都会额外多分配一些，这样的机制 （over-allocating：过度加载）保证了其操作的高效性：增加 / 删除的时间复杂度均为 O(1)

空 list 也会有 40 字节的占用，这个在 C 实现的时候，保存的一些信息，包括指针



```python
# coding: utf8

#  python 性能测试模块

from timeit import Timer


'''
class Timer:
    def __init__(self, stmt="pass", setup="pass", timer=default_timer,
                 globals=None):

def timeit(stmt="pass", setup="pass", timer=default_timer,
           number=default_number, globals=None):
'''


def test0():
    l = []
    for i in range(1000):
       # 最慢，+ 号会每次生成一个新列表，空间上消耗多
       l = l + [i]


def test1():
    l = []
    for i in range(1000):
       l.append(i)


def test2():
    l = [i for i in range(1000)]


def test3():
    l = list(range(1000))


def test4():
    l = []
    for i in range(1000):
        # 比 + 快，l不会生成新的列表，而是再原来的列表上加的，空间消耗少
        l.extend([i])


def test5():
    l = [1, 2, 3]


def test6():
    l = list("123")


if __name__ == "__main__":

    t0 = Timer("test0()", "from __main__ import test0")
    result0 = t0.timeit(1000)
    print(result0)
    t1 = Timer("test1()", "from __main__ import test1")
    result1 = t1.timeit(1000)
    print(result1)
    t2 = Timer("test2()", "from __main__ import test2")
    result2 = t2.timeit(1000)
    print(result2)
    t3 = Timer("test3()", "from __main__ import test3")
    result3 = t3.timeit(1000)
    print(result3)
    t4 = Timer("test4()", "from __main__ import test4")
    result4 = t4.timeit(1000)
    print(result4)
    t5 = Timer("test5()", "from __main__ import test5")
    result5 = t5.timeit(1000)
    print(result5)
    t6 = Timer("test6()", "from __main__ import test6")
    result6 = t6.timeit(1000)
    print(result6)

# 5.9860418
# 0.28568079999999973
# 0.1421606999999998
# 0.05804739999999953
# 0.4377943000000002
# 0.000501899999999722
# 0.001151799999999703


```


 [[CPython]] 
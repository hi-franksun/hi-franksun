

#data_structure #queue

queue 和 stack 一样，都是操作受限的线性表数据结构：queue：先进先出，first in first out（FIFO）

queue：主要有两个操作：
- 入队：enqueue 放一个数据到队尾 tail
- 出队：dequeue 取一个队头部的数据 head

![queue 数据结构](/Images/queue_structure.png)


### 顺序队列
array 实现的队列叫做顺序队列


### 链式队列
linked list 实现的队列叫做链式队列

### 双端队列
双端队列：deque（double-ended queue），双端队列可以再两端都进行出队和入队！

### 循环队列


## 阻塞队列和并发队列

### 阻塞队列，
其实就是生产者-消费者模型，当 queue 满的时候，enqueue 就要等 queue 不满的时候在入队，当 queue 空的时候，dequeue 就要等 enqueue 入队，queue 有了 element 时候再 dequeue，这个模型满足生产者-消费者模型；



---

## python

x
顺序表实现顺序 queue

```python
class Queue(object):
    """ queue """

    def __init__(self):
        self.__item = []

    def enqueue(self, item):
        return self.__item.append(item)

    def dequeue(self):
        if self.is_empty():
            return 'queue is null value'
        else:
            return self.__item.pop(0)

    def is_empty(self):
        return self.__item == []

    def size(self):
        return len(self.__item)


if __name__ == '__main__':
    queue = Queue()
    print(queue.size())
    print(queue.is_empty())
    print(queue.enqueue(1))
    queue.enqueue(2)
    queue.enqueue(3)
    queue.enqueue(4)
    print(queue.dequeue())
    print(queue.dequeue())
    print(queue.dequeue())
    print(queue.size())

```

---

python 实现 deque，但是 python 中已经存在一个 deque 容器 ，在 collections 类型中，可以实现两端的  pop和 append，一般使用这个标准库的双端队列；

```python

class Deque(object):
    """ deque """

    def __init__(self):
        self.__item = []

    def left_enqueue(self, item):
        return self.__item.insert(0, item)

    def right_enqueue(self, item):
        return self.__item.append(item)

    def left_dequeue(self):
        if self.is_empty():
            return 'deque is null value'
        else:
            return self.__item.pop(self.size() - 1)

    def right_dequeue(self):
        if self.is_empty():
            return 'deque is null value'
        else:
            return self.__item.pop(0)

    def is_empty(self):
        return self.__item == []

    def size(self):
        return len(self.__item)


if __name__ == '__main__':
    queue = Deque()

```
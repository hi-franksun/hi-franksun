
#data_structure #stack

## what stack

堆栈（stack）称为栈，是计算机科学中的一种抽象数据类型，只允许在有序的线性资料集合的一端（称为堆栈顶端-top）进行加入数据（push）和移除/弹出数据（pop）的运算。因而按照后进先出（LIFO, Last In First Out）的原理运作。

![Stack Structure](/Images/stack_structure.png)

stack 是一种抽象数据结构，主要的特征是 push and pop，array/lined list 也能满足这种描述，但是 array/lined list 内部操作过多，暴露更多的接口，复杂就会导致出错，stack 更加单纯，符合 stack 的场景就可以优先使用；

> stack 的出现就是一种设计，数据类型复杂和灵活会导致很多成本，更加简单的数据类型能简化某种抽象，专注，就是设计问题；



## stack type

stack 的实现有两种，分别使用 array 和 linked list 实现，但是push、pop 和 top element（栈顶元素的各种操作，查询、修改等） 这三个常用的操作其实都是 O(1) 时间复杂度；

### 顺序栈
> 数组实现的栈

基于数组的栈，要满足一下操作：
1. 申请一个数组，size 记录数组占用的大小，count 记录数组中的 element；
2. push 和 pop 操作，对数组来说其实就是对最后一个元素进行操作；
3. 操作 top element，对数组来说其实就是索引最后一个 element；
4. 判断数组为空和数组 element 超过数组大小的问题（这个涉及到动态数组了）；


### 链式栈
> 链表实现的栈

链式栈，基本上和数组的形式差不多，只是不存在内存不够的情况，优点是：**理论上栈不存在内存不足的情况**，缺点是：链表是需要存储指针的，所以内存消耗是要比数组多的。





## 动态扩容的栈
这个一般不会使用到，只是做个参考，用来比较均摊时间复杂度这个概念，其实动态扩容的 stack 复杂度还是 O(1) ，这个设计到均摊的概念；

---

## stack usage

### 函数调用栈
函数调用，其实也是使用了 stack 的结构，调用完成后，从 stack top 删除掉！

我们不一定非要用栈来保存临时变量，只不过如果这个函数调用符合后进先出的特性，用栈这种数据结构来实现，是最顺理成章的选择。

从调用函数进入被调用函数，对于数据来说，变化的是什么呢？是作用域。所以根本上，只要能保证每进入一个新的函数，都是一个新的作用域就可以。而要实现这个，用栈就非常方便。在进入被调用函数的时候，分配一段栈空间给这个函数的变量，在函数结束的时候，将栈顶复位，正好回到调用函数的作用域内。


---

### 表达式计算调用栈
在进行表达式计算的时候，也会用到栈，编译器会使用到 stack 进行运算，因为运算涉及到优先级别，编译器如何判断优先级，就利用了 stack 原理；

比如有一个表达式：1+2*3+4-6/2

编译器是将数组和操作符分别放入两个 stack 中，一个负责数字，一个负责运算符；

![operator](/Images/stack_openator.png)


同过对比 operator 优先级，对 number stack 进行操作，达到了计算的过程；


### 括号匹配

开发的过程中涉及到括号匹配，比如 string = “{[()]}” ，这种形式，经常会使用 stack 进行判断，将 左括号依次从左到右 push 到 stack 中，遇到右括号，则与 stack top 进行比较，相同则弹出，如果不相同则括号是不匹配的，或者判断最后 stack 中最后剩余的 element 是不是 0，就能判断括号是不是依次匹配的； 	


### 浏览器前进后退方法
分析：
浏览器的前进后退功能，这个操作可以抽象成 push/pop 操作，这样就不需要引进更加复杂的数据结构，其实队列、数组、链表、哈希表都能实现这个操作，这里只是简单的说 stack 的实现，因为更加的直观；

![brower forward and back](/Images/stack_brower_forward_back.png)

pop page 时候还会有可能再取回来，所以这里用两个 stack 实现，page6 是正在打开这个 webpage，会 push 到 first stack 中，这个时候我们看到的当前页，也就是 page6 就是 stack head，当 back page5 时候，page6 会 pop 出去，同时 pop 的 element 会 push 到 second stack 中，这样子就不会存在数据丢失的问题，在 page5 时候，再 forward 那么 second stack 会 pop page6 push 到 first stack 中；

当 first stack 没有 element 时候，second stack 中有 element，就代表无法后退，也就是起始页面，但是可以 forward，当两个 stack 都没有 element 时候，就代表是起始页，当 first stack 中有 element ，second中没有 element，代表在一直 forward，没有 back；

---

## stack in python

python3 中没有 stack 专门的结构，但是使用 list 构造一个 stack 非常容易，因为是底层是数组，push、pop、index 一个 tail element 非常容易，对于长度也可以走 len() 捷径，总体而言，效率还是比较高的！

```python
class Stack(object):

    def __init__(self):
        self.data = []

    def __str__(self):
        output_str = 'stack['
        for item in self.data:
            output_str += str(item) + ','
        output_str += ']'
        return output_str

    def is_empty(self):
        return not self.data

    def push(self, element):
        return self.data.append(element)

    def pop(self):
        return self.data.pop()

    def top(self):
        if self.data:
            return self.data[-1]
        else:
            return None

    def size(self):
        return len(self.data)


if __name__ == '__main__':
    stack = Stack()
    stack.push(100) 
    print(stack)
    for i in range(1, 11, 2):
        stack.push(i)
    stack.push('this is stack')
    print(stack)
    print(stack.size())
    print(stack.top())
    print(stack.pop())
    print(stack)

```
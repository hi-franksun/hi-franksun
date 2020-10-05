\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[blog.csdn.net\](https://blog.csdn.net/sodawaterer/article/details/71101811)

**前言：**
=======

flask 的轻便和强大的扩展性能会让 web 的初级开发者甚至是有经验的开发者神往。

flask 能在短时间内快速搭建 web 的后台，而《flask web 开发 -- 基于 python 的 web 应用开发实战》是最好的 flask 入门教程了。

但当中对应用上下文和请求上下文的讲解有点简单，本文对这两个重要的概念做一个总结，方便自己以后的回顾。

由于不看源码是很难理解上下文的前世今生，所有本文在某些地方会涉及到 flask 的源码，由于 flask 和 werkzug 结合得很紧密，werkzeug 也会有所展示。

**预备知识：**
=========

**1、Thread Local(本地线程)**
------------------------

从面向对象设计的角度看，对象是保存 “状态” 的地方。Python 也是如此，一个对象的状态都被保存在对象携带的一个特殊字典中。Thread Local 则是一种特殊的对象，它的 “状态” 对线程隔离 —— 也就是说每个线程对一个 Thread Local 对象的修改都不会影响其他线程。  

```
(local.py)
import threading
mydata = threading.local()
mydata.number = 42
print mydata.number
log = \[\]
 
def f():
    mydata.number = 11
    log.append(mydata.number)
 
thread = threading.Thread(target = f)
thread.start()
thread.join()
print log
print mydata.number
 
>python local.py
42
\[11\]   #在线程内变成了mydata.number其他的值
42     #但是没有影响到开始设置的值
```

这种对象的实现原理也非常简单，只要以线程的 ID 来保存多份状态字典即可，就像按照门牌号隔开的一格一格的信箱。这样来说，只要能构造出 Thread Local 对象，就能够让同一个对象在多个线程下做到状态隔离。这个 “线程” 不一定要是系统线程，也可以是用户代码中的其他调度单元，例如 Greenlet。  

**2、Werkzeug 的 Local**
----------------------

flask 和 werkzeug 结合紧密，werkzeug 是 flask 的 wsgi 工具集，所以 flask 中使用的本地线程是 werkzeug 中实现的 (werkzeug.local.Local)。

werkzeug.local.Local 和 threading.local 区别如下：

（1）werkzeug 使用了自定义的\_\_storage\_\_保存不同线程下的状态

（2）werkzeug 提供了释放本地线程的 release\_local 方法

（3）werkzeug 通过 get\_ident 函数来获得线程标识符

为什么 werkzeug 还自己搞了一套而不直接使用 threading.local 呢？  
在 python 中，除了线程之外，还有个叫协程的东东，(这里不提进程)。java 中貌似是无法实现协程的。而 python 的协程感觉高大尚的样子，python3.5 开始对协程内置支持，而且也有相关开源库 greenlet 等。  
协程是什么？  
举个例子，比如一个线程在处理 IO 时，该线程是处于空闲状态的，等待 IO 返回。但是此时如果不让我们的线程干等着 cpu 时间片耗光，有没有其他办法，解决思路就是采用协程处理任务，一个线程中可以运行多个协程，当当前协程去处理 IO 时，线程可以马上调度其他协程继续运行，而不是干等着不干活。  
这么一说，我们知道了协程会复用线程，WSGI 不保证每个请求必须由一个线程来处理，如果 WSGI 服务器不是每个线程派发一个请求，而是每个协程派发一个请求，所以如果使用 thread local 变量可能会造成请求间数据相互干扰，因为一个线程中存在多个请求。  
所以 werkzeug 给出了自己的解决方案：werkzeug.local 模块。  

除 Local 外，Werkzeug 还实现了两种数据结构：LocalStack 和 LocalProxy。  

**LocalStack** 是用 Local 实现的栈结构，可以将对象推入、弹出，也可以快速拿到栈顶对象。当然，所有的修改都只在本线程可见。和 Local 一样，LocalStack 也同样实现了支持 release\_pool 的接口。  
  
  
**LocalProxy** 则是一个典型的代理模式实现，它在构造时接受一个 callable 的参数（比如一个函数），这个参数被调用后的返回值本身应该是一个 Thread Local 对象。对一个 LocalProxy 对象的所有操作，包括属性访问、方法调用（当然方法调用就是属性访问）甚至是二元操作 \[6\] 都会转发到那个 callable 参数返回的 Thread Local 对象上，因此也不会影响到其他线程。  

**3、flask 上下文种类**
-----------------

<table><thead><tr><th>application context</th><th colspan="2">request context</th></tr></thead><tbody><tr><td>current_app（当前激活程序实例）</td><td colspan="2">request（请求对象，封装了 http 请求内容）</td></tr><tr><td>g（处理请求时临时存储的对象）</td><td colspan="2">session（用户会话，用于存储请求之间需要‘记住’的字典值）</td></tr></tbody></table>这四种上下文的定义都在 flask.globals.py 中

```
def \_lookup\_req\_object(name):
    top = \_request\_ctx\_stack.top
    if top is None:
        raise RuntimeError(\_request\_ctx\_err\_msg)
    return getattr(top, name)
 
 
def \_lookup\_app\_object(name):
    top = \_app\_ctx\_stack.top
    if top is None:
        raise RuntimeError(\_app\_ctx\_err\_msg)
    return getattr(top, name)
 
 
def \_find\_app():
    top = \_app\_ctx\_stack.top
    if top is None:
        raise RuntimeError(\_app\_ctx\_err\_msg)
    return top.app
 
 
# context locals
\_request\_ctx\_stack = LocalStack()
\_app\_ctx\_stack = LocalStack()
current\_app = LocalProxy(\_find\_app)
request = LocalProxy(partial(\_lookup\_req\_object, 'request'))
session = LocalProxy(partial(\_lookup\_req\_object, 'session'))
g = LocalProxy(partial(\_lookup\_app\_object, 'g'))
```

**4、为什么要用上下文**
--------------

flask 从客户端获取到请求时，要让视图函数能访问一些对象，这样才能处理请求。例如请求对象就是一个很好的例子。要让视图函数访问请求对象，一个显而易见的方法就是将其作为参数传入视图函数，不过这回导致程序中的每个视图函数都增加一个参数，为了避免大量可有可无才参数把视图函数弄得一团糟，flask 使用上下文临时把某些对象变为全局可访问（只是当前线程的全局可访问）。  

**正文：**
=======

**1、请求上下文（request、session）**
----------------------------

### **（1）生命周期**

先回顾一个请求上下文的使用例子

```
@app.route('/')
def index():
     user\_agent = request.headers.get('User-Agent')
     return'<p>Your broswer is %s<\\p>' % user\_agent
```

在视图函数中，可以直接使用 request 来获取请求对象。这里牵涉到请求上下文的生命周期问题了。

```
def handle\_request():
    print 'handle request'
    print request.url
 
handle\_request()   #会收到运行时错误：RuntimeError: working outside of request context
```

request 对象只有在请求上下文的生命周期内才可以访问。离开了请求的生命周期，其上下文环境也就不存在了，自然也无法获取 request 对象。  

那什么是请求上下文的生命周期呢？一般就是在视图函数里，或者请求钩子中，才能使用请求上下文 request。flask 在接收到来自客户端的请求时，会帮我们构造请求上下文的使用环境，当 flask 根据 url 跳到相应的视图函数的时候，自然而然就可以直接使用请求上下文，请求钩子也是同理。

此外，在不同 http 请求之间得到的 request 必然也是不一样的，它只包含当前 http 请求的一些信息，是线程隔离的。

而 session 不一样，session 是可以跨请求的，在不同的 http 请求之间，session 都是同一个，因此，可以借 session 来存储一些请求之间需要‘记住’的字典值。

### **（2）请求上下文环境构造（本篇幅有点长，涉及 flask 在接收 http 请求后的工作过程）**

```
from flask import Flask  
  
app = Flask(\_\_name\_\_)         #生成app实例  
 
@app.route('/')  
def index():  
        return 'Hello World'  
```

flask 是遵循 WSGI 接口的 web 框架，因此它会实现一个类似如下形式的函数以供服务器调用：

```
def application(environ, start\_response):               #一个符合wsgi协议的应用程序写法应该接受2个参数  
    start\_response('200 OK', \[('Content-Type', 'text/html')\])  #environ为http的相关信息，如请求头等 start\_response则是响应信息  
    return \[b'<h1>Hello, web!</h1>'\]        #return出来是响应内容  
```

这个 application 在 flask 里叫做 wsgi\_app。服务器框架在接收到 http 请求的时候，去调用 app 时，他实际上是用了 Flask 的 \_\_call\_\_方法，这点非常重要！！！  
因为\_\_call\_\_方法怎么写，决定了你整个流程从哪里开始。

```
class Flask(\_PackageBoundObject):        #Flask类  
  
#中间省略一些代码  
  
    def \_\_call\_\_(self, environ, start\_response):    #Flask实例的\_\_call\_\_方法  
        """Shortcut for :attr:\`wsgi\_app\`."""  
        return self.wsgi\_app(environ, start\_response)  #注意他的return，他返回的时候，实际上是调用了wsgi\_app这个功能  
```

如此一来，我们便知道，当 http 请求从 server 发送过来的时候，他会启动\_\_call\_\_功能，最终实际是调用了 wsgi\_app 功能并传入 environ 和 start\_response。

接下来查看 flask 中符合 wsgi 接口的这么一个函数 wsgi\_app：

```
class Flask(\_PackageBoundObject):  
  
    #中间省略一些代码  
    def wsgi\_app(self, environ, start\_response): 
        ctx = self.request\_context(environ)  
        ctx.push()  
        error = None  
        try:  
            try:  
                response = self.full\_dispatch\_request()    #full\_dispatch\_request起到了预处理和错误处理以及分发请求的作用  
            except Exception as e:  
                error = e  
                response = self.make\_response(self.handle\_exception(e))  #如果有错误发生，则生成错误响应  
            return response(environ, start\_response)       #如果没有错误发生，则正常响应请求，返回响应内容  
        finally:  
            if self.should\_ignore\_error(error):  
                error = None  
            ctx.auto\_pop(error)  
```

首先注意到这两行

```
       ctx = self.request\_context(environ)  
        ctx.push()
```

进去 request\_context 方法看看：

```
class Flask(\_PackageBoundObject): 
def request\_context(self, environ):
    return RequestContext(self, environ)
```

返回了一个 RequestContext 实例，于是查看类 RequestContext 的定义：

```
class RequestContext(object):
 
    def \_\_init\_\_(self, app, environ, request=None):
        self.app = app              #app是Flask(\_\_name\_\_)实例
        if request is None:
            request = app.request\_class(environ) #request\_class实质上是flask.wrappers下的class Request。这里根据environ创建一个Request实例
        self.request = request
        self.url\_adapter = app.create\_url\_adapter(self.request)
        self.flashes = None
        self.session = None
 
        self.\_implicit\_app\_ctx\_stack = \[\]
        self.preserved = False
        self.\_preserved\_exc = None
        self.\_after\_request\_functions = \[\]
 
        self.match\_request()
```

当第一个 http 请求过来时，request 默认是空的，通过 app.request\_class(environ) 构造  

因此 ctx 获得一个刚初始化过的 RequestContext 实例，ctx 包含了当前请求的 request、session 等各种信息，非常重要。

然后使用 ctx.push() 方法，这个方法很重要，继续查看：

```
class RequestContext(object):
    def push(self):
        top = \_request\_ctx\_stack.top   #top其实是个RequestContext实例
        if top is not None and top.preserved:
            top.pop(top.\_preserved\_exc)
        app\_ctx = \_app\_ctx\_stack.top
        if app\_ctx is None or app\_ctx.app != self.app:
            app\_ctx = self.app.app\_context()
            app\_ctx.push()
            self.\_implicit\_app\_ctx\_stack.append(app\_ctx)
        else:
            self.\_implicit\_app\_ctx\_stack.append(None)
 
        if hasattr(sys, 'exc\_clear'):
            sys.exc\_clear()
 
        \_request\_ctx\_stack.push(self)  #将自己入栈
        self.session = self.app.open\_session(self.request)   #根据当前客户端的cookie和有效时间来获取保存好的会话
        if self.session is None:                          #第一次http请求来时，得到的会话一般是空的
            self.session = self.app.make\_null\_session()   #创建一个新的会话
```

先从\_request\_ctx\_stack 获取栈顶元素。还记得 flask.globals 里的代码吗？

```
\_request\_ctx\_stack = LocalStack()
\_app\_ctx\_stack = LocalStack()
```

请求上下文栈和应用上下文栈都是 LocalStack 数据结构的实例

```
class LocalStack(object):
 
    def \_\_init\_\_(self):
        self.\_local = Local() #由此可以看出LocalStack实例包裹了一个Local类的实例
    def push(self, obj):
        rv = getattr(self.\_local, 'stack', None)
        if rv is None:
            self.\_local.stack = rv = \[\]
        rv.append(obj)
        return rv
 
    def pop(self):
        stack = getattr(self.\_local, 'stack', None)
        if stack is None:
            return None
        elif len(stack) == 1:
            release\_local(self.\_local)
            return stack\[-1\]
        else:
            return stack.pop()
 
    @property
    def top(self):
        try:
            return self.\_local.stack\[-1\]
        except (AttributeError, IndexError):
            return None
#LocalStack的push、pop、top的操作对象都是自身的\_local对象。
```

因此，\_request\_ctx\_stack 和\_app\_ctx\_stack 的 push、pop、top 操作都是针对自己 self.\_local.stack 属性进行的。  
而 self.\_local 是一个很重要属性，它是 WerkZeug.local.Local 的实例，是一个本地线程，因此不同线程之间的\_request\_ctx\_stack 和\_app\_ctx\_stack 是不一样的。  
现在回到 class RequestContext 的 push 方法，看以下几行代码：  

```
class RequestContext(object):
    def push(self):
        #省略了一些代码
        \_request\_ctx\_stack.push(self)  #将自己入栈
        self.session = self.app.open\_session(self.request)   #根据当前客户端的cookie和有效时间来获取保存好的会话
        if self.session is None:                          #第一次http请求来时，得到的会话一般是空的
            self.session = self.app.make\_null\_session()   #创建一个新的会话
```

执行 ctx.push 之后会将 ctx 自己压入\_request\_ctx\_stack 栈中，那么创建的 RequestContext 实例就被保存到了\_request\_ctx\_stack 栈里面了。这里关注另一个问题，请求上下文中 session。session 最先是根据 self.request 来打开，这里会根据 request 的信息，比如当前 http 请求的 cookie，来打开原来就保存好的，跟当前客户端对应的那个会话（不知道这样表述读者能不能理解）。因为要保证不同客户端过来时，打开的会话是不一样的。而同一个客户端的不同请求之间，打开的会话要是一样的。如果打开为空，则表面这个客户端是第一次链接，给它创建一个新的空的会话。  
这时候，ctx 已经被压栈，同时也已经携带好了 request 和 session 两个请求上下文了。但是还差一步，请求上下文的环境才算构造完成。  
这时候需要回到 globals.py 里回看 request 和 session 的获得过程：

```
def \_lookup\_req\_object(name):
    top = \_request\_ctx\_stack.top
    if top is None:
        raise RuntimeError(\_request\_ctx\_err\_msg)
    return getattr(top, name)
 
 
# context locals
\_request\_ctx\_stack = LocalStack()
\_app\_ctx\_stack = LocalStack()
 
request = LocalProxy(partial(\_lookup\_req\_object, 'request'))
session = LocalProxy(partial(\_lookup\_req\_object, 'session'))
```

request 和 session 都是先从\_request\_ctx\_stack 里获取头元素，这个元素是 RequestContext 实例，它包含了 request、session 等等的很多信息。request 和 session 就是从它身上获取的。这里可以看到，request 和 session 其实是一个 LocalProxy 实例，LocalProxy 其实就是一个代理，一个为 werkzeug 的 Local 对象服务的代理。他把所以作用到自己的操作全部 “转发” 到 它所代理的对象上去。它初始化的方法如下：

```
@implements\_bool
class LocalProxy(object):
 
    \_\_slots\_\_ = ('\_\_local', '\_\_dict\_\_', '\_\_name\_\_', '\_\_wrapped\_\_')
 
    def \_\_init\_\_(self, local, name=None):        
	# 这里有一个点需要注意一下，通过了\_\_setattr\_\_方法，self的
        # "\_LocalProxy\_\_local" 属性被设置成了local，你可能会好奇
        # 这个属性名称为什么这么奇怪，其实这是因为Python不支持真正的
        # Private member，具体可以参见官方文档：
        # http://docs.python.org/2/tutorial/classes.html#private-variables-and-class-local-references
        # 在这里你只要把它当做 self.\_\_local = local 就可以了 :)
        object.\_\_setattr\_\_(self, '\_LocalProxy\_\_local', local)
        object.\_\_setattr\_\_(self, '\_\_name\_\_', name)
        if callable(local) and not hasattr(local, '\_\_release\_local\_\_'):
            # "local" is a callable that is not an instance of Local or
            # LocalManager: mark it as a wrapped function.
            object.\_\_setattr\_\_(self, '\_\_wrapped\_\_', local)
```

LocalProxy 初始化的时候接收一个可以调用的方法，并且设置为自身的\_LocalProxy\_\_local 属性。因此 request 和 session 本质是一个 LocalProxy 实例，它们都有一个\_LocalProxy\_\_local 属性，分别指向偏函数\_lookup\_req\_object(request) 和\_lookup\_req\_object(session)。这时候请求上下文的环境就构造完成了。现在相信读者也应该理解了为什么这两个上下文只能在视图函数或者请求钩子里面使用了，因为只有这时候才具备了使用的环境。后面会介绍如何自行构造环境。  

### **（3）请求上下文的使用**

既然 request 和 session 本质是一个 LocalProxy 实例，那它是如何使用呢？这里看几个 LocalProxy 类下的重要的方法：  

```
@implements\_bool
class LocalProxy(object):
 
    def \_get\_current\_object(self):
        """
        获取当前被代理的真正对象，一般情况下不会主动调用这个方法，除非你因为
        某些性能原因需要获取做这个被代理的真正对象，或者你需要把它用来另外的
        地方。
        """
        # 这里主要是判断代理的对象是不是一个werkzeug的Local对象，在我们分析request
        # 的过程中，不会用到这块逻辑。
        if not hasattr(self.\_\_local, '\_\_release\_local\_\_'):
            # 从LocalProxy(partial(\_lookup\_req\_object, 'request'))看来
            # 通过调用self.\_\_local()方法，我们得到了 partial(\_lookup\_req\_object, 'request')
            # 也就是 \`\`\_request\_ctx\_stack.top.request\`\`
            return self.\_\_local()
        try:
            return getattr(self.\_\_local, self.\_\_name\_\_)
        except AttributeError:
            raise RuntimeError('no object bound to %s' % self.\_\_name\_\_)
    # 接下来就是一大段一段的Python的魔法方法了，Local Proxy重载了(几乎)?所有Python
    # 内建魔法方法，让所有的关于他自己的operations都指向到了\_get\_current\_object()
    # 所返回的对象，也就是真正的被代理对象。
 
    def \_\_getattr\_\_(self, name):
        if name == '\_\_members\_\_':
            return dir(self.\_get\_current\_object())
        return getattr(self.\_get\_current\_object(), name)
 
    def \_\_setitem\_\_(self, key, value):
        self.\_get\_current\_object()\[key\] = value
```

request 和 session 一般的使用如：request.form.get('args\_name')、request.args.get('args\_name') 或者 session\['name'\]=value。  
这里分析一个简单的例子：page = request.args.get('page', 1, type=int)  

这里会先调用 request 的\_\_getattr\_\_方法，返回 getattr(self.\_get\_current\_object(),'args') 获取真正的代理对象，即\_request\_ctx\_stack 栈顶的 RequestContext 元素，再在它身上得到 request，再获取 args。

```
class Flask(\_PackageBoundObject):  
    #中间省略一些代码  
    def wsgi\_app(self, environ, start\_response): 
        ctx = self.request\_context(environ)  
        ctx.push()  
        error = None  
        try:  
            try:  
                response = self.full\_dispatch\_request()    #full\_dispatch\_request起到了预处理和错误处理以及分发请求的作用  
            except Exception as e:  
                error = e  
                response = self.make\_response(self.handle\_exception(e))  #如果有错误发生，则生成错误响应  
            return response(environ, start\_response)       #如果没有错误发生，则正常响应请求，返回响应内容  
        finally:  
            if self.should\_ignore\_error(error):  
                error = None  
            ctx.auto\_pop(error)  
```

在视图函数处理完，wsgi 生成完 response 之后，\_request\_ctx\_stack 会将栈顶这个 RequestContext 实例出栈。虽然 ctx 已经出栈，但是 ctx.session 已经通过序列化保存在本地了，但是 request 已经被抛弃。这就是为什么 session 可以跨请求使用了。

### （4）自行构造请求上下文环境

可以仿照源码的形式，自行构造上下文：

```
from werkzeug.test import EnvironBuilder
 
ctx = app.request\_context(EnvironBuilder('/','http://localhost/').get\_environ())
ctx.push()
try:
    print request.url
finally:
    ctx.pop()
```

请求上下文基本就介绍完毕了。当然还有应用上下文，将在下篇介绍。 这里贴上几篇参考的博文，有兴趣的童鞋都可以看看： [Flask 进阶系列 (一)–上下文环境](http://www.bjhee.com/flask-ad1.html) [Flask 的 Context 机制](https://blog.tonyseek.com/post/the-context-mechanism-of-flask/) [flask 上下文的实现](https://segmentfault.com/a/1190000004223296) [Werkzeug Local 与 LocalProxy 等浅析](http://www.thinksaas.cn/topics/0/681/681414.html)  
[Python 中的魔法方法深入理解](http://www.jb51.net/article/52021.htm)  
[Flask 源码解读 <1> --- 浅谈 Flask 基本工作流程](http://blog.csdn.net/bestallen/article/details/54342120)  
[Flask 源码解读 <2> --- 请求上下文和 request 对象](http://blog.csdn.net/bestallen/article/details/54429629)
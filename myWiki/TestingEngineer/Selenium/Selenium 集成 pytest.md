# pytest

.gitignore 文件填写了，在 Pycharm 中会将忽视的文件变成一个黄色，这是个小技巧，哈哈哈哈

## pytest overview

pros and cons

pros

- 简单灵活，易于上手，文档丰富，信息完善；
- 支持参数化，可以细粒度的控制测试用例执行；
- 支持简单的单元测试，也支持负责的单元测试；
- 用例 skip、xfail 处理，装饰器方式；
- 第三方插件丰富，比如测试报告、case 重复执行、集成 Selenium
- 和 CI 工具结合便利，比如 Jenkins

python integrated tools：python 集成工具
一些包管理、虚拟环境、测试框架、格式化文档等的工具，可以灵活设置

## pytest 官网

[pytest official website](https://docs.pytest.org/en/stable/)
[pytest Official document](https://docs.pytest.org/en/stable/contents.html)
[pytest Github address](https://github.com/pytest-dev/pytest/)
[pytest 3rd party plugins](http://plugincompat.herokuapp.com/)

## pytest编写规则

- 测试文件以 test 开头/结尾
- 测试类以 Test 开头，且不能带有 `__init__` 方法
- 测试函数以 test 开头
- 断言使用基本的 assert 即可

## Console 参数

-s,-v 这两个参数是最常用

- -v 显示每个测试函数的执行结果
- -s 显示测试函数中 print() 函数的输出
- -q 只显示整体测试结果
- -x, --exitfirst，在第一个错误或者失败时立即退出
- -h 帮助

## 实例

使用命令行执行：pytest -sv test000.py
mian 方法执行：pytest.main(['-s', '-v', 'test000.py'])
Pycharm 执行，设置相应测试框架，在 IDE 里执行更加方便

## pytest 标记

pytest 查找策略

- pytest 会递归查找当前目录下以 test 开头和结尾的 python 脚本；
- 找到相应的符合规则脚本后，会找到相应以 test 开头的测试类和测试函数；
- 可以编写不作为测试用例的函数和类，主要用来被测试类调用，这个查找规则很好；

pytest 标记测试函数

- 显示指定函数名 pytest test.py::test000 这个test000 就不会执行
- 模糊匹配 pytest -k test_func test.py 模糊匹配 test_func 名字的函数就不会执行；

- pytest.mark 在函数上标记，使用配置文件

    ```ini
    [pytest]
    markers = test000, test008
        do:do
        undo:undo
    ```

    使用命令行执行；`pytest -m do test000.py`
    会看到 deselected 这信息，这个信息就是被标记不执行的用例；

## pytest 参数化处理

pytest 支持参数化测试，即每组参数都独立执行一次测试
pytest.mark.parametrize(argnames, argvulues)，注意这个 argnames 一定是个字符串形式传递的，如果存在多个参数要以整体 string 但内部以 , 进行分割，一般序列中用的多，放到一起可以是一个整体参数，如果分开，按照索引的数量进行分割就可以了 ['123'] ，这个可以当成一个整体，传入 'number'，但是也可以分开成三个参数，'a, b, c'！

list:

```python
data = ['123', '456']
@pytest.mark.parametrize(argnames='username', argvalues=data)
def test000(self, username):
    print(username)
```

tuple:

```python
data1 = ('123', '456')
@pytest.mark.parametrize(argnames='username', argvalues=data1)
def test000(self, username):
    print(username)

@pytest.mark.parametrize('a, b, c', data1)
def test003(self, a, b, c):
    print(a)
    print(b, c)
    print('this is third pytest testcase')
```

```python
dict:
data = [
    {
    '123': 123,
    '456': '456'
    }
    ]
@pytest.mark.parametrize(argnames='username', argvalues=data)
def test000(self, username):
    print(username)
# dict 类型直接传入就可以了
```

传入 argsvalues 也可以使用新的方式，可以增加 id，也就是增加了注释的意思，增加测试报告的可读性

```python
data = [
    pytest.param(1, 2, 3, id='a+b:pass'),
    pytest.param(4, 5, 6, id='a+b:fail'),
]

@pytest.mark.parametrize(argnames='a, b, c', argvalues=data)
def test000(self, a, b, c):
    result = add(a, b)
    assert a == c
```

## pytest fixture

- 定义 fixture 和普通函数一样，只不过要加上装饰器 @pytest.fixture()；
- fixture 装饰的函数不能以 test 开头，要和测试用例分开，fixture 是有返回值的；
- 用例调用 fixture 返回值，直接把 fixture 的函数名称当成 variable 使用

## pytest setUp and tearDown

模块级别(setup_module/teardown_module) 开始于模块始末，全局
函数级别(setup_function/tearndown_function) 只对函数生效
类级别(setup_class/teardown_class)只对类中前后运行一次（在类中）
方法级别(setup_method/teardown_method) 开始于方法始末（在类中）
类里面的(setip/teardown)运行在调用方法前；

明显的优势是粒度更细，更加灵活，增加了模块的复用，特别在 selenium 中，启动 brower一次可以执行多个用例，数据库连接，文件读取，用例依赖；

## allure 测试报告

1. 下载库：pip install allure-pytest

2. 官方网站和文档地址
[官方网站](http://allure.qatools.ru/)
[官方文档](https://docs.qameta.io/allure/#_report_generation)
[Github address 可以去下载 release](https://github.com/allure-framework/allure2)

3. allure 可执行文件下载地址
下载完 release 进行环境变量配置

## 安装 pytest-dependency 解决依赖问题

```python
# 被依赖
@pytest.mark.dependency(name='login')
# 依赖上边这个起的名字的用例
@pytest.mark.dependency(depends=['login'], scope='moduel') # 模块级别
```

有依赖关系又有ddt 要先加依赖关系的装饰器

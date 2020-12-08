
## 安装 pytest


## pytest 运行规则

- 测试文件以 `test_*` 开头或者 `*_test` 结尾也可以）
- 测试类以 `Test` 开头，并且不能带有 `__init__` 方法
- 测试函数/方法以 `test_` 开头
- 所有的包pakege必须要有__init__.py文件


## pytest 最简单的执行方式

```python
pytest # 第一种：直接在目录下执行，搜索符合条件的执行
python -m pytest # 第二种：也是直接执行

```

## pytest 执行测试文件

1. pytest dir_name/ 指定文件夹下的测试所有文件
2. pytest test_one.py 指定单个文件
3. -k 按关键字匹配
4. 按节点运行
5. 标记表达式
6. 从包里面运行


## 遇到错误后执行操作
### -x 遇到错误后停止执行

```python
pytest -x # 单个文件和所有文件都适用

=============================== short test summary info ===========================================
FAILED test_demo1.py::TestClass::test_two - AssertionError: assert 'hello' == 'check'
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! stopping after 1 failures !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
============================== 1 failed, 1 passed in 0.06s =========================================
```

### --maxfail=1 最大错误数停止运行

```python
pytest --maxfail=1 # 当错误书到达 1 时候，停止运行测试用例
```



## 测试用例的执行 setup 和 teardown

### 函数式
在执行函数的测试用例的时候进行使用，分为两种：

- setup_module/teardown_module：整个 *.py 文件只会执行一次，整个文件的开始和结束执行一次，比如打开和关闭浏览器；
- setup_function/teardown_function：整个 *.py 文件中，每个函数执行开始和结束都会执行一次，比如每次都会返回主界面；

执行顺序：setup_module --> setup_function --> test_one --> teardown_function --> teardown_module

```py
import pytest

def setup_module():
    print('this is setup_module')

def teardown_module():
    print('this is teardown_module')

def setup_function():
    print('this is setup_function')

def teardown_function():
    print('this is teardown_function')

def test_one():
    print('test_one')
    assert 'd' in 'de'

def test_two():
    print('test_two')
    assert 'h' in 'hello'

if __name__ == '__main__':
    pytest.main(['-s', 'test_demo2']) # -q 只显示结果，-s 会显示测试用例的打印过程

'''
collecting ... collected 2 items

test_demo2.py::test_one this is setup_module
this is setup_function
PASSED                                           [ 50%]
test_one
this is teardown_function

test_demo2.py::test_two this is setup_function
PASSED                                           [100%]
test_two
this is teardown_function
this is teardown_module

============================== 2 passed in 0.02s ==============================
'''
```


### 类中执行顺序

- 类级（setup_class/teardown_class）只在类中前后运行一次(在类中)
- 方法级（setup_method/teardown_method）开始于方法始末（在类中）
- 类里面的（setup/teardown）运行在调用方法的前后

如果三个都存在，一般不会同时使用 setup 和 setup_method，但是同时使用的时候，会先执行 setup_method 再执行 setup；

setup_class --> setup_method --> setup --> testcase --> teardown --> teardown_method --> teardown_class

```python
import pytest

class TestCaseOne(object):

    def setup_class(self):
        print('this is setup_class')

    def teardown_class(self):
        print('this is teardown_class')

    def setup_method(self):
        print('this is setup_method')

    def teardown_method(self):
        print('this is teardown_method')

    def setup(self):
        print('this is setup')

    def teardown(self):
        print('this is teardown')

    def test_one(self):
        print('test_one')
        assert 'd' in 'de'

    def test_two(self):
        print('test_two')
        assert 'h' in 'hello'


if __name__ == '__main__':
    pytest.main(['-s', 'test_demo2'])


'''
collecting ... collected 2 items

test_demo2.py::TestCaseOne::test_one 
this is setup_class
this is setup_method
this is setup
PASSED                              [ 50%]
test_one
this is teardown
this is teardown_method

test_demo2.py::TestCaseOne::test_two this is setup_method
this is setup
PASSED                              [100%]
test_two
this is teardown
this is teardown_method
this is teardown_class

============================== 2 passed in 0.03s ==============================
'''
```

### 当类中和函数中都会使用的时候

函数的方法和类中的方式不会影响，唯一的是 setup_module/teardown_model 这个模块的方法，会被提升到最高的优先级，也就是执行开始和结束的时候分别执行；


## fixture之conftest.py

## 生成测试报告
pytest-html



[[pytest]]
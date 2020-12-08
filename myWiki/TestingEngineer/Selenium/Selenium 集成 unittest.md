# unittest

## 简介

unittest 是 Python 的单元测试框架，类似与 Java 的 Junit，其实也是从 Java 借鉴来的，主要用来做单元测试；

unittest 5 个重要概念：

- test fixure
  - setup
  - teardown
- test case
- test suite
- test runner
- test loader

### Testcase

- 一个 Testcase 的实例就是一个测试用例，一个测试用例就是一个完整的测试流程，包括：
  - 测试环境准备：setUp
  - 执行测试代码：run
  - 测试环境还原：tearDown
- 单元测试（unit test）：一个完整的测试单元就是一个测试用例，通过运行这个测试单元，对程序进行测试，验证程序的正确性；

### Test suite

多个测试用例集合在一起，就是 TestSuite，TestSuite 也可以 嵌套 TestSuite

### Test runner

执行测试用例，其中的 run(test) 会执行 TestSuite/TestCase 中 run(result) 方法。

### TestLoader

用来加载 TestCase 到 TestSuite 中，其实有一些方法 loadTestForm_()，就是从各个地方寻找 TestCase，创建他们的实例，然后 add 到 Testsuite，再返回一个 TestSuite 实例；

### Test fixture

测试脚手架，主要负责测试环境的初始化和测试环境的还原；

## 详细

teardown 和 setup 又分成类方法和实例方法，类    方法会在整个测试用例类中执行一次，而实例方法每次运行一个 Testcase(实例方法)，都会运行一次；一般前置方法用于数据库连接、读写文件等操作、初始化 webdriver 、关闭 webdriver ，就是和具体的 TestCase 业务无关的内容；

测试用例之间的用例加载顺序遵循 ASCII 码规则，也就是按照 0-9，A-Z，a-z 顺序加载，这个写在方法里，也可以自己进行修改

断言方法

- assertFalse(self, expr, msg=None)
- assertTrue(self, expr, msg=None)
- assertEqual(self, first, second, msg=None)
- assertNotEqual(self, first, second, msg=None)
- assertIn(self, member, container, msg=None)
- assertNotIn(self, member, container, msg=None)

assertSequenceEqual(self, seq1, seq2, msg=None, seq_type=None)
assertListEqual(self, list1, list2, msg=None)
assertTupleEqual(self, tuple1, tuple2, msg=None)
assertSetEqual(self, set1, set2, msg=None)

assertIs(self, expr1, expr2, msg=None)
assertIsNot(self, expr1, expr2, msg=None)
assertDictEqual(self, d1, d2, msg=None)
assertDictContainsSubset(self, subset, dictionary, msg=None)
assertCountEqual(self, first, second, msg=None)
assertMultiLineEqual(self, first, second, msg=None)
assertIsNone(self, obj, msg=None)
assertIsNotNone(self, obj, msg=None)
assertIsInstance(self, obj, cls, msg=None)
assertNotIsInstance(self, obj, cls, msg=None)

- assertRaises(self, expected_exception, *args, **kwargs)
- assertWarns(self, expected_warning, *args, **kwargs)
- assertRaisesRegex(self, expected_exception, expected_regex, *args, **kwargs)
- assertWarnsRegex(self, expected_warning, expected_regex, *args, **kwargs)
- ssertRegex(self, text, expected_regex, msg=None)
- assertNotRegex(self, text, unexpected_regex, msg=None)

## 测试套件

## 测试用例的加载

test_suite.addTest(*args)
这里会先判断传入的参数是不是 TestCase或者 TestSuite，然后再 append 到一个 list 中，再执行这个 list 中的所有用例；

1. 通过测试用例类进行加载
  
    ```python
    test_suite = unittest.TestSuite()
    testcase_loader = unittest.TestLoader

    test_suite.addTest(testcase_loader.loadTestsFromTestCase(MyTest00))
    test_suite.addTest(testcase_loader.loadTestsFromTestCase(MyTest01))

    test_suite = unittest.TestSuite()
    testcase_loader = unittest.TestLoader
    ```

2. 通过测试用例模板去加载

    ```python
    test_suite = unittest.TestSuite()
    testcase_loader = unittest.TestLoader

    test_suite.addTest(testcase_loader.loadTestsFromModule(MyTest00))
    test_suite.addTest(testcase_loader.loadTestsFromModule(MyTest01))

    runner = unittest.TextTestRunner()
    runner.run(test_suite)
    ```

3. 通过路径加载

    ```python
    test_suite = unittest.TestSuite()
    testcase_loader = unittest.TestLoader

    path = os.path.dirname(os.path.abspath('__main__'))
    test_suite.addTest(testcase_loader(path))

    runner = unittest.TextTestRunner()
    runner.run(test_suite)
    ```

4. 通过逐条加载

    ```python
    test_suite = unittest.TestSuite()
    testcase_loader = unittest.TestLoader

    case0 = MyTest00('test000') # 只是添加了这一条用例
    test_suite.addTest(testcase000)

    runner = unittest.TextTestRunner()
    runner.run(test_suite)
    ```

## unittest 测试报告

## 测试报告

一般不会使用 unittest 进行，不好集成；

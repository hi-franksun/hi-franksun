
## Get starting

```python
from selenium import webdriver
from time import sleep


def t1():
    driver = webdriver.Chrome()
    driver.get('https://www.baidu.com')
    driver.find_element_by_id('kw').send_keys('selenium')
    driver.find_element_by_id('su').click()
    sleep(5)
    driver.quit()


class TestCase(object):
    def __init__(self):
        self.driver = webdriver.Chrome()

    def t2(self):
        self.driver.get('https://www.baidu.com')
        self.driver.find_element_by_id('kw').send_keys('selenium')
        self.driver.find_element_by_id('su').click()
        sleep(5)
        self.driver.quit()


if __name__ == '__main__':
    # t1()
    t2 = TestCase()
    t2.t2()
```

## Selenium 运行原理

## 8 methods of find elements

### 元素选择策略

在 WebDriver 中有 8 种不同的内置元素定位策略：

| 定位器 Locator | 描述 |
| --- | --- |
| class name | 定位class属性与搜索值匹配的元素（不允许使用复合类名），很少用 |
| css selector | 定位 CSS 选择器匹配的元素，用的较多 |
| id | 定位 id 属性与搜索值匹配的元素 ，用的较多|
| xpath | 定位与 XPath 表达式匹配的元素 ，用的较多|
| name | 定位 name 属性与搜索值匹配的元素 ，用的较多|
| link text | 定位link text可视文本与搜索值完全匹配的锚元素 |
| partial link text | 定位link text可视文本部分与搜索值部分匹配的锚点元素。如果匹配多个元素，则只选择第一个元素。 |
| tag name | 定位标签名称与搜索值匹配的元素，基本上很少有，tag 太多会返回一个列表 |

取到的 element 返回的数据类型是：

```python
<class 'selenium.webdriver.remote.webelement.WebElement'>
```

## WebDrive 常用的属性和方法

## WebElement 常用的属性和方法

| # | 属性 | 属性描述 |
|--|--|--|
| 1 | id | 标示 |
| 2 | size | 宽高 |
| 3 | rect | 宽高和坐标 |
| 4 | tag_name | 标签名称 |
| 5 | text | 文本内容 |

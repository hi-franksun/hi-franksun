# Quick Start

## 快速开始

脚手架快速生成一个测试文件夹

`httprunner startproject demo`

> 常见的证书校验问题，会报 SSL 错误，设置 `verify: false` 即可，或者关闭系统的代理

使用 `Charles` 进行抓包，导出为 `har` 文件格式，使用 `har2case` 命令行进行转换，一般推荐转换成 `yaml` 格式，

打开开发者工具（F12），选择 Network——Disable cache 即可。需要清除某网站缓存时 F12 打开开发者工具就会自动清除这个网站的缓存，而不必清除所有网站的缓存了。


har2case 命令行说明文档
```python
usage: har2case har2case [-h] [-2y] [-2j] [--filter FILTER] [--exclude EXCLUDE] [har_source_file]

positional arguments:
  har_source_file       Specify HAR source file

optional arguments:
  -h, --help            show this help message and exit
  -2y, --to-yml, --to-yaml
                        Convert to YAML format, if not specified, convert to pytest format by default.
  -2j, --to-json        Convert to JSON format, if not specified, convert to pytest format by default.
  --filter FILTER       Specify filter keyword, only url include filter string will be converted.
  --exclude EXCLUDE     Specify exclude keyword, url that includes exclude string will be ignored, multiple
                        keywords can be joined with '|'

```

extract:
    documentId: content.data.id # 获取 id 的语法是jsonpath 类似的语法
    documentId: content.data.id[0] # 如果是数组的话，要索引获取即可

export:
    - documentId
    - documentId123

%{documentId} # 可以直接引用
%{documentId}data # 如果这个引用后还有其他数据，比如 data还存在其他数据，那么这种方式就看了一了 

html 文件中使用正則匹配的方式進行讀取，


校验项：

validate:
    - eq: ["status_code", 200]
    - eq: ["body.form.foo1", "bar1"]
    - eq: ["body.form.foo2", "bar21"]
    - len_eq: ["body.data", 30] 校验长度

hook 函数

配置文件中的，这个目前已经基本不能使用了，推荐在 py 文件中直接使用，使用 unittest 和 pytest 都可以的

```python
class TestCaseCase000Login(HttpRunner):

    @classmethod
    def setup_class(cls):
        print('==========setup sleep 10 ==========')
        time.sleep(10)

    @classmethod
    def teardown_class(cls):
        print('==========teardown sleep 10 ==========')
        time.sleep(10)

    config = Config("login").variables(**{"sleep": "${sleep(1)}"}).verify(False)

    teststeps = [
```

步骤级别的还是可以卸载 yaml 文件中的，但是也可以写在 py 文件中，而且也可以使用 debugtalk 的中定义的函数
```yml
config:
    name: login
    variables: {
                   sleep: '${sleep(1)}'
    }
    setup_hooks:
        - ${sleep(10)}
    teardown_hooks:
        - ${sleep(10)}
    verify: false
teststeps:
....
```
setup_hooks:
    - ${sleep(60)}
    - ${print_document_id('this is testhooks')}


可以设置一个 setup_hooks
```python
RunRequest("/api/login/submit")
.setup_hook("${sleep(60)}")
.setup_hook("${print_document_id('this is testhooks')}")
```

## 核心源码

## 进阶

## 实战

## 注意项

## 其他

### 代理工具 Charles

PC 端抓包

移动端抓包

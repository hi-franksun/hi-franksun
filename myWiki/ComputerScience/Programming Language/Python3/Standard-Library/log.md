# log module

## log 作用

1. 程序调试
2. 了解程序运行是否正常
3. 故障分析和问题定位
4. 用户行为分析

## 日志级别

- DEBUG：详细的日志信息，典型的应用常见是问题诊断；
- INFO：仅次于 DEBUG 级别，主要记录关键 节点信息，用于确认是否按照我们预期的工作；
- WARNING：警告信息，比如磁盘不足、密码安全过低、网络延迟过高、某个 ip 请求次数过多；
- ERROR：导致**某些功能不能正常运行**的严重问题；
- CRITICAL：严重错误，导致**应用程序不能继续运行**；

## 使用方式

1. 使用 logging 提供的模块级别的函数

    ```python
    CRITICAL = 50
    FATAL = CRITICAL
    ERROR = 40
    WARNING = 30
    WARN = WARNING
    INFO = 20
    DEBUG = 10
    NOTSET = 0
    ```

    ```python
    import logging

    logging.basicConfig(level=logging.DEBUG) # 配置 log 的级别
    logging.debug('debug')
    logging.info('info')
    logging.warning('warning')
    logging.error('error')
    logging.critical('critical')
    logging.log(20, 'this is info level')
    ```

2. 使用 logging 日志系统的四大组件

- loggers（日志器）：提供应用程序代码直接使用的接口
- handlers（处理器）：将日志记录发送到指定位置（控制台、日志文件）
- filters（过滤器）：粒度更细的日志过滤功能，决定那些日志记录会被记录，那些会被忽略；
- formatters（格式器）：用于控制日志信息的最终输出格式，比如是否带时间等；

logging.basicConfig() 函数的参数说明：
| 参数名称 | 描述 |
| -- | -- |
| filename | 指定日志输出文件的文件名，配置后控制台就不会输出了 |
| filemode | 日志文件的打开模式，默认是 'a', 也就是追加写入的模式 |
| format | 指定日志格式的字符串，指定每个字段的意义和顺序 |
| datefmt | 指定日期/时间格式，format 中包含时间字段 %(asctime) 才会生效 |
| level | 指定日志过滤级别 |
| stream | 指定日志输出目标stream，如 sys.stdout, sys.stderr和网络 stream |
| style | 指定 format 格式字符串的风格默认是 % |
| handlers | 3.3 新加的配置 |

### Logger

相关方法

- Logger.setLevel()
- Logger.addHandler()
- Logger.removeHandler()
- Logger.exceptin()
- Logger.log()

### Handler

作用是将消息分发到 handler 指定的位置（文件、网络、邮件等）

- Handler.setLevel():设置 handler 将会处理日志的最低严重级别；
- Handler.setFormatter():设置一个 handler 的格式器对象；
- Handler.addFilter()/Handler.removeFilter():为 handler 添加和删除一个过滤器对象；

### Handler 子类（很重要）


```python

```

一般日志用在断言的地方，对断言进行 try except 处理，报错了就记录错误日志；

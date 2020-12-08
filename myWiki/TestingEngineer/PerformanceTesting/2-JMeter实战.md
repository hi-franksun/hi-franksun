# 性能测试实战

## 压测环境

使用 Flask 构建一个 api 服务，进行测试

## JMeter 进阶操作

### test upload/download files

文件上传，注意选择文件上传的路径和方法，以及上传的格式，在进行文件上传的测试中，文件的大小会对服务器的性能有很大影响，一般会把系统的平均值拿出来，而不是无脑的上大文件

```html
 <form action="/uploadfile" method=post enctype=multipart/form-data>
 ```

JMeter MIME Tpye要填写：multipart/form-data

### test databases

常见操作数据库的操作

0. 准备、制造测试数据
导入 `mysql-connector-java-8.0.15.jar` 数据库的 jar 包，到 JMeter 的 lib 文件夹下
新建配置，创建一个 JDBC Connection Configuration 组件
新建请求，常见一个 JDBC Request 组件
INSERT data：

1. 获取、查询测试数据
SELECT data：需要拿到一些数据，被后边的请求使用

2. 清理测试环境、删除过程数据
DELETE data

3. 数据库压测

一般 select 会高于 insert，所以读写分离还是很有必要的

### 关于线程组

Thread Group
性能测试的资源调度池
控制性能测试的运行调度、参与人数、执行策略
分类：Setup、Teardown、Normal，不同的分类再整个压测执行的生命周期内的执行时间点不同

线程组配置：

再请求取样器执行错误时需要执行的下一步动作
Continue：继续执行接下来的操作；
Start Next Loop：忽略错误，执行下一个循环
Stop Thread：退出该线程（不再对此线程进行任何操作）
Stop Test：等待当前执行的采样器结束后，结束整个测试
Stop Test Now：直接停止整个测试

Thread Properties(线程属性)
Number of Thread(users)：线程数、模拟用户数量
Ramp-up Period(加速器)：达到指定线程需要的时间，举例：线程数设置为 50，加速器设置为 5，那么每秒启动的线程数就是 50/5 = 10 个，模拟更加现实的情况，真实的情况是并发的启动绝对不可能是同时的。
Delay Thread creation until needed ：当线程需要执行开始的时候才会被创建。如果不勾选，那么在测试计划开始的时候，九八所有需要的线程全部常见完成了；
Loop count：（infinite-无限次）循环次数


调度配置：
Duration(seconds)：持续时间(秒)，此选项填入 N，说明这个计划从开始时间计算，执行 N 秒结束
Startup delay(seconds)：启动延迟(秒)，此选项填入 N，手动点击开始执行计划，然后延迟 N 秒后，计划才会真正执行

Scheduler：时间计划，

setUp：一般在压测准备极端执行，准备测试数据，设置参数，准备环境
tearDown：测试结束后，清理数据，环境归零


### JavaRequest 实战技巧

### JMeter 监听器和压测监控

### 常用的断言设置

### 性能测试常见设计
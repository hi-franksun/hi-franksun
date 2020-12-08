# Flask

## Flask 核心对象

## blueprint(蓝图)

blueprint 的设计是为了增加拓展，将不同的应用分别注册到应用上(核心对象)

Blueprint 是一种组织一组相关视图及其他代码的方式。与把视图及其他 代码直接注册到应用的方式不同，蓝图方式是把它们注册到蓝图，然后在工厂函数中 把蓝图注册到应用。

`blueprint(name, import_name, static_folder=None, static_url_path=None, template_folder=None, url_prefix=None, subdomain=None, url_defaults=None, root_path=None, cli_group=<object object>)`

## Flask 上下文

### 应用上下文

### 请求上下文

## view model

原始数据：从 api 和 databases 取出来的数据不需要修改直接使用；

原始数据是不满足需求的，原始数据一般会经过处理再返回给需求方，通常对原始数据的处理有三种

- 裁剪一些不需要的数据；
- 修饰数据，添加一些附加信息，比如字符串拼接；
- 合并数据，一些数据不满足需求，会对原始数据进行一些计算才符合要求；

## 消息闪现

消息闪现需要配饰 secret_key，否则会报错。

## flask 开启多线程

多线程是 webserver（nginx/Apache/Tomcat） 开启的，而不是 flask 自己开启的，flask 自带的 webserver 是为了方便调试而自带的，所以默认是单线程和单进程；

flask 开启自带的多线程和多进程

```python
app.run(threaded=True, ) # threaded:多线程，processes：多进程
```

requests：只是一个变量名，真正的请求对象是 Requests，

## filter 函数的应用

## 软删除

数据库删除数据的时候，一般都会使用软删除，也就是设计一个开关，一般在数据库中的某个字段，一般使用 0/1 进行标记，

## 用户系统

1. 登录
2. 注册
3. 找回密码
4. 修改密码

## 动态赋值

## 可读属性

## orm 提交数据模型

## orm 查询

## 重定向

## cookie 失效

设置有效时间：不设置有效时间只会保存一次，不会长时间保存
手动可以取消 cookie，清楚浏览器 cookie

## flask login

## 权限控制

## ajax 技术

ajax 技术可以提升服务器的性能，将一些不必要的文件渲染和查询避免掉，重定向会丧失服务器的很多性能，因为相当再次渲染了页面

## 时间转换

## 再谈 MVC 种的 model

m：model 层，也就是数据库的定义
v：模板层
c：control 层，其实也就是 视图层，也有很多人写成了 control 层

具体的业务写在哪一层，这个需要自己判断，但是一定要注意，M 层是可以些业务逻辑的，而不是说业务逻辑只能写在 c 层里，一定要灵活处理

## 重写 filter_by

## 负责 sql 编写、链式调用

query.first()/all() 才会去查询

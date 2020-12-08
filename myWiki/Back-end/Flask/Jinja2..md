# Jinjia2 基本语法

1. 注释和访问数据
    - 添加注释：`{# this is comment #}`
    - 访问数据(传入的变量)：`{{ data }}`

2. 对象和字典的访问
    Jinjia2 在对象和字典的访问数据有两种方式，`date.name` 或者 `date['name']`，推荐使用第一种，更加方便和符合直觉；

3. 流程控制语句

if 语句

```Jinjia2
{% if data == 0 %}
    {{ do sonething }}
{% elif data == 1 %}
    {{ do sonething }}
{% else data == 2 %}
    {{ do sonething }}
{% endif %}
```

for 语句

普通的遍历

```Jinjia2
{% for i in data %}
    {{ i }}
{% endfor %}
```

字典的遍历

```Jinjia2
{% for key, value in date.item() %}
    {{ key }}
    {{ value }}
{% endfor %}
```

## 模板填充和继承

模板的继承，会使用 block 一个类似块的语句表示，记住还要 `endblock`

```JInjia2
    {% block head %}
    {% endblock %}

    {% block content %}
    {% endblock %}

    {% block foot %}
    {% endblock %}
```

比如上面是主要的模板文件，其他文件会负责进行填充，比如填充 block content 内容，只需要在另一个文件中也以此标签包含的内容就会填充；

注意：要在开始的时候声明一下，{% extends 'layout.html' %} 意思就是要填充这个 html 文件；

```Jinjia2
{% extends 'layout.html' %}
{% block content %}
<div>
{{ data.age }}
    <div>{{ data.name }}</div>
</div>
{% for foo in [1,2,3] %}
    <h1>{{ foo }}</h1>
{% endfor %}
{% endblock %}
```

这是填充，如果要是继承的话，还需要添加一个 {{ super() }} 的关键字；

```Jinjia2
{% extends 'layout.html' %}

{% block content %}
    {{ super() }}
    <div>
    {{ data.age }}
        <div>{{ data.name }}</div>
    </div>
    {% for foo in [1,2,3] %}
        <h1>{{ foo }}</h1>
    {% endfor %}
{% endblock %}
```

## 过滤器/管道命令

常用的过滤器

- default()
- length()
- first()

## 通过路由寻找资源的  url

在前端文件中，静态资源比如css/js/images，一般不会使用绝对路径和相对路径，而是使用 url_for('static', filename='test.css') 这种方式来进行访问；

## 设置变量和变量作用域

set 关键字设置变量，这个变量的作用域是全局的

```Jinjia2
{% set variable %}
{{ variable }}
```

使用 with 语句，限定变量的作用域

```Jinjia2
{% with %}

{% endwith %}
```

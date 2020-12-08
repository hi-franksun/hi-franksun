
# 使用 MySQL

1. 安装驱动 pymysql

```python
import pymysql
```

2. 使用驱动连接数据库

```python
connect = pymysql.connect(host='', port=3306, user='root', password='', database='test', charset='utf8')
```

3. 获取一个连接的游标

```python
cursor = connect.cursor()
```

4. 对游标进行操作，也就是操作数据库

```python
cursor.execute('show databases;')
data = curror.fetchone() # 获取条数据
cursor.execute('use databases_name;')
cursor.execute('show tables')
cursor.execute('select * from table_name')
date1 = cursor.fetchone() # 获取单条数据，调用一次获取下一条
data = cursor.fetchall() # 获取所有的数据
for i in data:
    print(i)

cursor.execute('drop table if exists user') # 如果存在删除
cursor.execute('create table user(id varchar(20) primary key, name varchar(20))') # 创建表格
cursor.execute('insert into user (id, name) value(%s, %s )',['1','james']') # 插入数据
connect.commit() # 提交事务
```

5. 如果是创建、修改、删除等操作，需要对游标进行事务提交

```python
connect.commit()
```

6. 关闭游标

```python
cursor.close()
```

7. 关闭数据库连接

```python
connect.close()
```


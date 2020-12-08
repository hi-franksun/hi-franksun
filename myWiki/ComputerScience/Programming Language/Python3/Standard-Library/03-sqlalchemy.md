


ORM：对象关系映射，将数据库中的表与面向对象语言中的类建立联系，操作数据库的表格就可以通过操作类或者实例实现；

## 1、安装库
```python
import sqlalchemy 
```

## 2、连接数据库

```python
from sqlalchemy import create_engine

DB_TYPE = 'mysql+pymysql'
DB_USERNAME = 'root'
DB_PASSWORD = 'password'
DB_HOST = 'localhost'
DB_PORT = '3306'
DB_NAME = 'test'
DB_CHARSET = 'utf8'
engine = create_engine(f'{DB_TYPE}://{DB_USERNAME}:{DB_PASSWORD}@{DB_HOST}:{DB_PORT}/{DB_NAME}?{DB_CHARSET}')

engine = create_engine() # 连接数据库，初始化数据库
```

## 3、创建数据库表类（模型）

declarative_base()是sqlalchemy内部封装的一个方法，通过其构造一个基类，这个基类和它的子类，可以将Python类和数据库表关联映射起来。

数据库表模型类通过__tablename__和表关联起来，Column表示数据表的列;
```python
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base() # 构造模型的基类

class Users(Base):
  __tablename__ = "users"
 
  id = Column(Integer, primary_key=True)
  name = Column(String(64), unique=True)
  email = Column(String(64))
  def __init__(self, name, email):
    self.name = name
    self.email = email
```


## 4、生成数据库表
创建表格，如果存在，那么会忽略，如果不存在会重新创建
```python
Base.metadata.create_all(engine)
```


## 5、session用于创建程序和数据库之间的会话
**所有对象的载入和保存都需要通过session对象，sessionmaker 调用创建一个工厂，并关联Engine以确保每个session都可以使用该Engine连接资源**

```python
from sqlalchemy.orm import sessionmaker

DBSession = sessionmaker(bind=engine) # 创建 DBSession 类型,bind:绑定
session = DBSession
```

## 6、利用 session 进行 curd
有了 session 这个接口，就可以正常和数据库交互了

### session.add() 增加
```python
add_user = Users("test", "test123@qq.com")
session.add(add_user)
session.commit()
```

### session.query() 进行 查询、修改、删除




## sqlalchemy 常用的数据类型：
| 类型 | 对应数据类型 |
| -- | -- |
| Integer | 整型，映射到数据库的 int 类型；|
| String | 字符类型，映射到数据库中的 varchar 类型；|
| Boolenn | 布尔类型，映射到数据库的 bool 类型；|
| Text | 文本类型，映射到数据库的 text 类型；|
| Date | 日期类型，映射到数据库的 date 类型，使用的时候传递 datetime.date()；|
| DateTime | 日期时间类型，映射到数据库的 datetime 类型，使用时候传递 datetime.datetime()；|
| Float | 浮点类型；|

## sqlalchemy 常用属性：
| 属性 | 说明 |
| -- | -- |
| primary_key | 主键，True和 False |
| autoincrement | 是否自增长，True和 False |
| unique | 是否唯一 |
| nullable | 是否可空，默认 True |
| default | 默认 |
| onupdate | - |


## mysql 常用的数据类型
| 类型 | 存储范围 | 占用字节 | 
| -- | -- | -- |
| TINYINT | 有符号值：`-128`到`127`（`-2^7` 到 `2^7-1`）无符号值：`0`到`255` （`0` 到 `2^8-1`） | 1 | 
| SMALLINT | 有符号值： `-32768`到`32767` （`-2^15` 到 `2^15-1`）无符号值：`0` 到 `65535` （`0` 到 `2^16-1`） | 2 | 
| MEDIUMINT | 有符号值： `-8388608` 到 `8388607` （`-2^23` 到 `2^23-1`）无符号值：`0` 到`16777215` （`2^24-1`） | 3 | 
| **INT** | 有符号值 ： `-2147483648`到`2147483647` （`-2^31` 到 `2^31-1`）无符号值: `0` 到 `4294967295` （`2^32-1`） | 4 |
| **BIGINT** | 有符号值 ：`-9223372036854775808` 到 `9223372036854775807` （`-2^63` 到 `2^63-1`）无符号值：`0` 到 `18446744073709551615` （`2^64-1`） | 8 |


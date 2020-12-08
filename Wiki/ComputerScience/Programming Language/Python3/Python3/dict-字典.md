# dict

>Python 3.7 起，常规字典已经保证有序。但是删除存在的 key，再添加相同的 key，这个时候会在尾部添加；

## what dict

dict（字典）在其他语言中是 map，dict 其实就是一种 hash 的类型，使用键值存储（key:value）,查找速度快，属于用空间换取时间的方式；

```python
{"key":"value"} -- {"city":"nanjing"}
```

> dict 的 key 是不可变对象，所以 **key可以是：str、int、tuple**，注意 tuple 中如果嵌套了 list 类似的可变对象，也是不能作为 key；
> dict 的 value 可以是任何数据类型**
> dict 是不能有相同的 key，如果存在相同的 key 内部会进行处理，保留最后一个**

---

## create dict

1. `{key: value}`
    直接用花括号，进行创建就可以了，这个也是性能最好的；

    ```python
    dict1 = {"a":1, "b":2}
    ```
2. `dict(**kwargs)`
    调用 `dict` 函数创建对象

3. `dict(mapping, **kwargs)`

4. `dict(iterable, **kwargs)`

---

## dict methods

> for 循环遍历是直接遍历 dict 的 key，而要单独遍历 value ，需要遍历 dict.values()，遍历 dict 的item，需要 dict.items()；而且字典中的很多操作都是针对 key，而不是 value。

1. dict[key]
    直接访问 key 的值就能查找到对应的 value，如果没有找到 key 会报错；

    return: vaule/KeyError

    ```python
    dict1 ={"city":"nanjing", "age":25}
    dict["city"]
    # nanjing
    dict["age"]
    #25
    ```

2. dict.get(key)
    利用内置的 get() 方法，找到会返回对应的 value，找不到对应的 value，会返回一个 None 值，或者自定义一个值，用来判断可增加可读性；

    return: value/None

    ```python
    dict1 ={"city":"nanjing", "age":25}
    print(dict1.get("city"))
    # "nanjing"
    print(dict1.get("cit"))
    # None
    a.get('weather', 'this dict has no key 3') 
    # this dict has no key 3
    ```

3. dict[key] == value

    修改：根据 `key` 直接修改 `value` 的值
    添加：添加 `key` 和 `value` 即可

    return: 无返回值

    ```python
    dict1 ={"city":"nanjing", "age":25}
    # modify
    dict1["city"] = "beijing"
    #{'city': 'beijing', 'age': 25}
    # add
    dict1["score"] = 100
    # {'city': 'beijing', 'age': 25, 'score': 100}
    ```

4. del dict[key]
    用 del 可以删除指定 key 的值；

    return：无返回值/KeyError

    ```python
    dict1 ={"city":"nanjing", "age":25}
    del dict1["city"]
    #{'age': 25}
    ```

5. del dict or dict.clean()
    dict 内置函数 clear() 可以删除整个字典内容，剩下一个空的字典，注意区别，只是删除内容，不是删除这个 dict 的引用；
    return None

    del 直接删除整个 dict，**这个时候这个 dict 就直接删除，再引用会发生错误。**

    ```python
    dict1 ={"city":"nanjing", "age":25}
    del dict1["city"]
    #{'age': 25}
    dict1.clear()
    #{}
    del dict1
    print(dict1)
    # 会报错，直接删除了这个 dict1 的变量和值
    ```

6. pop(key[, default])

    如果 key 存在于字典中则将其移除并返回其值，否则返回 default。 如果 default(不是关键字参数，可变参数，传入一个对象即可) 未给出且 key 不存在于字典中，则会引发 KeyError。

    return: value/default message/Keyerror

    ```python
    d = {'city': 'nanjing', 'age': '25'}

    print(d.pop('name', 'dict delete fail')) # dict delete fail
    print(d.pop('city', 'dict delete fail')) # nanjing
    print(d) # {'age': '25'}
    ```

7. popitem()
    从字典中移除并返回一个 (键, 值) 对。 键值对会按 LIFO 的顺序被返回，也就是后进先出，删除尾部的元素，同时由于 dict 是有序的了，所以原来的 dict 还是有序的，如果没有这个 key 会返回 Keyerror；

    popitem() 适用于对字典进行消耗性的迭代，这在集合算法中经常被使用。

    return：tuple(key, value)/Keyerror

    ```python
    d = {'city': 'nanjing', 'age': '25', 'name': 'ok'}
    print(d.popitem()) # ('name', 'ok')
    ```

8. dict.copy()
    返回原字典的浅拷贝；

9. dict.has_key(key)
    key 在 dict 中就返回 bool 值；

10. list(dict)
    return list about dict key(返回 dict key 列表)

11. len(dict)
    return element of dict length  

12. setdefault(key, default_key)
    设置默认值

---

## 字典视图对象

由 dict.keys(), dict.values() 和 dict.items() 所返回的对象是 视图对象。 该对象提供字典条目的一个动态视图，**这意味着当字典改变时，视图也会相应改变。**

> 以下说明是动态改变的

```python
d = {'city': 'nanjing', 'age': '25'}

result = d.items()
print(result)  # dict_items([('city', 'nanjing'), ('age', '25')])
d['city'] = 'beijing'
print(result)  # dict_items([('city', 'beijing'), ('age', '25')])
```

> 返回的字典视图对象是可逆的，也就是说可以使用 dict() 恢复刚才的 dict，可以使用 list()，tuple() 方法转成相应的数据类型；

```python
d = {'city': 'beijing', 'age': '25'}
result = d.items()
print(list(result)) # [('city', 'beijing'), ('age', '25')]
print(tuple(result)) # (('city', 'beijing'), ('age', '25'))
print(dict(result)) # {'city': 'beijing', 'age': '25'}
```

### dict.items()

把字典中每对 key 和 value 组成一个 tuple，再把这些 tuple 放在 list 中返回，这种数据类型是 dict_items（字典视图对象），其实就是遍历出 dict 的 key 和 value，然后再进行操作；

```python
a = {1: 1, 2: 2}
result = a.items() # dict_items([(1, 1), (2, '2')])
for key in result:
    print(key)
# (1, 1)
# (2, '2')

for key, value in result:
    print(key, value)
# 1 1
# 2 2

keys = []
values = []
for key, value in result:
    keys.append(key)
    values.append(value)
print(keys) # [1, 2]
print(values) # [1, '2']
```

### dict.keys()

获取 dict 的 keys

```python
d = {'city': 'nanjing', 'age': '25'}
result = d.keys()
print(result) # dict_keys(['city', 'age'])
```

### dict.values()

获取 dict 的 values

```python
d = {'city': 'nanjing', 'age': '25'}
result = d.values()
print(result) # dict_values(['nanjing', '25'])
```

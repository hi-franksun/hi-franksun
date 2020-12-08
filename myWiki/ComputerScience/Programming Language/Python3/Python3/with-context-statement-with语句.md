# context

```python
class A(object):

    def __enter__(self):
        return 'this is test context'

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('exit context')


if __name__ == '__main__':
    with A() as f: # f 是 __enter__ 返回的对象
        result = f.split(' ')
        print(result)
# ['this', 'is', 'test', 'context']
# exit context
```

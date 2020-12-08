# shutil

shutil: sh + util == shutil，即 shell 的工具包，处理以下文件和目录的 copy、move、delete 操作，比较高效；

## 1. copy file

- src：源码(source)
- dst：目的地，目标(destination)

### shutil.copyfileobj(fsrc, fdst[, length])

这个是复制文件内容，也就是 fsrc，读取 src 内容，然后写入 dst，如果 dst 不存在那么会自动创建，如果存在会覆盖；

```python
import shutil

src_path = './t1.py'
dst_path = './demo/t1.py'
print(shutil.copyfileobj(open(src_path, 'r', encoding='utf-8'), open(dst_path, 'w', encoding='utf-8')))
# None 返回 None
```

### shutil.copyfile(src, dst, *, follow_symlinks=True)

复制文件 src 的内容到 dst 并返回 dst，如果 dst 不存在则自动创建，src 和 dst 是字符串类型的路径名，如果 src 和 dst 指向同一个文件，抛出 SameFileError

```python
import shutil

src_path = './t1.py'
dst_path = './demo/t1.py'
print(shutil.copyfile(src_path, dst_path))
# ./demo/t1.py 返回 dst 的路径
```

### copytree(src, dst, symlinks=False, ignore=None, copy_function=copy2, ignore_dangling_symlinks=False)

> ignore_patterns(*patterns)
忽略满足匹配模式的文件和目录。示例如下：
`shutil.ignore_patterns('tmp*')`

递归复制以 `src` 为根目录的整个目录树，返回目标目录 dst，dst 必须是不存在的目录，它和它不存在的父目录都将被创建，使用 `copystat()` 复制目录元数据，使用 `copy2()` 复制文件内容和元数据。

symlinks：是否复制软链接；
ignore：指定不参与复制的文件，其值应该是一个 ignore_patterns() 方法；
copy_function：指定复制的模式。

```python
shutil.copytree('folder1', 'folder2', ignore=shutil.ignore_patterns( 'tmp*'))
```

### other copy

还有一些复制的操作，比如权限，时间属性等等，查 api 文档即可；

## 2. move file

### move(src, dst, copy_function=copy2)

This is similar to the Unix "mv" command. Return the file or directory's destination.

移动文件或目录到目标位置，如果目标位置 dst 是一个存在的目录，将 src 移动到 dst 路径下。可以移动目录，也可以移动单个文件，注意权限问题

```python
import shutil
shutil.move('folder1/', 'folder2/')
```

## 3. remove directory

### rmtree(path, ignore_errors=False, onerror=None)

```python
import shutil

remove_path = './demo/test/'  # 必须是 path，不能是 file，不然会报错
print(shutil.rmtree(remove_path)) # 返回 None
```

## 4. archiving files

### shutil.make_archive(base_name, format[, root_dir[, base_dir[, verbose[, dry_run[, owner[, group[, logger]]]]]]])

- base_name：归档的文件名称，不包括任何拓展名，
- root_dir：是一个目录，它将作为归档文件的根目录，归档中的所有路径都将是它的相对路径；例如，我们通常会在创建归档之前用 chdir 命令切换到 root_dir。

```python
from shutil import make_archive
import os
archive_name = os.path.expanduser(os.path.join('~', 'myarchive'))
root_dir = os.path.expanduser(os.path.join('~', '.ssh'))
make_archive(archive_name, 'gztar', root_dir)
# '/Users/tarek/myarchive.tar.gz' 返回归档的路径
```

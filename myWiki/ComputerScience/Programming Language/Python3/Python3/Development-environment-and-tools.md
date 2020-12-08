# enviroment

## venv 使用

`Python` 环境管理始终是一个难题，不同项目需要不同的解释器，不同项目又需要不同的依赖包，所以解释器的虚拟环境和第三方库的版本管理就成了一个必须要面对的问题。

现在比较常见的是  `venv` (`Python3` 自带虚拟环境) 和  `pipenv` 等等

建议 `pipenv` 现在存在很多问题，所以现阶段最好还是使用 `Python3` 自带的 `venv` 库进行虚拟环境的创建最为合适。

### 创建虚拟环境

```python
# 命令
python -m venv /user/TestVenv/(创建虚拟环境的磁盘地址)
```

### 使用虚拟环境（进入虚拟环境）

```shell
# 不同的操作系统启动文件后缀不一样，但是名字都是 activate
# 也就是进入 venv/Scripts/ 文件夹下，启动 activate 文件就可以了
 <venv>\Scripts\activate.bat
# 关闭虚拟环境是
 deactivate

 # mac 平台激活方式不一样，mac 平台激活方式是放在 /bin/ 目录下激活，文件名还是一样的

```

### 安装第三方库

```python
# 进入到虚拟环境
# (venv) PS C:\Users\TT\Github\flask_demo\venv\Scripts>

前面会显示一个 (venv) 标识，这个时候和正常的操作没有区别了，就是正常安装就可以了
```

### 运行文件

```bash
# 运行 py文件，正常运行就可以了
python main.py
```

### 需改安装库的源

- 阿里源："https://mirrors.aliyun.com/pypi/simple"
- 豆瓣源："https://pypi.douban.com/simple"

> pip install SQLAlchemy -i https://pypi.douban.com/simple

---

## jupyter notebook

[jupyter offices website](https://jupyter.org/)

1. install jupyterlab

    ```shell
    pip install jupyterlab
    ```

2. start jupyterlab

    ```shell
    jupyter notebook
    ```

[jupyter 使用文章](https://mp.weixin.qq.com/s/fpWtJAOb_Uz5ApndtXAlyw)

## Pycharm

## VSCode

# shell 简述

常用的 Shell 版本

- bash
- sh
- zsh
常用的一般是 bash

```bash
sunyunxian@sunyunxian:/$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
```

## variable(变量)

变量的定义：

```bash
a=1
a="test is test"
a='this book name is "run run run" '

echo $a
echo ${a} # 两种使用变量的方法，针对不同情况分别使用就好
```

- 变量名大小写敏感
- **= 左右不要有空格**
- 如果内容有空格，需要使用单引号/双引号
- **双引号支持转义，$ 开头的变量会被自动替换**，单引号不支持引用
- **变量未声明，使用的时候是空值**

### 预定义变量

Linux 启动后就存在的变量

```bash
$PWD
$USER
$HOME
$PATH # Linux 环境变量，可以在任意目录下执行脚本、文件等，不需要在切换到该目录下
```

### 数组变量

数组定义：小括号内放置所有的元素，不使用任何符号分割，直接使用空格进行分割；

```bash
array=(1 2 3 4 5)

echo $array
echo ${array[*]}
```

输出的时候默认会显示 array index 为 0 的元素，获取相应的 index 直接使用索引就可以了；
如果要输出素有元素使用 {array[@]}/{array[*]}，获取长度使用  {#array[@]/{#array[*]；

### 反引号使用命令

数组中可以执行相关的 shell 命令并命名给变量

```bash
test = `pwd`
array=(`pwd`)
echo ${test}  # /home/sunyunxian/learn-shell
echo ${array} # /home/sunyunxian/learn-shell
```

也可以不使用反引号，直接使用 $(pwd) 也是可以的，就是通用性会差一点，有的版本不支持；

### 转义模式

echo -e 使转义字符生效

```bash
echo -e "hello\nworld"
```

算法拓展：echo (())


## shell 变量类型

string：a="test"
number：a=123
bool：a=false/true

## 字符串操作

### 字符串长度

```bash
${#varname}
```

### 子字符串

offset：表示的偏移是直接从 0 计算的，偏移 6 个，则显示第 6 个开始。

```bash
${varname:offset:length} # offset:偏移，表示对于原目标的标准，进行的便宜，length:偏移的长度，默认是到末尾
```

切片

### 搜索与替换

双引号和不加双引号

```bash
s="hello from develoer"
echo ${s#hello}
echo "${s#hello}"
# 非贪婪匹配和贪婪匹配
```

头部模式匹配
尾部模式匹配

字符串替换

### boolen 值

执行成功永远是 0
执行不能成永远不是 0


判断：
算数判断
字符串判断
逻辑判断
shell内置判断

### 改变大小写


## 算术运算

[ 2 -eq 2 ]等于
[ 2 -ne 2 ]不等于
[ 2 -gt 1 ] 大于
-ge 大于等于
-lt 小于
-le 小于等于

[ 2 -ne 2 -a 2 -gt 1 ] 逻辑与
[ 2 -ne 2 -o 2 -gt 1 ] 逻辑或


### 算数表达式


echo $? 返回 0 代表执行成功，返回 1 代表执行失败

浮点数运算需要 awk 支持，原始的计算是不支持的

## 逻辑控制



## shell 环境


## 脚本应用


## 自动化

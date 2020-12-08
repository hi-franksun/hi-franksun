
# What Git

> Official website: [Git Official website](https://git-scm.com/)

> Short introduction: Git is a free and open source distributed version control system;

## About Verson Control

## History of Git

# Getting Started





## 安装 git

### window平台
下载 git [下载地址 ](https://git-scm.com/)


## git 基础配置

1. 配置 user 信息 user.name user.email
提交个人信息和邮箱，这样版本管理时候便于管理不同成员提交代码和邮箱通知。
```bash
git config --global user.name 'your_name'
git config --global user.email 'your_email'
```

2. config 作用域（缺省等于 local）
```bash
# 只对某个仓库有效，切换别的仓库不会生效 git config --local
# 对当前用户所有的仓库有效 git config --global
# 对系统所有登录用户有效 gir config --system
```

3. 显示 config 配置 加 --list
```bash
git config --list # 显示所有的配置
git config --list --local
git config --list --global
git config --list --system
```


## 创建 git 仓库

### 已有项目
```bash
cd 项目所在文件夹
git init # 初始化项目
```

### 新建项目
```bash
cd 某个文件夹
git init your_project # 会在当前路径下创建与项目同名文件夹
cd your_project # 切换到项目文件夹下，就可以开始工作了
```

### 项目信息配置
如果存在全局的 config ，如果想在项目下边创建不一样的管理者，这个时候可以用 `local` ，就单独配置了，不会用全局 `global` 配置信息。



































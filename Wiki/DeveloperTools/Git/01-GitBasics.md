

git 有三个区


### 工作区和暂存区

working directory:工作区
stage:暂存区

## 解析 .git 目录

```bash
git cat-file -t string # 查看字符串类型 
git cat-file -p string # 查看字符串内容
```

.git 目录下的文件内容
```bash
-rw-r--r-- 1 TT 197121   17 8月  25 20:41 COMMIT_EDITMSG
-rw-r--r-- 1 TT 197121  358 8月  25 21:28 config # 一些配置信息
-rw-r--r-- 1 TT 197121   73 8月  23 23:56 description
-rw-r--r-- 1 TT 197121   90 8月  25 17:11 FETCH_HEAD
-rw-r--r-- 1 TT 197121 1200 8月  25 21:28 gitk.cache
-rw-r--r-- 1 TT 197121   23 8月  23 23:56 HEAD
drwxr-xr-x 1 TT 197121    0 8月  23 23:56 hooks/
-rw-r--r-- 1 TT 197121  919 8月  25 20:41 index
drwxr-xr-x 1 TT 197121    0 8月  23 23:56 info/
drwxr-xr-x 1 TT 197121    0 8月  23 23:56 logs/
drwxr-xr-x 1 TT 197121    0 8月  25 20:40 objects/
-rw-r--r-- 1 TT 197121   41 8月  25 17:11 ORIG_HEAD
-rw-r--r-- 1 TT 197121  114 8月  23 23:56 packed-refs
drwxr-xr-x 1 TT 197121    0 8月  23 23:56 refs/ # 引用
```

### HEAD
查看当前的分支，可以手动修改，这个就和命令行切换分支一样的效果
```bash
$ cat HEAD
ref: refs/heads/master # 查看当前的分支
```

### config
查看 git 的配置。
```bash
$ cat .git/config
[core]
        repositoryformatversion = 0
        filemode = false
        bare = false
        logallrefupdates = true
        symlinks = false
        ignorecase = true
[remote "origin"]
        url = git@github.com:sunyunxian/coding.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[gui]
        wmstate = normal
        geometry = 841x483+187+80 189 218

```

### refs
查看 git 的引用
```bash
$ ls -al
total 4
drwxr-xr-x 1 TT 197121 0 8月  23 23:56 ./
drwxr-xr-x 1 TT 197121 0 8月  25 21:28 ../
drwxr-xr-x 1 TT 197121 0 8月  25 20:41 heads/ # 分支，独立的开发空间，比如前后端用不同分支，工作不影响，集成时候可以合成一个主分支
drwxr-xr-x 1 TT 197121 0 8月  23 23:56 remotes/ # 远程
drwxr-xr-x 1 TT 197121 0 8月  23 23:56 tags/ # 标签(里程碑)，比如版本开发到 1.0 版本，可以打一个标签
```

查看 heads
```bash
$ ll
total 1
-rw-r--r-- 1 TT 197121 41 8月  25 20:41 master # 显示不同的分支
$ cat master
fef5d6af8fefdb7ffb02d84f73e6e173361ebe1d
$ git cat-file -t fef5d6 # 查看类型
commit
$ git cat-file -p fef5d6 # 查看内容
tree 2248e26c91eda2b5bd4e6948fd1625d04b97c7c4
parent 1b8849a4ac1c978d1925debc2ad918c6eb9b462e
author youtai <work_sunyunxian@icloud.com> 1566736917 +0800
committer youtai <work_sunyunxian@icloud.com> 1566736917 +0800

rename README.MD
```


查看 tags:
```bash
$ ls -al
v1.0 # 已经创建的 tags
$ cat v1.0
59a4......
$ git cat-file -t 59a4
tag # 是一个 tag 类型
$ git cat-file -p 59a4
object 2ffjhaodisuio....
type commit
tag v1.0
# 标签的内容，包括标签者信息，时间戳，时区
tagger sunyunxian <work_sunyunxian@icloud.com> 111111 +0800

```

### objects
查看 git 的对象
```bash
$ ls -al
total 12
drwxr-xr-x 1 TT 197121 0 8月  25 20:40 ./
drwxr-xr-x 1 TT 197121 0 8月  25 21:28 ../
drwxr-xr-x 1 TT 197121 0 8月  25 19:10 03/
drwxr-xr-x 1 TT 197121 0 8月  25 17:50 05/
drwxr-xr-x 1 TT 197121 0 8月  25 18:45 06/
drwxr-xr-x 1 TT 197121 0 8月  25 18:38 08/
drwxr-xr-x 1 TT 197121 0 8月  25 18:02 09/
drwxr-xr-x 1 TT 197121 0 8月  25 19:10 10/
drwxr-xr-x 1 TT 197121 0 8月  25 16:30 17/
drwxr-xr-x 1 TT 197121 0 8月  25 18:38 d2/
drwxr-xr-x 1 TT 197121 0 8月  25 18:59 f2/
drwxr-xr-x 1 TT 197121 0 8月  25 20:33 f3/
drwxr-xr-x 1 TT 197121 0 8月  25 16:29 fa/
drwxr-xr-x 1 TT 197121 0 8月  25 20:33 fb/
drwxr-xr-x 1 TT 197121 0 8月  25 20:41 fe/
drwxr-xr-x 1 TT 197121 0 8月  25 16:30 ff/
drwxr-xr-x 1 TT 197121 0 8月  23 23:56 info/
drwxr-xr-x 1 TT 197121 0 8月  23 23:56 pack/

$ cd f3 # 进入目录
$ ll # 查看内容，是一段字符串，也就是一段哈希值
total 1
-r--r--r-- 1 TT 197121 86 8月  25 20:33 43d5052d1f11a15ebbbb9996e87c60e848f5d3

$ git cat-file -t `f3`43d5052d1f11a # 查看这段字符类型，注意前面要带上 `f3`
tree # 类型显示一个 tree
$  git cat-file -p f343d5052d1f11a # 查看这段字符的内容，注意前面要带上 `f3`
#        文件对象       哈希值
100644 blob a338e9a9bc65d2ef6e33ba4ea077c559c0275437    git-000.md
100644 blob 5bad3ed7d7ac9a181c30b3c38d83c4e2b0fb7a23    readme.md
$  git cat-file -t a338e # 查看这个哈希值的类型
blob # 类型显示 blob

TT@DESKTOP-MQVA91K MINGW64 /d/Github/coding/.git/objects/f3 (GIT_DIR!)
$  git cat-file -p a338e # 产看哈希值的内容
# 以下就是文件的内容了
```

![](areas.png)



![](lifecycle.png)



![](git_over.png)

Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库


## Git Ignoring Files

开发过程中，很多文件不需要追踪的，比如敏感的数据、日志、编译过程中产生的临时文件，所以需要一个忽略文件的模式，git 中使用一个文件实现这个模式，.gitignore，上边会列出需要忽略的文件或者是忽略模式；

在一开始创建仓库的时候就要创建这个文件，这是个非常好的习惯！同时也能避免很多不安全的事情发生；

.gitignore 格式规范：
- 所有空行或者以 # 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。

GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表， 你可以在 [GitHub gitignore](https://github.com/github/gitignore)

---

## Git Remove File
git rm file

如果在暂存区内，就要强制的 git rm -f file 删除，这样子文件就不会出现在下次跟踪的内容了，工作目录也会相应的删除掉；

---

## Move/Rename File

git mv file_old file_new

其实这个 mv 执行了 3 步操作，就是

```bash
mv file_old file_new
git rm file_old
git add file_new
```
---

## Git Commit History
查看提交历史记录

### git status
查看当前 branch 的状态，显示有变更的文件和一些其他提醒等；
```bash
$ git status
On branch learnGit
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Software_Engineering/ProgrammingTools/Git/01-GitBasics.md

no changes added to commit (use "git add" and/or "git commit -a")
```

### git log

```bash
git log [<options>] [<revision range>] [[--] <path>…​]
```


git log 会列出 commit history，默认显示会按照时间的先后顺序，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

```bash
$ git log
commit 628f0ee456c36fbc1742c160afced4ca3709e9ad (HEAD -> master, origin/master, origin/HEAD)
Merge: aed3ff3 71d3e4c
Author: 友台 <work_sunyunxian@icloud.com>
Date:   Thu Jul 23 17:46:01 2020 +0800

    Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git

commit aed3ff352ee4d00e768e69fdc5dbcca0dbd6b5e3
Author: 友台 <work_sunyunxian@icloud.com>
Date:   Thu Jul 23 17:45:28 2020 +0800

    update folder name

commit 71d3e4c5254bb63734a64a2a001ec33d795ecca0
Author: sunyunxian <work_sunyunxian@icloud.com>
Date:   Wed Jul 22 20:05:37 2020 +0800
```


#### git log -n
查看过去几次的提交历史，这个一般和其他命令搭配使用；

$ git log -2
commit d6a652bf3d4f911ef8a86e0d3398530d120a0f15 (HEAD -> learnGit, origin/master, origin/HEAD, master)
Author: 友台 <work.sunyunxian@gmail.com>
Date:   Fri Jul 24 12:04:52 2020 +0800

    update GitBasics.md

commit 628f0ee456c36fbc1742c160afced4ca3709e9ad
Merge: aed3ff3 71d3e4c
Author: 友台 <work_sunyunxian@icloud.com>
Date:   Thu Jul 23 17:46:01 2020 +0800

    Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git



#### git log --stat
Statistic:统计信息，显示 commit history，并且显示变化的文件；

```bash
$ git log --stat
commit d6a652bf3d4f911ef8a86e0d3398530d120a0f15 (HEAD -> learnGit, origin/master, origin/HEAD, master)
Author: 友台 <work.sunyunxian@gmail.com>
Date:   Fri Jul 24 12:04:52 2020 +0800

    update GitBasics.md

 .../ProgrammingTools/Git/01-GitBasics.md           | 130 ++++++++++++++++++++-
 1 file changed, 129 insertions(+), 1 deletion(-)

commit 628f0ee456c36fbc1742c160afced4ca3709e9ad
Merge: aed3ff3 71d3e4c
Author: 友台 <work_sunyunxian@icloud.com>
Date:   Thu Jul 23 17:46:01 2020 +0800

    Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git

commit aed3ff352ee4d00e768e69fdc5dbcca0dbd6b5e3
Author: 友台 <work_sunyunxian@icloud.com>
Date:   Thu Jul 23 17:45:28 2020 +0800

    update folder name

 .../00-pytest/01-\345\237\272\347\241\200.md"                             | 0
 .../01-Django/01-\345\237\272\347\241\200.md"                             | 0
 {Software_engineering => Software_Engineering}/01-Django/03-orm.md        | 0
 .../02-PyCharm/\345\270\270\347\224\250\351\205\215\347\275\256.md"       | 0
 .../03-Vim/Vim\345\237\272\346\234\254\346\223\215\344\275\234.md"        | 0
 .../ProgrammingTools/04-VSCode/VSCode\345\205\245\351\227\250.md"         | 0
 .../ProgrammingTools/Git/00-WhatGit-GettingStarted.md                     | 0
 .../ProgrammingTools/Git/01-GitBasics.md                                  | 0
 .../ProgrammingTools/Git/02-GitBranching.md                               | 0
 .../ProgrammingTools/Git/git-001.md                                       | 0
 .../ProgrammingTools/Git/git-004.md                                       | 0
 .../ProgrammingTools/Git/git-006.md                                       | 0
 .../ProgrammingTools/Git/readme.md                                        | 0

```


#### git log --graph
graphic:图形 
图形化显示分支和合并关系，默认会显示成这样子，一般会与其他命令结合使用；

```bash
$ git log --graph
*   commit 628f0ee456c36fbc1742c160afced4ca3709e9ad (HEAD -> master, origin/master, origin/HEAD)
|\  Merge: aed3ff3 71d3e4c
| | Author: 友台 <work_sunyunxian@icloud.com>
| | Date:   Thu Jul 23 17:46:01 2020 +0800
| |
| |     Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git
| |
| * commit 71d3e4c5254bb63734a64a2a001ec33d795ecca0
| | Author: sunyunxian <work_sunyunxian@icloud.com>
| | Date:   Wed Jul 22 20:05:37 2020 +0800
| |
| |     add some image about me
| |
* | commit aed3ff352ee4d00e768e69fdc5dbcca0dbd6b5e3
|/  Author: 友台 <work_sunyunxian@icloud.com>
|   Date:   Thu Jul 23 17:45:28 2020 +0800
|
|       update folder name
|
* commit e9399d8d2848a9bb22a3886a8fa4099389b2ab9a
| Author: 友台 <work_sunyunxian@icloud.com>
| Date:   Wed Jul 22 18:19:48 2020 +0800
```

#### git log --pretty=oneline
查看单行，显示每个 commit 的 SHA-1 校验和 commit messages

```bash
$ git log --pretty=oneline
628f0ee456c36fbc1742c160afced4ca3709e9ad (HEAD -> master, origin/master, origin/HEAD) Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git
aed3ff352ee4d00e768e69fdc5dbcca0dbd6b5e3 update folder name
71d3e4c5254bb63734a64a2a001ec33d795ecca0 add some image about me
e9399d8d2848a9bb22a3886a8fa4099389b2ab9a move folder
79e4dc61ffc8101e11e3e6df0b5c9ad6c6d9f355 rename folder name
ccf54ea3437a8f0a185e8863634b74ff19dd1bf7 update folder 编程语言 to programingLanguage
040a7ec698cd1972202daf401362afa514e0b463 update English name
6635bb40e8f9b7e78bc16d1773d5de57545fe874 Update git-001.md
a8e7dcd31a03b6726ad0e4a5bf825a768350761b learn Git branching
32c6780c662cab7f1fc972777cbfd99554426869 Git Tagging
01223163077096770a9d19058cfa996f1d8fdb60 update Git schedule
26d9e732ede9f87ce225a83b827ce8ed60b4b675 Get started about Git
2ffd9f10b440de3ec6be4b3ec7676f839171ba49 rename folder to ProgrammingTools
148b549b5298b08668bfdee20bfb17f7675d92e5 update Readme
d582ff13da7abd7064f97a2cb4fe0fa0adb7b0ce git
f74796369e5c2442cf73a9cdf31757fba47aa2e4 解决冲突
6591d1fcec866e1fa871ee6df30d5f5e1e813947 data structure

```

#### git log --pretty=format:"%H" 
格式化输出日志，能输出自己想要的日志格式，这个最常用，一般与其他命令搭配使用；

常用的选项如下

| 选项	| 说明 |
| --- | --- |
| %H | 提交的完整哈希值 |
| %h | 提交的简写哈希值 |
| %T | 树的完整哈希值 |
| %t | 树的简写哈希值 |
| %P | 父提交的完整哈希值 |
| %p | 父提交的简写哈希值 |
| %an | 作者名字 |
| %ae | 作者的电子邮件地址 |
| %ad | 作者修订日期（可以用 --date=选项 来定制格式）|
| %ar | 作者修订日期，按多久以前的方式显示 |
| %cn |提交者的名字 |
| %ce | 提交者的电子邮件地址 |
| %cd | 提交日期 |
| %cr | 提交日期（距今多长时间）|
| %s | 提交说明 |


#### 混合使用

Example1：以图形化格式输出日志：校验值、提交人、时间、内容
git log --pretty=format:"%h - %an, %ar : %s" --graph
```bash
$ git log --pretty=format:"%h - %an, %ar : %s" --graph
*   628f0ee - 友台, 18 hours ago : Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git
|\
| * 71d3e4c - sunyunxian, 2 days ago : add some image about me
* | aed3ff3 - 友台, 18 hours ago : update folder name
|/
* e9399d8 - 友台, 2 days ago : move folder
* 79e4dc6 - 友台, 2 days ago : rename folder name
* ccf54ea - 友台, 2 days ago : update folder 编程语言 to programingLanguage
* 040a7ec - 友台, 2 days ago : update English name
*   6635bb4 - 友台, 3 days ago : Update git-001.md
|\
| * 6da8e01 - sunyunxian, 4 months ago : 测试分支
* | a8e7dcd - 友台, 3 days ago : learn Git branching
* | 32c6780 - 友台, 3 days ago : Git Tagging
* | 0122316 - 友台, 5 days ago : update Git schedule
* | 26d9e73 - 友台, 6 days ago : Get started about Git
* | 2ffd9f1 - 友台, 6 days ago : rename folder to ProgrammingTools
* | 148b549 - 友台, 6 days ago : update Readme
* |   d582ff1 - sunyunxian, 10 days ago : git
|\ \
| * \   f747963 - 友台, 5 weeks ago : 解决冲突
```

Example2：git log --pretty=oneline --graph
这个命令会显示的更加详细

```bash
$ git log --pretty=oneline --graph
*   628f0ee456c36fbc1742c160afced4ca3709e9ad (HEAD -> master, origin/master, origin/HEAD) Merge branch 'Git' of https://github.com/youtai2020/Wiki into Git
|\
| * 71d3e4c5254bb63734a64a2a001ec33d795ecca0 add some image about me
* | aed3ff352ee4d00e768e69fdc5dbcca0dbd6b5e3 update folder name
|/
* e9399d8d2848a9bb22a3886a8fa4099389b2ab9a move folder
* 79e4dc61ffc8101e11e3e6df0b5c9ad6c6d9f355 rename folder name
* ccf54ea3437a8f0a185e8863634b74ff19dd1bf7 update folder 编程语言 to programingLanguage
* 040a7ec698cd1972202daf401362afa514e0b463 update English name
*   6635bb40e8f9b7e78bc16d1773d5de57545fe874 Update git-001.md
|\
| * 6da8e01f318cfe75d264875bdab414624cc1e450 测试分支
* | a8e7dcd31a03b6726ad0e4a5bf825a768350761b learn Git branching
* | 32c6780c662cab7f1fc972777cbfd99554426869 Git Tagging
* | 01223163077096770a9d19058cfa996f1d8fdb60 update Git schedule
* | 26d9e732ede9f87ce225a83b827ce8ed60b4b675 Get started about Git
* | 2ffd9f10b440de3ec6be4b3ec7676f839171ba49 rename folder to ProgrammingTools
* | 148b549b5298b08668bfdee20bfb17f7675d92e5 update Readme
* |   d582ff13da7abd7064f97a2cb4fe0fa0adb7b0ce git
|\ \
| * \   f74796369e5c2442cf73a9cdf31757fba47aa2e4 解决冲突
```


#### git log -S [keyword]
搜索提交历史，根据关键词，这个非常好用



#### git log --since/after= --until/before

根据时间查询，since/after：指定时间之后的提交，until/before：指定时间之前的提交

时间格式有两种：
- "2008-10-01" 必须有短线
- 2years 2months 2weeks 1day 3minutes ago 相对日期的查看

#### git log -- folder/
查看指定目录的提交历史

```bash
$ git log -- image/
commit 71d3e4c5254bb63734a64a2a001ec33d795ecca0
Author: sunyunxian <work_sunyunxian@icloud.com>
Date:   Wed Jul 22 20:05:37 2020 +0800

    add some image about me

commit a5f1556f103eccc78df190a7c3c718c9ef4ba455
Author: youtai <work_sunyunxian@icloud.com>
Date:   Mon Aug 26 02:23:49 2019 +0800

    add git-002.md

commit fb469db4833d4f433eecace39c0a23a87af5cccd
Author: youtai <work_sunyunxian@icloud.com>
Date:   Sun Aug 25 20:33:32 2019 +0800

    add image wechat.jpg
```

### other view commit history 
git log --author=
查看指定提交者的提交

git shortlog -sn
查看所有提交者每个人的提交次数
```bash
$ git shortlog -sn
    78  youtai
    63  sunyunxian
    43  友台
```
---









## Remotes Repository
远程仓库



## Git Tag

command line
 tag：Create, list, delete or verify a tag object signed with GPG

### Creating Tags


### Listing Tags

#### Annotated Tags

#### Lightweight Tags

### Tagging Later



### Sharing Tags

### Deleting Tags

### Checking out Tags


## Git Aliases







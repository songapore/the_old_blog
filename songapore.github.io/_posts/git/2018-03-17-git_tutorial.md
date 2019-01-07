---
layout: post
title:  "Git教程"
date:   2018-03-17 16:45:13
categories: git
tags: git github
---

* content
{:toc}

ROS教程-0.1 ROS安装-《用ROS学习机器人编程的系统方法》
<!--more-->

# 在Windows上安装Git
在Windows上使用Git，可以从Git官网直接[下载安装程序](https://git-scm.com/downloads)，（[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

# 第一个版本库repository
什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
## 初始化一个Git仓库
通过git init命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
## 把文件添加到版本库

### git add添加文件

```
$ git add readme.txt
```

### git commit提交

```
$ git commit -m "wrote a readme file"
[master (root-commit) cb926e7] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```
-m 后面输入的是本次提交的说明，可以输入任意内容
### 工作流程图
整个上述过程可以被这张 git 官网上的流程图直观地表现:

![image](http://p5ocy6pck.bkt.clouddn.com/2-1-1.png)

- 图中告诉我们存在4个状态：untracked, unmodified,modified,staged
- 创建的新文件，且没有git add 则属于untracked状态；add之后为staged状态
- staged状态 git commit 之后为 unmodified 状态
- 当我们修改后，变为modified 状态；需要继续git add 添加，才变为staged状态
- 当我们git rm之后，变为 untracked状态

## 记录及修改 (log & diff)
在 git 中, 每一次提交(commit)的修改, 都会被单独的保存起来. 也可以说 git 的中的所有文件都是一次次修改累积起来的. 文件好比楼房, 每个 commit 记录 了盖楼需添加或者拿走的材料. 整个施工过程也被记录了下来.
### 修改记录 log
我们来查看版本库的施工过程
```
$ git log
```
### 查看状态status
git status命令可以让我们时刻掌握仓库当前的状态
```
$ git status
```
- 要随时掌握工作区的状态，使用git status命令。

- 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。


### 查看 unstaged
如果想要查看这次还没 add (unstaged) 的修改部分 和上个已经 commit 的文件有何不同, 我们将使用 $ git diff:

```
$ git diff
```
### 查看 staged (--cached)
如果你已经 add 了这次修改, 文件变成了 “可提交状态” (staged), 我们可以在 diff 中添加参数 --cached 来查看修改:

```
$ git add .   # add 全部修改的文件
$ git diff --cached
```

# 回到从前
## 回到从前 (reset)
### 修改已 commit 的版本
有时候我们总会忘了什么, 比如已经提交了 commit 却发现在这个 commit 中忘了附上另一个文件。

接下来我们模拟这种情况.我们最后一个 commit 是 change 2, 我们将要添加另外一个文件, 将这个修改也 commit 进 change 2. 所以我们复制 1.py 这个文件, 改名为 2.py. 并把 2.py 变成 staged, 然后使用 **--amend** 将这次改变合并到之前的 change 2 中.

```
$ git add 2.py
$ git commit --amend --no-edit   # "--no-edit": 不编辑, 直接合并到上一个 commit
$ git log --oneline    # "--oneline": 每个 commit 内容显示在一行
```
### reset 回到 add 之前

有时我们添加 add 了修改, 但是又后悔, 并想补充一些内容再 add. 这时, 我们有一种方式可以回到 add 之前.
```
$ git add 1.py
$ git status
#后悔了 咋办
$ git reset 1.py
```
### reset 回到 commit 之前
在穿梭到过去的 commit 之前, 我们必须了解 git 是如何一步一步累加更改的。
![image](http://p5ocy6pck.bkt.clouddn.com/2-2-1.png)
![image](http://p5ocy6pck.bkt.clouddn.com/2-2-2.png)

每个 commit 都有自己的 id 数字号, HEAD 是一个指针, 指引当前的状态是在哪个 commit. 最近的一次 commit 在最右边, 我们如果要回到过去, 就是让 HEAD 回到过去并 reset 此时的 HEAD 到过去的位置.


```
# 不管我们之前有没有做了一些 add 工作, 这一步让我们回到 上一次的 commit
$ git reset --hard HEAD    
# 输出
HEAD is now at 904e1ba change 2
-----------------------
# 看看所有的log
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
-----------------------
# 回到 c10ea64 change 1
# 方式1: "HEAD^"
$ git reset --hard HEAD^  

# 方式2: "commit id"
$ git reset --hard c6762a1
-----------------------
# 看看现在的 log
$ git log --oneline
# 输出
c6762a1 change 1
13be9a7 create 1.py
```
怎么 change 2 消失了!!! 还有办法挽救消失的 change 2 吗? 我们可以查看 $ git reflog 里面最近做的所有 HEAD 的改动, 并选择想要挽救的 commit id:

```
$ git reflog
# 输出
c6762a1 HEAD@{0}: reset: moving to c6762a1
904e1ba HEAD@{1}: commit (amend): change 2
0107760 HEAD@{2}: commit: change 2
c6762a1 HEAD@{3}: commit: change 1
13be9a7 HEAD@{4}: commit (initial): create 1.py
```
重复 reset 步骤就能回到 commit (amend): change 2 (id=904e1ba)这一步了:

```
$ git reset --hard 904e1ba
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
```
我们又再次奇迹般的回到了 change 2.

## 回到从前 (checkout 针对单个文件)
之前我们使用 reset 的时候是针对整个版本库, 回到版本库的某个过去. 不过如果我们只想回到某个文件的过去, 又该怎么办呢?
### 改写文件 checkout
我们仅仅要对 1.py 进行回到过去操作, 回到 c6762a1 change 1 这一个 commit.

使用 **checkout + id c6762a1 + -- + 文件目录 1.py**, 我们就能将 1.py 的指针 HEAD 放在这个时刻 c6762a1:

```
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
---------------------
$ git checkout c6762a1 -- 1.py
```
## 分支管理
### 分支 (branch)
很多时候我们需要给自己或者客户用一个稳定的版本库, 然后同时还在开发另外一个升级版. 自然而然, 我们会想到把这两者分开处理, 用户使用稳定版, 我们开发我们的开发版. 不过 git 的做法却不一样, 它把这两者融合成了一个文件, 使用不同的分支来管理.
### 分支 图例
之前我们说编辑的所有改变都是在一条主分支 master 上进行的. 通常我们会把 master 当作最终的版本, 而开发新版本或者新属性的时候, 在另外一个分支上进行, 这样就能使开发和使用互不干扰了
![image](http://p5ocy6pck.bkt.clouddn.com/branch.PNG)

### 使用 branch 创建 dev 分支
我们之前的文件当中, 仅仅只有一条 master 分支, 我们可以通过 --graph 来观看分支

```
$ git log --oneline --graph
# 输出
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```
接着我们建立另一个分支 dev, 并查看所有分支:

```
$ git branch dev    # 建立 dev 分支
$ git branch        # 查看当前分支

# 输出
  dev       
* master    # * 代表了当前的 HEAD 所在的分支
```
### 使用 checkout 切换到 dev 分支
当我们想把 HEAD 切换去 dev 分支的时候, 我们可以用到上次说的 checkout:

```
$ git checkout dev

# 输出
Switched to branch 'dev'
--------------------------
$ git branch

# 输出
* dev       # 这时 HEAD 已经被切换至 dev 分支
  master
```
使用 checkout -b + 分支名, 就能直接创建和切换到新建的分支:


```
$ git checkout -b  dev

# 输出
Switched to a new branch 'dev'
--------------------------
$ git branch

# 输出
* dev       # 这时 HEAD 已经被切换至 dev 分支
  master
```

### 在 dev 分支中修改
因为当前的指针 HEAD 在 dev 分支上, 所以现在对文件夹中的文件进行修改将不会影响到 master 分支.

我们修改dev分支, 然后再 commit:
```
$ git commit -am "change 3 in dev"  # "-am": add 所有改变 并直接 commit
```
假设我们的开发板 dev 已经更新好了, 我们要将 dev 中的修改推送到 master 中, 大家就能使用到正式版中的新功能了.

首先我们要切换到 master, 再将 dev 推送过来.
```
$ git checkout master   # 切换至 master 才能把其他分支合并过来

$ git merge dev         # 将 dev merge 到 master 中
$ git log --oneline --graph

# 输出
* f9584f8 change 3 in dev
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

要注意的是, 如果直接 git merge dev, git 会采用默认的 Fast forward 格式进行 merge, 这样 merge 的这次操作不会有 commit 信息. log 中也不会有分支的图案. 我们可以采取 --no-ff 这种方式保留 merge 的 commit 信息.
```
$ git merge --no-ff -m "keep merge info" dev         # 保留 merge 信息
$ git log --oneline --graph

# 输出
*   c60668f keep merge info
|\  
| * f9584f8 change 3 in dev         # 这里就能看出, 我们建立过一个分支
|/  
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```
## merge 分支冲突
情况是这样, 想象不仅有人在做开发版 dev 的更新, 还有人在修改 master 中的一些 bug. 当我们再 merge dev 的时候, 冲突就来了. 因为 git 不知道应该怎么处理 merge 时, 在 master 和 dev 的不同修改.

当创建了一个分支后, 我们同时对两个分支都进行了修改.
比如在:

- master 中的 1.py 加上 # edited in master.
- dev 中的 1.py 加上 # edited in dev.

当我们想要 merge dev 到 master 的时候:

```
$ git branch #查看当前分支
 dev
* master
-------------------------
$ git merge dev  合并

# 输出
Auto-merging 1.py
CONFLICT (content): Merge conflict in 1.py
Automatic merge failed; fix conflicts and then commit the result.
```
git 发现的我们的 1.py 在 master 和 dev 上的版本是不同的, 所以提示 merge 有冲突. 具体的冲突, git 已经帮我们标记出来, 我们打开 1.py 就能看到:
```
a = 1
# I went back to change 1
<<<<<<< HEAD
# edited in master
=======
# edited in dev
>>>>>>> dev
```
所以我们只要在 1.py 中**手动合并**一下两者的不同就 OK 啦. 然后再 commit 现在的文件, 冲突就解决啦.

```
$ git commit -am "solve conflict"
```

### 临时修改 (stash)
#### 暂存修改
假设我们现在在 dev 分支上快乐地改代码:
```
$ git checkout dev
```
在 dev 中的 1.py 中加上一行 # feel happy, 然后老板的电话来了, 可是我还没有改进完这些代码. 所以我就用 stash 将这些改变暂时放一边.
```
$ git status -s
# 输出
 M 1.py
------------------
$ git stash
# 输出
Saved working directory and index state WIP on dev: f7d2e3a change 3 in dev
HEAD is now at f7d2e3a change 3 in dev
-------------------
$ git status
# 输出
On branch dev
nothing to commit, working directory clean  # 干净得很
```
#### 做其它任务
然后我们建立另一个 branch 用来完成老板的任务:
```
$ git checkout -b boss

# 输出
Switched to a new branch 'boss' # 创建并切换到 boss
```
然后苦逼地完成着老板的任务, 比如添加 # lovely boss 去 1.py. 然后 commit, 完成老板的任务.
```
$ git commit -am "job from boss"
$ git checkout master
$ git merge --no-ff -m "merged boss job" boss
```
#### 恢复暂存
轻松了, 现在可以继续开心的在 dev 上刷代码了.
```
$ git checkout dev
$ git stash list    # 查看在 stash 中的缓存

# 输出
stash@{0}: WIP on dev: f7d2e3a change 3 in dev
```
上面说明在 dev 中, 我们的确有 stash 的工作. 现在可以通过 pop 来提取这个并继续工作了.
```
$ git stash pop

# 输出
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   1.py

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (23332b7edc105a579b09b127336240a45756a91c)
----------------------
$ git status -s
# 输出
 M 1.py     # 和最开始一样了
```

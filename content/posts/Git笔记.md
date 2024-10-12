+++
title = 'git'
date = 2024-02-09T17:44:43+08:00
draft = true
+++

(学习廖俊峰教程的笔记)

## 基础概念

***在.git文件所在目录下进行操作***

三个区域：

1.工作区(Working Directory所处的文件目录)

2.***暂存区(stage)***  (重点)

3.版本库（Repository，工作区有一个隐藏目录 *.git* ，这个不算工作区，而是Git的版本库）:
**创建Git版本库时，Git自动为我们创建了唯一一个master(main)分支，所以git commit就是往master(main)分支上提交更改**

## 基础操作(windows下)

**git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。**

**在powershell或git bash下输入**
*建议git bash-支持很多linux下的命令*

一.初始化一个Git仓库，使用`git init`命令。
二.添加文件到Git仓库，分两步：

 1.使用命令`git add <file>`，注意，可反复多次使用，添加多个工作区文件到暂存区；

 2.使用命令`git commit -m <message>`添加暂存区所有文件到Git仓库，完成。

### 注意：***`git add .`可以一次性添加所有改动.***

## 检查操作

检查 ***工作区+暂存区*** 的状态，使用`git status`命令。

    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            modified:   1.md

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
            modified:   1.md

如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

    $ git diff readme.txt 
    diff --git a/readme.txt b/readme.txt
    index 46d49bf..9247db6 100644
    --- a/readme.txt
    +++ b/readme.txt
    @@ -1,2 +1,2 @@
    -Git is a version control system.
    +Git is a distributed version control system.
     Git is free software.

在Git中，我们用`git log`命令查看历史修改记录：

    $ git log
    commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

    commit e475afc93c209a690c39c13a46716e8fa000c366
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

    commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file

如果嫌出来的记录有点多，加上--pretty=oneline参数

    $ git log --pretty=oneline
    049c19eb21cbe21986810b7de105d3d7ab584d1c (HEAD -> main) 1
    a87bfdefbae52977a72b7955a406eab118a04df5 w
    1dc3373085994a336323614fa726732b582f22f2 1
    049c19eb21cbe21986810b7de105d3d7ab584d1c (HEAD -> main) 1
    a87bfdefbae52977a72b7955a406eab118a04df5 w
    1dc3373085994a336323614fa726732b582f22f2 1
    5b720547ec5708c2d8aef5cfce5d5e48ade4e2fd 一次还原
    fe5a8fe8cfe640934dab93dee4f02ad46da89cdb 提交
    fe4ba1b1cb1daa9e1234655a8c937ddf4c067ee7 第二次

一大串类似049c19eb21cbe...的是commit id（版本号）(SHA1计算出来的一个非常大的数字，用十六进制表示)

在Git中，**用HEAD表示当前版本** ，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），**上一个版本就是HEAD^**， 上上一个版本就是 **HEAD^^** ，当然往上100个版本写100个^比较容易数不过来，所以写成 **HEAD~100**

## 回退操作

### 工作区回退(自己手动修改也可以)

`git checkout -- file`可以丢弃工作区的修改：
`$ git checkout -- readme.txt`

**`--`很重要**

### 暂存区回退

`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区
`git reset HEAD 1.md`

### 版本库回退

`git reset`命令：把当前版本append GPL回退到上一个版本add distributed

    $ git reset --hard HEAD^
    HEAD is now at e475afc add distributed

个人理解：版本回退就是修改HEAD指针的指向(HEAD指向当前所处版本)

回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。

Git提供了一个命令`git reflog`来记录你的 ***每一次命令*** ：

    $ git reflog
    e475afc HEAD@{1}: reset: moving to HEAD^
    1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
    e475afc HEAD@{3}: commit: add distributed
    eaadf4e HEAD@{4}: commit (initial): wrote a readme file

从输出可知，append GPL的commit id是1094adb(每次的commit都对应着一个版本)

## 删除操作

排除git一般情况下：

通常直接在文件管理器中把没用的文件删了，或者用rm命令删了
`$ rm test.txt`

而在git中

    $ git status
    On branch master
    Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    test.txt

    no changes added to commit (use "git add" and/or "git commit -a")

可以看出工作区和版本库不一致

现在你有两个选择：

1.确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

    $ git rm test.txt
    rm 'test.txt'

    $ git commit -m "remove test.txt"
    [master d46f35e] remove test.txt
     1 file changed, 1 deletion(-)
     delete mode 100644 test.txt

现在，文件就从版本库中被删除了。

2.删错了，版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

`$ git checkout -- test.txt`

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远 *不用担心误删* ，但是要小心，**你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容**。

## 远程仓库

我的github `git clone`后本地仓库和远程仓库自动同步了,所以每次直接 本地仓库更新完直接`git push`一下就好了

`$ git push orgin master`

本地仓库与远程仓库同步 使用`git remote`命令

`$ git remote add orgin https://github.com/tyhzxh/tyhzxh.github.io.git`

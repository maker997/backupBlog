---
title: git 常用操作整理之玩转本地仓库
date: 2017-11-12 16:42:20
tags: 
    - Git
---
### 说明
git 这个工具很好用,命令较多,初次学习不是很容易理解上手.理解其原理之后对于各种命令就会轻松驾驭.
之前都是master 分支用到底.从新整理下学习下 git 还是很有必要的

<!---more--->
### 几张原理轻松帮助理解 git
![仓库关系](http://7xtc4k.com1.z0.glb.clouddn.com/仓库关系.png)
![git 分支结构](http://7xtc4k.com1.z0.glb.clouddn.com/git 分支结构.png)
![git 下的文件状态](http://7xtc4k.com1.z0.glb.clouddn.com/git 下的文件状态.png)

### commit和分支的理解
每一个 commit 都是对所有文件的快照,一个分支从物理组成上由工作区和暂存区和版本区构成.分支可以理解为一个对象,它可以继承.
git checkout -b dev 创建了一个叫 dev 的分支,这个分支可以理解为继承自 master 分支(每个仓库在初始化时候默认有一个叫 master 的分支) dev 引用的是 master 分支内容.

### 1.创建用户

```
    git config --global user.name "maker" #设置用户名
    git config --global user.email "806160921@qq.com" #设置用户邮箱
    git config user.name #查看用户名
    git config user.email #查看用户邮箱
```
### 2.创建一个版本库

```
git init #初始化版本库
```
### 3.查看文件状态

```
git status #查看文件状态
```

### 4.加入版本控制中

```
git add fileName #添加文件
git add . #把当前文件夹下所有文件添加到版本控制中
```

### 5.提交到本地仓库中

```
git commit -m "commit1"
git commit -a -m "commit2" #使用在被修改的文件之前被 git管理过
git commit —amend #覆盖上一个commit
```

### 6.查看提交日志

```
git log #查看提交的所有日志
git log --oneline #单行显示日志
git reflog #回滚到之前状态后查看将来的日志
git log --oneline --graph #缩略显示,并展示分支
```

### 7.显示文件的不同

```
git diff #工作区和缓存区的文件区别
git diff --cached #缓存区和最后一次版本区的区别
git diff --cached [版本1] 文件A #文件A在缓存中和版本1的不同
git diff 版本1 版本2 文件A #文件A 在版本1与 版本2的不同
```
### 8.影响工作区的操作

```
 git checkout . #从缓存区或者版本区拉取数据
 git checkout branchA #切换分支A 下的工作区
 git checkout HEAD . #直接从当前版本库到工作区
 git merge branchA #合并分支A
 git pull #等同git fetch + git merge
 git reset --hard 版本号 #切换版本号
```
### 9.回退到制定版本

```
git reset --hard 版本号 #回退到制定版本
git reset --hard HEAD~2 #切换到上上个版本
```
### 10.分支

```
 git branch #查看所有分支
 git branch -v #查看所有分支详情
 git branch dev #创建 dev 分支
 git checkout dev #切换到 dev 分支
 git checkout -b dev #创建 dev 分支并切换到 dev 分支
 git branch -d dev #删除 dev 分支(此时指针不在 dev 分支上才能执行次操作)
 git merge branchA #分支A合并到当前分支上来
 git pull #git fetch + git merge
```





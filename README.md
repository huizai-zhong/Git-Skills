# Git-Skills

Git learning experience.

## 版本控制

### 1. 基本概念

-   **版本库**
    -   典型的客户/服务器系统
        -   版本库是版本控制的核心
        -   可以连接任意数量的客户端
        -   客户端通过写数据库分享代码
-   **两种版本控制系统**
    -   集中式版本控制系统
        -   开发者之间共用一个仓库（repository）
        -   所有操作需要联网
        -   常用的集中式版本控制软件：CVS，SVN(Subversion)
    -   分布式版本控制系统
        -   每个开发者都是一个仓库的完整克隆，每个人都是服务器
        -   支持断网操作
        -   常用的分布式控制系统软件：Git，BitKeeper（收费）

### 2. 分布式版控制 Git

#### 2.1 Git基本概念

-   **Git仓库**
    -   保存所有数据的地方
-   **工作区**
    -   从仓库提取出来的文件，放在磁盘上供你使用或修改
-   **暂存区**
    -   就是一个文件，索引文件，保存了下次将提交的文件列表信息

#### 2.2 Git工作流

![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image002.png)



#### 2.3 基本操作

##### 2.3.1 Git常用指令

|  指令  |                   作用                   |      |
| :----: | :--------------------------------------: | ---- |
| clone  |       将远程服务器的仓库克隆到本地       |      |
| config |               修改git配置                |      |
|  add   |             添加修改到暂存区             |      |
| commit |            提交修改到本地仓库            |      |
|  push  |           提交修改到远程服务器           |      |
|  pull  | 从一个仓库或者本地的分支拉取并且整合代码 |      |
| Status |           查看仓库中的数据状态           |      |

##### 2.3.2 clone

-   本地访问

```shell
# /var/git 为本地仓库
$ git clone file:///var/git
```

-   远程ssh访问

```shell
# /var/git 为服务器上的仓库
$ git clone root@服务器IP:/var/git
```

-   Web

```shell
# 服务器需要额外配置Web服务器
# 客户端可以浏览访问
$ git clone http://服务器IP/git仓库
$ git clone https://服务器IP/git仓库
```

##### 2.3.3 config

```shell
$ git config --global user.email "your_email@example.com"
$ git config --global user.name "your name"
$ cat ~/.gitconfig
```

##### 2.3.4 status

```shell
# 查看状态
$ git status
```

##### 2.3.5 add

```shell
# 提交到暂存区
$ git add . # .代表所有修改
```

##### 2.3.6 commit

```shell
# 提交到本地仓库
$ git commit -m "comment" # option -m 可以在提交的时候添加注释
```

##### 2.3.7 push

```shell
# 推送到远程服务器仓库
$ git push
```

##### 2.3.8 pull

```shell
# 将服务器上的数据更新到本地
$ git pull
```

##### 2.3.9 log,reflog

```shell 
# 查看版本日志 四种方式
$ git log
$ git log pretty=oneline
$ git log --oneline
$ git reflog
```

#### 2.4 图形操作

-   安装Git and tortoiseGit软件即可

#### 2.5 进阶操作

##### 2.5.1 HEAD指针操作

-   HEAD指针是一个可以在任何分支和版本移动的指针，通过移动指针我们可以将数据还原至任何版本。
-   每做一次提交操作都会导致git更新一个版本，HEAD指针也跟着自动移动。

```shell
# 1.查看版本
$ git log --oneline
04ddc0f num.txt:789
7bba57b num.txt:456
301c090 num.txt:123
b427164 new.txt:third
0584949 new.txt:second
ece2dfd new.txt:first line
e1112ac add new.txt
# 2.移动HEAD指针，将数据还原到任意版本
$ git reset --hard 301c090   
$ git reflog
301c090 HEAD@{0}: reset: moving to 301c0
04ddc0f HEAD@{1}: commit: num.txt:789
7bba57b HEAD@{2}: commit: num.txt:456
301c090 HEAD@{3}: commit: num.txt:123
b427164 HEAD@{5}: commit: new.txt:third
0584949 HEAD@{6}: commit: new.txt:second
# 更多操作
# 使用HEAD^将版本回滚一个版本
$ git reset --hard HEAD^
# 使用HEAD~数字，可以将版本回滚n个版本
$ git reset --hard HEAD~2
#
```

##### 2.5.2 Git分支操作

-   基本概念
    -   分支可以让开发分多条主线同时进行，每条主线互不影响

-   分支效果图

![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image005.png)

-   常见的分支规范
    -   MASTER分支（MASTER是主分支，是代码的核心）。
    -   DEVELP分支（DEVELOP最新开发成果的分支）。
    -   RELEASE分支（为发布新产品设置的分支）。
    -   HOTFIX分支（为了修复软件BUG缺陷的分支）。
    -   FEATURE分支（为开发新功能设置的分支）。

-   管理分支

```shell
# 查看当前分支
$ git branch -v
# 创建分支
$ git branch hotfix
$ git branch feature
$ git branch -v #查看
# 切换分支
# 在新的分支上可以继续进行数据操作（增、删、改、查）
$ git checkout hotfix
# 合并分支
# 将hotfix合并到master
# 注意：合并前必须要先切换到master分支，然后再执行merge命令。
$ git checkout master #切换到master
$ git merge hotfix
```

-   解决分支冲突
    -   查看有冲突的文件内容，修改文件为最终版本的数据，解决冲突
-   分支操作流程图
    -   创建分支testing，HEAD指向master
    -   ![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image006.png)
    -   切换分支，HEAD指向testing
    -   ![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image007.png)
    -   在testing分支中修改提交代码
    -   ![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image008.png)
    -   将分支切换至master
    -   ![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image009.png)
    -   在master分支中修改数据，更新版本
    -   ![img](http://tts.tmooc.cn/ttsPage/VIP/NSDVN201904/OPERATION/DAY06/CASE/01/index.files/image010.png)


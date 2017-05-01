---
layout: post
title:  "Git Command"
date:   2016-11-7 22:03:34
category: 架构设计与开源技术
tags: Git
excerpt: 常见的git命令。
---
* content
{:toc}
----------



> 按照使用频率排序，建议记住前十个常用的。



##  **`git clone`**

获取一个url对应的远程Git repo, 创建一个local copy.

```
    git clone [url]   //下载一个项目和它的整个代码历史
```


##  **`git status`**

查询repo的状态.
-s表示short, -s的输出标记会有两列,第一列是对staging区域而言,第二列是对working目录而言.

```
    git status -s       //显示有变更的文件
```


##  **`git add`**

在提交之前,Git有一个暂存区(staging area),可以放入新添加的文件或者加入新的改动.
commit时提交的改动是上一次加入到staging area中的改动,而不是我们disk上的改动.

```
    git add <file>　　  //把单一指定文件加入至暂存区（须加后缀格式）

    //大批量 增、删、改文件：
    git add -A          //暂存区与工作区保持一致（stages All）
    git add .           //暂存区新建文件及更改文件（stages new and modified, without deleted）
    git add -u          //暂存区删除文件及更改文件（stages modified and deleted, without new）
```

##  **`git commit`**

提交已经被add进来的改动.

```
    git commot -m "<message>"　　 //把暂存区的所有修改提交到分支，须输入描述信息
    git commot --amend　　　　    //更改之前一次commit的描述信息
    git commot -a -m "<message>"　//把暂存区的所有被修改或者已删除的且已经被git管理的文档提交提交到分支，须输入描述信息（可省略 git add 过程）
```

##  **`git config`**

Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。。

```
    git config --list              //显示当前的git配置
    git config -e [--global]       //编辑Git配置文件
    git config [--global] user.name "[name]"             // 设置提交代码时的用户名
    git config [--global] user.email "[email address]"   // 设置提交代码时的邮箱地址
```

##  **`git remote`**

远程仓库操作

```
   git remote -v                   //显示所有远程仓库
   git remote show [remote]        //显示某个远程仓库的信息
   git remote add [shortname] [url]      //增加一个新的远程仓库，并命名
```

##  **`git pull`**

git pull会首先执行git fetch,然后执行git merge,把取来的分支的head merge到当前分支.这个merge操作会产生一个新的commit.

pull == fetch + merge FETCH_HEAD

```
   git pull [remote] [branch]      //取回远程仓库的变化，并与本地分支合并
```

##  **`git push`**

push your new branches and data to a remote repository.

```
    git push [remote] [branch]          // 上传本地指定分支到远程仓库
    git push [remote] --force           // 强行推送当前分支到远程仓库，即使有冲突
    git push [remote] --all             // 推送所有分支到远程仓库
```

##  **`git merge`**

把一个分支merge进当前的分支.
如果出现冲突,需要手动修改,可以用git mergetool.
解决冲突的时候可以用到git diff,解决完之后用git add添加,即表示冲突已经被resolved.

```
    git merge [branch]                  // 合并指定分支到当前分支
```

##  **`git init`**

在本地新建一个repo

```
    git init                        // 在当前目录新建一个Git代码库
    git init [project-name]         // 新建一个目录，将其初始化为Git代码库
```

##  **`git tag`**

会在一个提交上建立永久性的书签,通常是发布一个release版本或者ship了什么东西之后加tag.

```
    git tag                     //  列出所有tag
    git tag [tag]               //新建一个tag在当前commit
    git tag [tag] [commit]      //新建一个tag在指定commit
    git tag -d [tag]            //删除本地tag
    git push origin :refs/tags/[tagName]        //  删除远程tag
    git show [tag]              //查看tag信息
```

##  **`git log`**

show commit history of a branch.

```
    git log                        //显示当前分支的版本历史
    git log --stat                 //显示commit历史，以及每次commit发生变更的文件
    git log -S [keyword]           //搜索提交历史，根据关键词

    git log --follow [file]
    git whatchanged [file]          //显示某个文件的版本历史，包括文件改名

    git log -p [file]               //显示指定文件相关的每一次diff
    git log -5 --pretty --oneline   //显示过去5次提交
    git shortlog -sn                //显示所有提交过的用户，按提交次数排序
    git blame [file]                //显示指定文件是什么人在什么时间修改过

    git log [tag] HEAD --pretty=format:%s  //显示某个commit之后的所有变动，每个commit占据一行
    git log [tag] HEAD --grep feature       //显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
```

##  **`git diff`**

show diff of unstaged changes.


```
    git diff                            // 显示暂存区和工作区的差异
    git diff --cached [file]            // 显示暂存区和上一个commit的差异
    git diff HEAD                       //显示工作区与当前分支最新commit之间的差异
    git diff [first-branch]...[second-branch]           //显示两次提交之间的差异
    git diff --shortstat "@{0 day ago}"                 //显示今天你写了多少行代码
```

##  **`git reset`**

undo changes and commits.

这里的HEAD关键字指的是当前分支最末梢最新的一个提交.也就是版本库中该分支上的最新版本.


```
    git reset [file]        //重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
    git reset --hard        //重置暂存区与工作区，与上一次commit保持一致
    git reset [commit]      //重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变

    git reset --hard [commit]   //重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
    git reset --keep [commit]   //重置当前HEAD为指定commit，但保持暂存区和工作区不变
```

##  **`git revert`**

反转撤销提交.

```
   git revert [commit]   //新建一个commit，用来撤销指定commit,后者的所有变化都将被前者抵消，并且应用到当前分支
```

##  **`git rm`**

移除文件


```
    git rm [file1] [file2] ...          // 删除工作区文件，并且将这次删除放入暂存区
    git rm --cached [file]              // 停止追踪指定文件，但该文件会保留在工作区
```

##  **`git mv`**

移动文件

```
    git mv [file-original] [file-renamed]          //改名文件，并且将这个改名放入暂存区
```

##  **`git stash`**

把当前的改动压入一个栈.


```
    git stash list          //会显示这个栈的list.
    git stash apply          //取出stash中的上一个项目(stash@{0}),并且应用于当前的工作目录.

    git stash drop          //删除上一个,也可指定参数删除指定的一个项目.
    git stash clear         // 删除所有项目.
```

##  **`git branch`**

列出分支,创建分支和删除分支.


```
    git branch        // 列出所有本地分支
    git branch -r     // 列出所有远程分支
    git branch -a     //列出所有本地分支和远程分支

    git checkout [branch-name]      //切换到指定分支，并更新工作区

    git branch [branch-name]        //新建一个分支，但依然停留在当前分支
    git checkout -b [branch]        // 新建一个分支，并切换到该分支
    git branch [branch] [commit]    //新建一个分支，指向指定commit

    git branch --track [branch] [remote-branch]         //新建一个分支，与指定的远程分支建立追踪关系
    git branch --set-upstream [branch] [remote-branch]  //建立追踪关系，在现有分支与指定的远程分支之间
```

##  **`git fetch`**

download new branches and data from a remote repository.


```
    git fetch [remote]          // 下载远程仓库的所有变动
```


----------


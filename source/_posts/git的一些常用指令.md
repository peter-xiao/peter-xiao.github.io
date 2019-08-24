---
title: git的一些常用指令
date: 2019-07-31 02:32:22
tags: 
 - git
 - 常用
categories: 实习
---
一些常用的`git`指令
<!--more-->
### 主干流程需要的git指令

进行一个项目的流程很简单，首先在远端仓库`remote`有主干`master`，为了方便我们进行工作，防止频繁的对远端仓库发起请求，首先要拷贝`pull`远端仓库里的项目到本地仓库`respository`。然后创建一个分支`branch`，在工作区`workspace`对本地仓库的文件进行修改，将修改的文件及新加入的文件加入暂存区`index`，然后暂存区传到`commit`本地仓库，最后将本地仓库的分支传到`push`远端仓库进行合并`merge`，经过管理员确认后，远端仓库将发生变化。  
下文涉及缩写，可点击[链接](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)查看

*   从远端仓库拷贝项目到本地仓库，`gcm`，`pl`
*   创建分支，`gcb [branch_name]`
*   将文件加入跟踪&&将跟踪的文件更新，`ga`
*   将暂存区文件提交到本地仓库，`gca`
*   将本地仓库的分支传到远端仓库，`gp [remote_name] [branch_name]`
*   更新本地仓库代码，`gcm`，`gl`

### 项目管理需要的git指令

*   查看项目状态，`git status`
*   从暂存区移除文件，`git -f rm [file_name]`
*   查看提交历史，便于回滚项目，`git log -P(用来显示每次提交内容的差异) -2(用来显示最近两次)`
*   用来查看当前项目的变化，`git diff --cached`
*   撤销上次提交，并补充提交本次，`git commit --amend`

---
title: Git 常用命令
date: 2018-03-30 13:19:50
tags: [git, cmd]
---

#### commit

    $: git add .  // 将所有改动加入缓存区
    $: git add -A // 将所有改动加入缓存区
    $: git add <file/directory> // 将某些文件或目录加入缓存区
    $: git commit -m <note>  // 提交缓存区代码并添加单行说明
    $: git commit -a  // 将缓存区与部分工作区添加到提交，并开始编辑多行说明
    $: [Insert]  // 开始编辑
    $: [:]
    $: wq // 保存并退出编辑
    $: git commit --amend // 追加提交

#### pull/push

    $: git pull // 拉取代码，默认从同名远程分支，但是第一次拉取可能失败
    $: git branch --set-upstream-to=origin/<branch name> <branch name> // 第一次拉去失败后根据提示设置
    $: git pull origin <branch name> // 从远程 origin 版本库，<branch name>分支拉取代码
    $: git pull --rebase // 提交线图有分叉的话，Git 会 rebase 策略来代替默认的 merge 策略
    $: git push // 默认提交到同名远程分支
    $: git push origin <branch name> // 提交到远程 origin 版本库，<branch name>分支
    $: git push origin HEAD:refs/for/<branch name> // Gerrit 提交规则，表示需要code review

#### reset

    $: git reset HEAD // 将 add 内容重新移出仓库
    $: git reset <file/directory> // 将部分文件重新移出仓库
    $: git reset HEAD^ // 将上次 commit 撤销，工作目录不变
    $: git reset HEAD^ --soft // 将上次 commit 撤销，缓存区和工作目录都不会被改变
    $: git reset HEAD^ --mixed // 将上次 commit 撤销，工作目录不变
    $: git reset HEAD^ --hard // 将上次 commit 撤销，缓存区和工作目录都同步到你指定的提交

#### checkout

    $: git checkout . // 将所有未提交到缓存区文件，撤销更改
    $: git checkout <file/directory> // 将某些未提交到缓存区文件，撤销更改
    $: git checkout <branch name> // 切换到某分支
    $: git checkout -b <branch name> // 新建分支并切换过去

#### branch

    $: git branch -a // 查看所有分支
    $: git branch -r // 查看远程分支
    $: git checkout -b <branch name> // 新建分支并跳转
    $: git branch <branch name> // 新建分支

#### log/reflog/gitk

    $: git log // 查看 commit 记录
    $: git reflog // 本地操作记录，可以回退到某个操作，而不是某个 commit
    $: gitk // 弹出GUI界面查看 commit 记录

#### stash

    $: git stash // 修改压栈
    $: git stash save "注释" // 修改压栈并添加注释
    $: git stash list // 修改压栈列表
    $: git stash apply // 修改出栈
    $: git stash apply stash@{2} // 指定修改出栈
    $: git stash drop <ID> // 删除指定 stash，ID：stash@{0}
    $: git stash clear // 删除所有 stash

#### cherry-pick

##### 单次处理

    $: git cherry-pick <commit id> // 单独合并一个提交
    $: git cherry-pick -x <commit id> // 同上，不同点在于保留原提交者信息

##### 区间批量处理

Git 从 1.7.2 版本开始支持批量 cherry-pick，就是一次可以 cherry-pick 一个区间的 commit，&lt;start-commit-id&gt;到&lt;end-commit-id&gt;只需要 commit-id 的前 6 位即可，并且&lt;start-commit-id&gt;在时间上必须早于&lt;end-commit-id&gt;。

把&lt;start-commit-id&gt;到<end-commit-id&gt;之间(左开右闭，不包含 start-commit-id)的提交 cherry-pick 到当前分支：

    $: git cherry-pick <start-commit-id>..<end-commit-id>

把&lt;start-commit-id&gt;到&lt;end-commit-id&gt;之间(闭区间，包含 start-commit-id)的提交：

    $: git cherry-pick <start-commit-id>^..<end-commit-id>

##### 解决冲突

    $: git status // 手动编辑
    $: git add .
    $: git commit
    $: git cherry-pick --continue
    $: git cherry-pick --quit
    $: git cherry-pick --abort

#### 远程分支引用

    $: git fetch -p // 删除本地无效远程引用
    $: git config --global fetch.prune true // 默认不保存本地无效远程分支引用

#### submodule

#### remote

git 操作中，一般使用的`git push origin <branch>`中的`origin`实际上是远程仓库名，该命令的实际意义是`git push <remote name> <branch name>`，实际可以添加多个远程仓库，添加方式：

    $: git remote add <name> git@**.com:**/**.git  // 添加远程仓库，那么为自己起的远程仓库名，比如origin就是常用默认仓库名
    $: git push <name> <branch>  // 将代码提交到某远程仓库的某分支
    $: git remote -v  // 查看本地所有远程仓库
    $: git remote --help  // 查看remote帮助，还有其他有趣命令

[子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

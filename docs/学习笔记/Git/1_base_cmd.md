---
title: Git 常用指令
---

## 基本操作
- 初始化仓库
```bash
git init
```
初始化一个新的 Git 仓库。

- 查看全局配置
```bash
git config --global --list
```

- 配置用户信息
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
设置全局用户名和邮箱。

- 克隆仓库
```bash
git clone <repository_url>
```
克隆远程仓库到本地。

## 暂存
- 添加文件到暂存区
```bash
git add <file>
git add .
```
将文件或所有更改添加到暂存区。

## 提交
- 提交更改
```bash
git commit -m "Commit message"
```
提交暂存区的更改到本地仓库。

```bash
git commit --amend
```
将当前暂存区的更改追加到上一次提交，并允许修改提交信息。

- 查看提交历史
```bash
git log
git log --oneline
```
查看详细或简洁的提交历史。

## 分支管理
- 创建分支
```bash
git branch <branch_name>
```
创建新分支。

- 切换分支
```bash
git checkout <branch_name>
```
切换到指定分支。

- 创建并切换分支
```bash
git checkout -b <branch_name>
```
创建并切换到新分支。

- 合并分支
```bash
git merge <branch_name>
```
将指定分支合并到当前分支。

- 删除分支
```bash
git branch -d <branch_name>
```
删除本地分支。

- Cherry-pick
```bash
git cherry-pick <commit_hash>
```
将指定的提交 <commit_hash> 应用到当前分支。

```bash
git cherry-pick -n <commit_hash>
```
将指定的提交 <commit_hash> 应用到当前分支，但不会自动创建提交。更改会保留在暂存区，允许你在提交前进行修改。

## 远程操作
- 查看远程仓库
```bash
git remote -v
```
查看远程仓库信息。

- 拉取远程更新
```bash
git pull
```
拉取远程仓库的最新更改。

- 推送到远程仓库
```bash
git push origin <branch_name>
```
将本地分支推送到远程仓库。

- 强制推送
```bash
git push origin <branch_name> --force
```
!!! warning "注意"
    强制将本地分支的内容推送到远程分支，覆盖远程分支的历史记录。请谨慎使用，避免影响他人工作。

- 从远程仓库中删除指定的分支
```bash
git push origin --delete <branch_name>
```


## 差异
- 查看差异
```bash
git diff
```
查看工作区与暂存区的差异。

## 恢复
- 恢复工作区文件
```bash
git restore <file>
```
将指定文件恢复到暂存区或最后一次提交的状态。

- 恢复暂存区文件
```bash
git restore --staged <file>
```
将文件从暂存区移除，但保留工作区的更改。

## 撤销与重置
- 撤销指定提交
```bash
git revert <commit_hash>
```
创建一个新的提交，用于撤销指定提交的更改。

- 重置到指定提交
```bash
git reset <commit_hash>
```
将当前分支重置到指定提交，保留工作区更改。

- 硬重置
```bash
git reset --hard <commit_hash>
```
将当前分支重置到指定提交，并丢弃工作区的所有更改。

## 变基
- 变基到指定分支
```bash
git rebase <branch_name>
```
将当前分支的提交变基到指定分支上。


- 交互式变基
```bash
git rebase -i <base_commit>
```
对提交历史进行交互式编辑。
例如进入交互式编辑器中，有内容：
```
pick 89abcde Add feature part 1
pick 1234567 Add feature part 2
pick 63ea85b Add feature part 3
```
修改为如下内容：
```
p 89abcde Add feature part 1
s 1234567 Add feature part 2
s 63ea85b Add feature part 3
```
这表示把第2、3个提交合并到第1个提交

## 常用别名
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
```
设置常用命令的别名。

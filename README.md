# Git

## What Git?
> 代码版本控制系统

专家盲点：expert blind spot
对一个事务知道的越多，就越发不记得不知道这个事的情形

## Git History
- BitKeeper 商业公司
- BitMover

## git 配置
- git config --global user.name "lingyima"
- git confnig --global user.email "67668283@qq.com"
- git confnig --list

## 区域
- working Directory
- State(Index)
- Repository(HEAD)

## 文件三种状态
- modified 	已修改
- staged 	已暂存
- commited 	已提交

## 基本步骤
- git add 资源
- git commit -m "添加描述"
- git status
	+ new file
	+ modified: 资源
		* git checkout -- 资源 (会恢复到修改前)
- git log 查看历史提交记录，按最近顺序
- git reset 
	+ repository -> stage

- add 		<-> checkout
- commit	<->	reset

- git reset HEAD~
	+ ~ 标识上一个版本
	+ 指针移动到之前版本
	+ git status
	+ ~~, ~2
	+ git reset --soft HEAD~
		* 移动HEAD的指向，将其指向上一个快照
		* 但暂存区不变
	+ git reset --hard HEAD~
		* 移动HEAD的指向，将其指向上一个快照
		* 暂存区回滚到当前指针指向的版本
		* 将暂存区的文件还原到工作目录

	+ git reset --mixed, 默认
		* 将快照回滚到暂存区域

- 回滚个别文件
	+ git reset 版本快照 文件名/路径

- 不仅可以往回滚，还可以往前滚
	+ git reset 版本快照的ID号


## git diff
### 比较暂存区与工作目录
- git diff
- --- a/file.py : 暂存区
- +++ a/file.py : 工作目录
- h: 帮助命令

### 比较两个历史快照
- git diff commit_id commit_id

### 当前工作目录和 git 仓库中的快照
- git diff commit_id

### 最新提交的快照与当前工作目录
- git diff HEAD

### 比较暂存区与和 git 仓库快照
- git diff --cached commit_id
- git diff --cached 最新的 仓库快照

## 修改最后一次提交
- `--amend` 选项的commit 提交命令会更正最近的一次提交
- git commit --amend

### 中文乱码
- git commit --amend -m "中文说明"

## 删除文件
- 工作目录中删除文件
- git status

### 恢复文件
- rm -rf 文件名
- git checkout -- 文件名

### 工作目录和暂存区域的文件，取消跟踪，在下次提交时不纳入版本管理
- git rm 文件名
- git status
- git reset --soft HEAD~
- git log
- git status

### 示例
1. vim test.py
	print('test')
2. git add test.py
3. vim test.py
	print('test~')
4. git status
5. git rm test.py
	失败
6. git rm -f test.py 暂存区与工作目录同时删除
6.1 git rm --cacached test.py 仅删除暂存区域的文件

7. git status

## 重命名文件(不识别，git 认为删除旧文件，创建新文件)
- git mv old.py new.py
- git status

	renamed: old.py => new.py

1. ren/mv old.py new.py
2. git rm old.py
3. git add new.py

## 分支：branch

### 创建分支
- git branch 分支名

### 显示分支
- git log --decorate
- head -> master

### 显示分支
- git checkout 分支名
- git log --decorate --oneline
- head -> 分支名
- git add file
- git commit -m ""
- git checkout master 最后 master 快照

- 创建并提交
- git log 仅显示当前分支的快照
- git log --decorate --oneline -- graph --all
	+ decorate: 当前分支引用
	+ oneline: commmit, branch, command
	+ graph: 图形化心啊是
	+ all: 所有分支

### 合并分支

- 开发中的分支
	+ Master
	+ Hotfix: 修复bug
	+ Release: 内部发布
	+ Develop: 开发版本
	+ Feature
	+ Feature

- git log --decorate --all --oneline --graph

- 合并分支(feature分支合并到当前分支)
	+ git merch feature
	+ 冲突
		* git status
		* 修改文件名
		* 内容不一致：修改文件内容
		* git add 修改内容文件
		* git commit -m "fix conflicts"

- git checkout -b feature2
- vim feature2.md
- git add feature2.md
- git commit -m "add feature2.md file"
- git log --decorate --all --oneline --graph
- git checkout master
- git merge feature2
- git log --decorate --all --oneline --graph

### 删除分支
- git branch -d 分支名

- 分支及时指针的引用
- 在分支上创建文件，再合并到主分支就是指针改动。所以速度非常快

### 匿名分支
1.txt,2.txt,3.txt
- git checkout HEAD~
- git log --decorate --all --oneline --graph
- vim 4.txt
- git checkout master

- git log --decorate --all --oneline --graph



git reset --hard feature
reset命令将head指向的分支以及head本身都切换到了feature分支里，原来快照已经消失了

checkout 只切换head 指针
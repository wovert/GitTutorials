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

- modified 已修改
- staged 已暂存
- commited 已提交

## git config 配置

--global 用户全局配置(用户级别)

--system 用户系统配置

--local 用户资源配置文件

## 基本步骤

``` git
$ git add 资源
$ git commit -m "添加描述"
$ git status
  new file
  modified: 资源
$ git checkout -- 资源 (会恢复到修改前)
$ git log 查看历史提交记录，按最近顺序
$ git reset
  repository -> stage

add <-> checkout
commit <-> reset

$ git reset HEAD~
~ 标识上一个版本
指针移动到之前版本
git status
~~, ~2

$ git reset --soft HEAD~
移动HEAD的指向，将其指向上一个快照
但暂存区不变
  
$ git reset --hard HEAD~
移动HEAD的指向，将其指向上一个快照
暂存区回滚到当前指针指向的版本
将暂存区的文件还原到工作目录

$ git reset --mixed, 默认
将快照回滚到暂存区域

回滚个别文件

$ git reset 版本快照 文件名/路径

 不仅可以往回滚，还可以往前滚
$ git reset 版本快照的ID号
```

HEAD 指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。

用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区：

$ git reset HEAD readme.txt

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

$ ssh-keygen -t rsa -C "youremail@example.com"
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

在本地的learngit仓库下运行命令：$ git remote add origin git@github.com:michaelliao/learngit.git

把本地库的所有内容推送到远程库上: $ git push -u origin master

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

用带参数的git log也可以看到分支的合并情况：

``` git
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：

$ git merge --no-ff -m "merge with no-ff" dev

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

合并后，我们用git log看看分支历史：

$ git log --graph --pretty=oneline --abbrev-commit

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用git remote：

$ git remote

git remote -v显示更详细的信息：

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。不信可以用git branch命令看看：

$ git branch
* master
现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

$ git checkout -b dev origin/dev
现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：

$ cat env.txt

$ git add env.txt

$ git commit -m "add new env"

$ git push origin dev

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：

git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

$ git branch --set-upstream-to=origin/dev dev

$ git pull

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

$ git branch

$ git checkout master

敲命令git tag <name>就可以打一个新标签：$ git tag v1.0

可以用命令git tag查看所有标签：

$ git tag

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

$ git tag v0.9 f52c633

注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

$ git show v0.9

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

$ git tag -a v0.1 -m "version 0.1 released" 1094adb
用命令git show <tagname>可以看到说明文字：

如果标签打错了，也可以删除：

$ git tag -d v0.1

Deleted tag 'v0.1' (was f15b0dd)

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令git push origin <tagname>：

$ git push origin v1.0

或者，一次性推送全部尚未推送到远程的本地标签：

$ git push origin --tags

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
然后，从远程删除。删除命令也是push，但是格式如下：

$ git push origin :refs/tags/v0.9

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后st就表示status：

$ git config --global alias.st status

当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch：

$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
以后提交就可以简写成：

$ git ci -m "bala bala bala..."

--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

在撤销修改一节中，我们知道，命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名：

$ git config --global alias.unstage 'reset HEAD'
当你敲入命令：

$ git unstage test.py
实际上Git执行的是：

$ git reset HEAD test.py
配置一个git last，让其显示最后一次提交信息：

$ git config --global alias.last 'log -1'
这样，用git last就能显示最近一次的提交：

$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py
甚至还有人丧心病狂地把lg配置成了：

git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

配置别名
阅读: 188871
有没有经常敲错命令？比如git status？status这个单词真心不好记。

如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后st就表示status：

$ git config --global alias.st status
好了，现在敲git st看看效果。

当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch：

$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
以后提交就可以简写成：

$ git ci -m "bala bala bala..."
--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

在撤销修改一节中，我们知道，命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名：

$ git config --global alias.unstage 'reset HEAD'
当你敲入命令：

$ git unstage test.py
实际上Git执行的是：

$ git reset HEAD test.py
配置一个git last，让其显示最后一次提交信息：

$ git config --global alias.last 'log -1'
这样，用git last就能显示最近一次的提交：

$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py
甚至还有人丧心病狂地把lg配置成了：

git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
来看看git lg的效果：

git-lg

为什么不早点告诉我？别激动，咱不是为了多记几个英文单词嘛！

配置文件
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中：

$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中：

$ cat .gitconfig
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com
配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

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
- git diff --cached 最新的仓库快照

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
- git checkout -- 文件名 (恢复工作区修改且没有暂存的文件)

### 工作目录和暂存区域的文件，取消跟踪，在下次提交时不纳入版本管理

- git rm 文件名
- git status
- git reset --soft HEAD~ （恢复到上一个分支）
- git log
- git status

### 示例

``` git
$ vim test.py
 print('test')
$ git add test.py
$ vim test.py
  print('test~')
$ git status
$ git rm test.py 失败
$ git rm -f test.py 暂存区与工作目录同时删除
$ git rm --cached test.py 仅删除暂存区域的文件
$ git status

// dev 分支上修改文件并切换分支（修改内容不影响当前分支）
$ git stash 临时修改存储
$ git checkout master 切换分支
$ git checkout dev
$ git stash pop 恢复临时修改存储

$ git stash list

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```

## 重命名文件(不识别，git 认为删除旧文件，创建新文件)

- git mv old.py new.py
- git status

	renamed: old.py => new.py

$ ren/mv old.py new.py
$ git rm old.py
$ git add new.py

## 分支：branch

### 创建分支

- git branch 分支名

### 显示分支

- git log --decorate
- head -> master

- git checkout 分支名
- git log --decorate --oneline
- head -> 分支名
- git add file
- git commit -m ""
- git checkout master 最后 master 快照

- 创建并提交
- git log 仅显示当前分支的快照
- git log --decorate --oneline -- graph --all
  - decorate: 当前分支引用
  - oneline: commmit, branch, command
  - graph: 图形化心啊是
  - all: 所有分支

### 合并分支

- 开发中的分支
  - Master
  - Hotfix: 修复bug
  - Release: 内部发布
  - Develop: 开发版本
  - Feature
  - Feature

- git log --decorate --all --oneline --graph

- 合并分支(feature分支合并到当前分支)
  - git merch feature
  - 冲突
    - git status
    - 修改文件名
    - 内容不一致：修改文件内容
    - git add 修改内容文件
    - git commit -m "fix conflicts"

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

- git reset --hard feature

reset 命令将 head 指向的分支以及 head 本身都切换到了feature 分支里，原来快照已经消失了

checkout 只切换 head 指针


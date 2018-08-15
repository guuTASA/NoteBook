# Git !

### 1-创建新仓库

创建新文件夹，打开，然后执行 
`git init`
以创建新的 git 仓库。

### 2-检出仓库

执行如下命令以创建一个本地仓库的克隆版本：
`git clone /path/to/repository` 
如果是远端服务器上的仓库，你的命令会是这个样子：
`git clone username@host:/path/to/repository`

### 3-工作流

你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，它指向你最后一次提交的结果。

### 4-添加和提交

你可以提出更改（把它们添加到暂存区），使用如下命令：
`git add <filename>`
这是 git 基本工作流程的第一步；使用如下命令以实际提交改动：
`git commit -m "代码提交信息"`
现在，你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库。

### 5-推送改动

你的改动现在已经在本地仓库的 **HEAD** 中了。执行如下命令以将这些改动提交到远端仓库：
`git push origin master`

`git push -u origin master`		//-u 为首次

可以把 *master* 换成你想要推送的任何分支。 
如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
`git remote add origin <server>`

`git remote add origin git@server-name:path/repo-name.git `

`git remote add origin git@github.com/guuTASA/NoteBook.git`

`git remote add origin https://github.com/guuTASA/NoteBook.git`

如此你就能够将你的改动推送到所添加的服务器上去了。

`git remote rm origin` 删除



要更新你的本地仓库至最新改动，执行：
`git pull`

如何你想从资源库中删除文件，我们使用rm。

`git rm file`

`touch readme.txt `建立txt文件

`mkdir name` 建立文件夹



git@git.coding.net:sjm519229770/workSpace.git



可以通过如下命令进行代码合并【注：pull=fetch+merge]

git pull --rebase origin master



此时可以查看远程仓库地址：

` $ git remote -v`

```
##将本地test分支推送到远程服务器
giscafer@Faronsince2016 /G/002_project/Comments (master)
$ git push origin test
Username for 'https://git.oschina.net': giscafer
Password for 'https://giscafer@git.oschina.net':
Total 0 (delta 0), reused 0 (delta 0)
To https://git.oschina.net/giscafer/Comments.git
 * [new branch]      test -> test

##切换到test分支
giscafer@Faronsince2016 /G/002_project/Comments (master)
$ git checkout test
Switched to branch 'test'
```

```
##删除本地分支（提示错误是因为当初使用这test分支）
    giscafer@Faronsince2016 /G/002_project/Comments (test)
$ git branch -d test
error: Cannot delete the branch 'test' which you are currently on.

##切换到其他分支
giscafer@Faronsince2016 /G/002_project/Comments (test)
$ git checkout doc
Switched to branch 'doc'
Your branch is up-to-date with 'origin/doc'.
```

clone 某个分支：

   git clone -b  dev5   https://git.coding.net/aiyongbao/tradepc.git      tradepc_zzgdapp

 

clone 所有分支：

　　git   clone  https://git.coding.net/aiyongbao/tradepc.git      tradepc_zzgdapp

​     git　　branch -r

​     git checkout dev5

### 6-分支

创建本地分支 `git branch xxxxx`

查看本地分支`git branch`

查看远程分支 `git branch -a`

git branch -d dev  删除分支

git checkout dev 切换到指定分支

git checkout -b dev 新建并切换到当前分支

git merge dev 合并某分支到当前分支



### 7-git add的三种用法

`git add -A`  提交所有变化

`git add -u ` 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

`git add . ` 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件



### 8-用户名和邮箱地址

查看用户名和邮箱地址：

```
$ git config user.name
$ git config user.email
```

修改用户名和邮箱地址：

```
$ git config --global user.name "username"
$ git config --global user.email "email"
```

> 注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。 

### 9- 将 git add 和 git commit 合并为一步

git commit -am"This is a commit created by xxx" 



### 10- git add 、git commit 、 git push的区别

git add: 把文件推到暂存区（stage）

git commit: 一次性把暂存区的所有修改提交到本地库的分支 

git push: 把本地库的内容推到远程分支



**三棵树**：https://blog.csdn.net/ligang2585116/article/details/51816372

| 树                | 用途                                 |
| ----------------- | ------------------------------------ |
| HEAD              | 上一次提交的快照，下一次提交的父结点 |
| Index             | 预期的下一次提交的快照               |
| Working Directory | 沙盒                                 |

**HEAD**：HEAD是当前分支引用的指针，它总是指向该分支上的最后一次提交。 这表示 HEAD 将是下一次提交的父结点。 通常，理解 HEAD 的最简方式，就是**将它看做你的上一次提交的快照**。

```
# 显示了 HEAD 快照实际的目录列表
$ git cat-file -p HEAD12
```

**Index**：索引是你的“预期的下一次提交”–“暂存区域”，运行git add后，代码就进入“暂存区域”。

```
# 显示出索引当前的样子
$ git ls-files -s12
```

**Working Directory**：可以把工作目录当做“沙盒”。在将修改提交到暂存区并记录到历史之前，可以随意更改。 



### 11- git status、git diff

status：查看修改的状态

diff：查看区别在哪儿



### 12- git log（--pretty=oneline）

查看修改的记录（提交日志）

显示的都是用SHA1加密过的 commit id + commit描述

```s
$ git log --pretty=oneline
29c462ae571d8b9ba4c4b20d8dedb59e261b01b1 (HEAD -> master) test
6ab7c14c78779f4687822774ea4f555cb3596bc5 (origin/master) commit test
326d394601a2305df3b76f842a9971e7aec540c0 python3
75b4b7dfbedce54ba5d713962db176130a618f97 daily update
2677deab80206d63c01fa15d7f60588d8def4f14 fiddler
3ef9eee2edc8b10e133dacac861c5de2099284cd daily update
46c8dcd9e6c686caabb1460622ae6e50606f8089 daily update
f457bedc02d9b663e7bcd5684722046b62ffeffb find some bugs
659d9fa4d8bee6ddfd5ac146c80eadeb89a3a624 daily update
6ddbe6865361f51cb2145c13f3464e6e1de15723 daily update
```

以上，`HEAD->` 的是上一次commit提交上去的那个版本

`origin/master`的是远程分支上边的版本



### 13- git reset --hard HEAD^ 

`git reset --hard commit_id`



### 14- git reflog

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本 











----



### 999-错误及解决方法

#### 01- fatal: remote origin already exists.

解决方法：https://blog.csdn.net/dreamskyforjava/article/details/24322533

> ​    1、先输入 git remote rm origin
>     2、再输入 git remote add origin  https://github.com/(user_name)/(app_name).git 就不会报错了！
>     3、如果输入 git remote rm origin 还是报错的话，error: Could not remove config section 'remote.origin'. 我们需要修             改gitconfig文件的内容
>     4、找到你的github的安装路径，我的是                                       C:\Users\DELL\AppData\Local\GitHub\PortableGit_054f2e797ebafd44a30203088cd3d58663c627ef\etc            
>
> ​    5、找到一个名为gitconfig的文件，打开它把里面的[remote "origin"]那三行删掉就好了！

#### 02- LF will be replaced by CRLF in test.txt. 

https://www.cnblogs.com/horizonli/p/5323363.html

原因：LF是linux下的换行符，而CRLF是enter + 换行

​	当创建文档的时候使用的环境是windows，而git运行环境在linux环境

解决方法：git config --global core.autocrlf false

#### 03- git reset --hard HEAD^  MORE?

https://blog.csdn.net/simpleelegant/article/details/77413391

然后各种百度发现原来在CMD中 ^ 是特殊字符，要使用时必须用双引号包括才行~

```
git reset --hard HEAD"^"
```
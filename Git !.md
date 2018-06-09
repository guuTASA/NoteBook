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
`git add *`
这是 git 基本工作流程的第一步；使用如下命令以实际提交改动：
`git commit -m "代码提交信息"`
现在，你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库。

### 5-推送改动

你的改动现在已经在本地仓库的 **HEAD** 中了。执行如下命令以将这些改动提交到远端仓库：
`git push origin master`
可以把 *master* 换成你想要推送的任何分支。 
如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
`git remote add origin <server>`
如此你就能够将你的改动推送到所添加的服务器上去了。



要更新你的本地仓库至最新改动，执行：
`git pull`

如何你想从资源库中删除文件，我们使用rm。

`git rm file`

touch readme.txt 建立txt文件

mkdir name 建立文件夹



git@git.coding.net:sjm519229770/workSpace.git



可以通过如下命令进行代码合并【注：pull=fetch+merge]

git pull --rebase origin master



##### **4、此时可以查看远程仓库地址：**

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

### 分支

创建本地分支 `git branch xxxxx`

查看本地分支`git branch`

查看远程分支 `git branch -a`
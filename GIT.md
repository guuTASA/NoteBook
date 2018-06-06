# GIT

### 0-命令

git init

git commit -m "修改的说明"

git remote add origin git@server-name:path/repo-name.git 

git push (-u) origin master		//-u 为首次

### 999-错误及解决方法

#### fatal: remote origin already exists.

解决方法：https://blog.csdn.net/dreamskyforjava/article/details/24322533

​    1、先输入 git remote rm origin
    2、再输入 git remote add origin  https://github.com/(user_name)/(app_name).git 就不会报错了！
    3、如果输入 git remote rm origin 还是报错的话，error: Could not remove config section 'remote.origin'. 我们需要修             改gitconfig文件的内容
    4、找到你的github的安装路径，我的是                                       C:\Users\DELL\AppData\Local\GitHub\PortableGit_054f2e797ebafd44a30203088cd3d58663c627ef\etc            

​    5、找到一个名为gitconfig的文件，打开它把里面的[remote "origin"]那三行删掉就好了！


# Question

### 001-关系型数据库/非关系型数据库 的区别？应用场景？

### 002- The file will have its original line endings in your working directory.

Git默认配置替换回车换行成统一的`CRLF`，我们只需要修改配置禁用该功能即可。

Gitshell中输入如下命令解决：

```
git config --global core.autocrlf  false

```

使用`git config --list`命令查看Git所有配置。


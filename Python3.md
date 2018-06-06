# #Python3

### 1-实现print不换行

`print(n , end = '')`

### 2-.py打包成exe文件

安装pyinstaller

`pip install pyinstaller`

安装好之后可以执行pyinstaller 把xxx.py变成xxx.exe

`pyinstaller -F xxx.py`

PS:也是最坑的一点。就是当你使用错误的参数去打包或者打包到一半中断，等等此类运行到一半没了的情况。会导致你原来的py文件变成一个0KB的空文件。里面的代码会全部消失！！！所以以后需要有个良好的习惯，就是复制一份代码出来，用这个副本进行打包。并且参数出错，或者打错了导致失败时，检查下副本文件的py文件是否还存在再继续重新打包，不然打出来的就是空的文件，自然一直闪退，因为压根没内容 
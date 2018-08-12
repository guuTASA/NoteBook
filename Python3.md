# #Python3

### 1-实现print不换行

`print(n , end = '')`

### 2-.py打包成exe文件

安装pyinstaller

`pip install pyinstaller`

安装好之后可以执行pyinstaller 把xxx.py变成xxx.exe

`pyinstaller -F xxx.py`

PS:也是最坑的一点。就是当你使用错误的参数去打包或者打包到一半中断，等等此类运行到一半没了的情况。会导致你原来的py文件变成一个0KB的空文件。里面的代码会全部消失！！！所以以后需要有个良好的习惯，就是复制一份代码出来，用这个副本进行打包。并且参数出错，或者打错了导致失败时，检查下副本文件的py文件是否还存在再继续重新打包，不然打出来的就是空的文件，自然一直闪退，因为压根没内容 



### 3- 打印List的内容

来自：http://www.runoob.com/python3/python3-data-type.html

（从底部评论区截取）

- python中的字典是使用了一个称为散列表（hashtable）的算法（不具体展开），

  其特点就是：不管字典中有多少项，in操作符花费的时间都差不多。

  如果把一个字典对象作为for的迭代对象，那么这个操作将会遍历字典的**键**：

```
def example(d):
    # d 是一个字典对象
    for c in d:
        print(c)
        #如果调用函数试试的话，会发现函数会将d的所有键打印出来;
        #也就是遍历的是d的键，而不是值.
```

- 针对楼上的 字典 拓展，做测试的时候，想要**输出 kye:value的组合**发现可以这样：

```
for c in dict:
print(c,':',dict[c])
```

或者

```
for c in dict:
print(c,end=':');
print(dict[c])
```

于是发现 print()函数 其实可以 添加多个参数，用逗号 隔开。

本来想要用

```
for c in dict:
print(c+':');
print(dict[c])
```

  这样的方式打印 key：value结果发现其实 key不一定是 string类型，所以 **用+ 号会出问题**。

- 输入 dict 的键值对，可直接用 `items()` 函数：

```
dict1 = {'abc':1,"cde":2,"d":4,"c":567,"d":"key1"}
for k,v in dict1.items():
    print(k,":",v)
```


### 4- yield的使用

http://www.runoob.com/w3cnote/python-yield-used-analysis.html
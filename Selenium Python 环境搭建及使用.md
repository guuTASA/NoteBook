# Selenium Python 



## 1-环境搭建

参考：https://blog.csdn.net/dave_haijie/article/details/54893229

### 1.1-安装python、pip、selenium

我在这里安装的是python3.6.5。

python这个版本自带了pip，我把pip更新到了10.0.1。然后使用`pip install selenium`即可正常安装selenium。没出什么问题。



### 1.2-安装geckodriver、chromedriver

**geckodriver** : https://github.com/mozilla/geckodriver/releases (下载win64位的)

+FireBug+FirePath

**chromedriver** : https://npm.taobao.org/mirrors/chromedriver/2.38/ （下载32位的即可，没有64位的）

**（备选）IEDriverServer** : <http://selenium-release.storage.googleapis.com/index.html> 



下载并解压好后将文件放在`\Python36\`目录下



### 1.3-DEMO

**webTest.py**

```python
#DEMO 1.3

from selenium import webdriver
from time import sleep

driver = webdriver.Chrome()
driver.get("https://www.baidu.com")
driver.find_element_by_id("kw").send_keys("kitten")
driver.find_element_by_id("su").click()
sleep(10)
driver.quit()
```

打开百度，搜索kitten，关闭窗口。



## 0-笔记

### 0.1-关闭标签页

1、关闭浏览器全部标签页

​	 `driver.quit()`

2、关闭当前标签页（从标签页A打开新的标签页B，关闭标签页A） 

​	 `driver.close()`

3、关闭当前标签页（从标签页A打开新的标签页B，关闭标签页B）

​	可利用浏览器自带的快捷方式对打开的标签进行关闭

### 0.2-常用的查找元素的方法

find_element_by_name 

find_element_by_id 

find_element_by_xpath 

find_element_by_link_text 

find_element_by_partial_link_text 

find_element_by_tag_name find_element_by_class_name 

find_element_by_css_selector 
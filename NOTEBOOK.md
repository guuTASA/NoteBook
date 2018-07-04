

#NOTEBOOK

##[1] MINST手写识别

**有监督的学习任务**


```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

minst = input_data.read_data_sets("MNIST_data1/",one_hot = True)
#导入数据

x = tf.placeholder(tf.float32, [None, 784])
W = tf.Variable(tf.zeros([784, 10]))
b = tf.Variable(tf.zeros([10]))
y = tf.nn.softmax(tf.matmul(x, W) + b)
y_ = tf.placeholder(tf.float32, [None, 10])

#数据的初始化 y = Wx + b , y_是真实的结果


cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices = [1]))
#按行

train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
#(0.01)

init = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init)
#sess.run(tf.initialize_all_variables())
#初始化

for i in range(1000):
	batch_xs,batch_ys = minst.train.next_batch(64)
	sess.run(train_step, feed_dict = {x: batch_xs, y_:batch_ys})


correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_, 1))
#判断是否相等
accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
#这里返回一个布尔数组。为了计算我们分类的准确率，我们将布尔值转换为浮点数来代表对、错，然后取平均值。

print(sess.run(accuracy, feed_dict = {x: minst.test.images, y_:minst.test.labels}))

```
**以上代码准确率在0.91左右**


##### 2017.10.30

```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets("MNIST_data1/",one_hot = True)


sess = tf.InteractiveSession()

x = tf.placeholder("float", shape = [None, 784])
y_ = tf.placeholder("float", shape = [None, 10])


#定义两个函数用于初始化

#1 权重初始化函数
def weight_variable(shape):
	#输出服从截尾正态分布的随机值
	initial = tf.truncated_normal(shape, stddev = 0.1)
	return tf.Variable(initial)

#2偏置初始化函数
def bias_variable(shape):
	initial = tf.constant(0.1, shape = shape)
	return tf.Variable(initial)

#我们的卷积使用1步长（stride size），0边距（padding size）的模板
#保证输出和输入是同一个大小
#我们的池化用简单传统的2x2大小的模板做max pooling。
#为了代码更简洁，我们把这部分抽象成一个函数。

#shape为[batch,height,width,channels]
#填充类型为SAME,可以不丢弃任何像素点

#1 卷积
def conv2d(x, W):
	return tf.nn.conv2d(x, W, strides = [1, 1, 1, 1], padding = 'SAME')

#2 池化
#采用最大池化，也就是取窗口中的最大值作为结果
#ksize表示pool窗口大小为2x2,也就是高2，宽2
#strides，表示在height和width维度上的步长都为2
def max_pool_2x2(x):
	return tf.nn.max_pool(x, ksize = [1, 2, 2, 1], strides = [1, 2, 2, 1], padding = 'SAME')


#第一层卷积

#它由一个卷积接一个max pooling完成。卷积在每个5x5的patch中算出
#32个特征。卷积的权重张量形状是[5, 5, 1, 32]，前两个维度是patch
#的大小，接着是输入的通道数目，最后是输出的通道数目。 而对于每
#一个输出通道都有一个对应的偏置量。

W_conv1 = weight_variable([5, 5, 1, 32])
b_conv1 = bias_variable([32])

#为了用这一层，我们把x变成一个4d向量，其第2、第3维对应图片的
#宽、高，最后一维代表图片的颜色通道数。

#-1表示自动推测这个维度的size
x_image = tf.reshape(x, [-1, 28, 28, 1])

#我们把x_image和权值向量进行卷积，加上偏置项，然后应用ReLU激活
#函数，最后进行max pooling。

##h_pool1的输出即为第一层网络输出，shape为[batch,14,14,1]
h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1) + b_conv1)
h_pool1 = max_pool_2x2(h_conv1)

#第二层卷积

W_conv2 = weight_variable([5, 5, 32, 64])
b_conv2 = bias_variable([64])

##h_pool2即为第二层网络输出，shape为[batch,7,7,1]
h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2)
h_pool2 = max_pool_2x2(h_conv2)

#密集连接层/全连接层
#现在，图片尺寸减小到7x7
#我们加入一个有1024个神经元的全连接层，用于处理整个图片

#7*7是h_pool2输出的size，64是第2层输出神经元个数
W_fc1 = weight_variable([7 * 7 * 64, 1024])
b_fc1 = bias_variable([1024])

h_pool2_flat = tf.reshape(h_pool2, [-1, 7 * 7 * 64])
h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)


#Dropout 减少过拟合

keep_prob = tf.placeholder("float")
h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)

W_fc2 = weight_variable([1024, 10])
b_fc2 = bias_variable([10])

y_conv = tf.nn.softmax(tf.matmul(h_fc1_drop, W_fc2) + b_fc2)


#训练和评估模型

cross_entorpy = -tf.reduce_sum(y_ * tf.log(y_conv))
#使用ADAM优化器来做梯度下降。学习率为0.0001
train_step = tf.train.AdamOptimizer(1e-4).minimize(cross_entorpy)
#tf.equal返回的是布尔值
correct_prediction = tf.equal(tf.argmax(y_conv, 1), tf.argmax(y_, 1))
#使用tf.cast把布尔值转换成浮点数，然后用tf.reduce_mean求平均值
accuracy = tf.reduce_mean(tf.cast(correct_prediction, "float"))
sess.run(tf.initialize_all_variables())

#开始训练模型，循环20000次，每次随机从训练集中抓取50幅图像
for i in range(20000):
	batch = mnist.train.next_batch(50)
	if i % 100 == 0:
		#每100次输出一次日志
		train_accuracy = accuracy.eval(feed_dict = {x: batch[0], y_: batch[1], keep_prob: 1.0})
		print("step %d, training accuracy %g" % (i, train_accuracy))
	train_step.run(feed_dict = {x: batch[0], y_: batch[1], keep_prob: 0.5})
print("test accuracy %g" % accuracy.eval(feed_dict = {x: mnist.test.images, y_:mnist.test.labels, keep_prob: 1.0}))

```

**以上代码准确率在0.992左右**




### 1-最好自行下载数据集 放在MINST_data里 使用python自动下载的可能无法连接 导致报错
下载下来的数据集被分成两部分：60000行的训练数据集（mnist.train）和10000行的测试数据集（mnist.test）。这样的切分很重要，在机器学习模型设计时必须有一个单独的测试数据集不用于训练而是用来评估这个模型的性能，从而更加容易把设计的模型推广到其他数据集上（泛化）。

正如前面提到的一样，每一个MNIST数据单元有两部分组成：一张包含手写数字的图片和一个对应的标签。我们把这些图片设为“xs”，把这些标签设为“ys”。训练数据集和测试数据集都包含xs和ys，比如训练数据集的图片是 mnist.train.images [60000, 784] 张量，训练数据集的标签是 mnist.train.labels。 [60000, 10] 数字矩阵。

### 2-展平图片的数字数组会丢失图片的二维结构信息。
这显然是不理想的，最优秀的计算机视觉方法会挖掘并利用这些结构信息，我们会在后续教程中介绍。但是在这个教程中我们忽略这些结构，所介绍的简单数学模型，softmax回归(softmax regression)，不会利用这些结构信息。

##### 2017.11.02

### 3-Tensorflow的图描述
Tensorflow不单独地运行单一的复杂计算，而是让我们可以先用图描述一系列可交互的计算操作，然后全部一起在Python之外运行。（这样类似的运行方式，可以在不少的机器学习库中看到。）

##### 2017.11.06

### 4-Session/InteractiveSession
Tensorflow依赖于一个高效的C++后端来进行计算。与后端的这个连接叫做session。一般而言，使用TensorFlow程序的流程是先创建一个图，然后在session中启动它。

这里，我们使用更加方便的InteractiveSession类。通过它，你可以更加灵活地构建你的代码。它能让你在运行图的时候，插入一些计算图，这些计算图是由某些操作(operations)构成的。这对于工作在交互式环境中的人们来说非常便利，比如使用IPython。如果你没有使用InteractiveSession，那么你需要在启动session之前构建整个计算图，然后启动该计算图。

### 5-Placeholder（占位符）
```python
x = tf.placeholder("float", shape=[None, 784])
y_ = tf.placeholder("float", shape=[None, 10])
```
这里的x和y并不是特定的值，相反，他们都只是一个占位符，可以在TensorFlow运行某一计算时根据该占位符输入具体的值。

### 6-one-hot（独热码）
独热码，在英文文献中称做 one-hot code, 直观来说就是有多少个状态就有多少比特，而且只有一个比特为1，其他全为0的一种码制。

例如，有6个状态的独热码状态编码为：000001，000010，000100，001000，010000，100000。

### 7-Variable（变量）
```python
W = tf.Variable(tf.zeros([784,10]))
b = tf.Variable(tf.zeros([10]))
```
一个变量代表着TensorFlow计算图中的一个值，能够在计算过程中使用，甚至进行修改。在机器学习的应用过程中，模型参数一般用Variable来表示。
```python
sess.run(tf.initialize_all_variables())
```
变量需要通过seesion初始化后，才能在session中使用。这一初始化步骤为，为初始值指定具体值（本例当中是全为零），并将其分配给每个变量,可以一次性为所有变量完成此操作。

### 8-交叉熵
![Alt text](./mnist10.png)
交叉熵可在神经网络(机器学习)中作为损失函数，p表示真实标记的分布，q则为训练后的模型的预测标记分布（非真实分布），交叉熵损失函数可以衡量p与q的相似性。交叉熵作为损失函数还有一个好处是使用sigmoid函数在梯度下降时能避免均方误差损失函数学习速率降低的问题，因为学习速率可以被输出的误差所控制。
因此，交叉熵越低，这个策略就越好，最低的交叉熵也就是使用了真实分布所计算出来的信息熵，因为此时  ，交叉熵 = 信息熵。这也是为什么在机器学习中的分类算法中，我们总是最小化交叉熵，因为交叉熵越低，就证明由算法所产生的策略最接近最优策略，也间接证明我们算法所算出的非真实分布越接近真实分布。

参考：

《数学之美》吴军

Information entropy

作者：CyberRep
链接：https://www.zhihu.com/question/41252833/answer/195901726
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 9-Dropout
为了减少过拟合，我们在输出层之前加入dropout。我们用一个placeholder来代表一个神经元的输出在dropout中保持不变的概率。这样我们可以在训练过程中启用dropout，在测试过程中关闭dropout。 TensorFlow的tf.nn.dropout操作除了可以屏蔽神经元的输出外，还会自动处理神经元输出值的scale。所以用dropout的时候可以不用考虑scale。

### 10-全连接层
全连接层（fully connected layers，FC）在整个卷积神经网络中起到“分类器”的作用。如果说卷积层、池化层和激活函数层等操作是将原始数据映射到隐层特征空间的话，全连接层则起到将学到的“分布式特征表示”映射到样本标记空间的作用。

### 11-池化
池化作用于图像中不重合的区域（这与卷积操作不同）
平均池化/最大池化/随机池化
缩小图片尺寸但基本不丢失信息

通常情况下，我们使用的都是2x2的最大池，就是在2x2的范围内，取最大值。因为最大池化（max-pooling）保留了每一个小块内的最大值，所以它相当于保留了这一块最佳的匹配结果（因为值越接近1表示匹配越好）。这也就意味着它不会具体关注窗口内到底是哪一个地方匹配了，而只关注是不是有某个地方匹配上了。这也就能够看出，CNN能够发现图像中是否具有某种特征，而不用在意到底在哪里具有这种特征。这也就能够帮助解决之前提到的计算机逐一像素匹配的死板做法。

作者：地球的外星人君
链接：https://www.zhihu.com/question/49909565/answer/207609620
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

###12-卷积
Relu是常用的激活函数，所做的工作就是max(0,x)，就是输入大于零，原样输出，小于零输出零，这里就不展开了。


###//Q&A
用什么方法识别? 深度学习?
传统BP和cnn都对自己的手写数字识别不好，不知道为什么

1) CNN的话,有没有加个dropout层,有dropout层应该会增加一定的泛化能力
2) 图像有没有留一定像素的白边, 我测试的时候发现留一定的白边会提高一些识别率
3) Mnist的是外国人的手写体,可能你测试的是中国人的 有些不一样,导致识别率不高,这个只能从增加样本入手





##[2] Python3

print('hello python')

input 等待用户输入

Python可以在同一行中使用多条语句，语句之间使用分号(;)分割
print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 end=""

type()不会认为子类是一种父类类型。
isinstance()会认为子类是一种父类类型。

在 Python2 中是没有布尔型的，它用数字 0 表示 False，用 1 表示 True。到 Python3  中，把 True 和 False 定义成关键字了，但它们的值还是 1 和 0，它们可以和数字相加。

数值的除法（/）总是返回一个浮点数，要获取整数使用//操作符。
在混合计算时，Python会把整型转换成为浮点数。
```python
print (str)          # 输出字符串
print (str[0:-1])    # 输出第一个到倒数第二个的所有字符
print (str[0])       # 输出字符串第一个字符
print (str[2:5])     # 输出从第三个开始到第五个的字符
print (str[2:])      # 输出从第三个开始的后的所有字符
print (str * 2)      # 输出字符串两次
print (str + "TEST") # 连接字符串
```

Python 使用反斜杠(\)转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 r，表示原始字符串：

```python
>>> print('Ru\noob')
Ru
oob
>>> print(r'Ru\noob')
Ru\noob
>>> 
```
### 列表
###元组
Python中的字符串不能改变。

Python 的元组与列表类似，不同之处在于元组的元素不能修改。
元组使用小括号，列表使用方括号。
元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可。
元组中只包含一个元素时，需要在元素后面添加逗号

tuple(seq)
将列表转换为元组。
### 字典


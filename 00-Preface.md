### 简介
本笔记使用Jupyter Notebook进行交互式学习。

SciPy是一个开源的数学软件集合。SciPy中包括NumPy，它提供数组对象、线性代数、随机数等。Matplotlib提供NumPy数组的打印。SciPy的模块复制了NumPy中的一些功能，同时添加了优化、图像处理等功能。

### 代码细节
numpy.array实现一个或多个维度的数组。它的类型是numpy.ndarray，我们简称它为ndarray。

import numpy as np

x = np.array([[1., 2.],
              [3., 4.]])
              
用作下标的冒号（：）是该行或列中所有数据的简写。

与Python列表一样，可以使用负索引来引用数组的末尾-1表示最后一个索引。

#### 矩阵
+元素相加

*元素相乘

@矩阵乘法 i.e. x@x 或者 np.dot(x, x) 或者 x.dot(x)

转置x.T

逆矩阵  np.linalg.inv(x) 或者 linalg.inv(x)

零矩阵 np.zeros(7) 或者 np.zeros((3, 2))

单位矩阵 np.eye(3)

等间距数据 np.arange(0, 2, 0.1) (开始, 结束, 步长) 或者 np.linspace(0, 2, 20) (开始, 结束, 总数)

import matplotlib.pyplot as plt

#### 绘图 
plt.plot([1, 4, 2, 5, 3, 6])

输出[<matplotlib.lines.Line2D at 0x2ba160bed68>]是因为plt.plot返回刚创建的对象。

通常我们不想看到这种情况，所以加一个；到最后一个绘图命令，以抑制输出。

默认情况下，绘图假设x序列递增一。可以通过传递x和y来提供自己的x级数。

plt.plot(np.arange(0,1, 0.1), [1,4,3,2,6,4,7,3,4,5]);

创建10个元素均为0.1的NumPy array：

np.ones(10) / 10

np.array([0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1])

np.zeros(10)+0.1

np.linspace(0.1, 0.1, 10) 

print(np.array([0.1] * 10))

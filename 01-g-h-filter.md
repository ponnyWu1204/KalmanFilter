### 通过思想实验建立直觉感受

想象一下，我们生活在一个没有秤的世界里。某一天，一个同事跑过来向你宣布她发明了一种“秤”。在她解释之后，你急切地站在上面，然后得到结果：“172磅”。你非常兴奋，因为在你的一生中，这是你第一次知道自己的体重。更重要的是，当你想象着将这个设备卖给世界各地的减肥诊所时，你将大赚一笔！

另一个同事听到喧闹声，过来凑凑热闹。你解释了这个发明，然后再次踏上秤，自豪地宣布结果：“161磅”。然后你犹豫了，感到困惑。

“几秒钟前它显示的是172磅，”你向同事抱怨。

“我从未说过它是准确的，”她回答道。

传感器是不准确的。这是过滤工作中一个巨大的动机。过去半个世纪发展起来的解决方案是通过提出一些非常基本、根本的问题来发展的，这些问题涉及到我们所知道的事物的本质以及我们如何知道这些事物。在我们尝试数学模型之前，让我们跟随那场发现之旅，看看它是否能增进我们对过滤的直觉感受。


### g-h过滤器

> **`g`是从预测值和测量值得到估计值的缩放因子，`h`是从预测值和测量值更新预测值导数的缩放因子**

```python
import numpy as np
from numpy.random import randn
import matplotlib.pylab as pylab

def g_h_filter(data, x0, dx, g, h, dt=1.):
    x_est = x0
    results = []
    for z in data:
        # prediction step
        x_pred = x_est + (dx*dt)
        dx = dx

        # update step
        residual = z - x_pred
        dx = dx + h * (residual) / dt
        x_est = x_pred + g * residual
        results.append(x_est)
    return np.array(results)

def gen_data(x0, dx, count, noise_factor=0., accel=0.):
    return [x0 + dx*i + accel * (i**2) /2 + randn()*noise_factor for i in range(count)]

predictions = []
zs = gen_data(x0=10., dx=1., count=20, noise_factor=0, accel=1.)
data = g_h_filter(data=zs, x0=10., dx=1., g=0.2, h=0.02, dt=10)
data_bar = g_h_filter(data=zs, x0=10., dx=2., g=0.2, h=0.02, dt=5)
plot_g_h_results(measurements=zs, filtered_data=data)

print (f"data：{data}")
print ("data_bar：", data_bar) 
```

从数学角度上：

$$\mathbf{x} = \begin{bmatrix}x \\ \dot{x}\end{bmatrix}$$

$$\mathbf{x_i} = \bar{\mathbf{x_{i-1}}}$$



### 通过思想实验建立直觉感受

想象一下，我们生活在一个没有秤的世界里。某一天，一个同事跑过来向你宣布她发明了一种“秤”。在她解释之后，你急切地站在上面，然后得到结果：“172磅”。你非常兴奋，因为在你的一生中，这是你第一次知道自己的体重。更重要的是，当你想象着将这个设备卖给世界各地的减肥诊所时，你将大赚一笔！

另一个同事听到喧闹声，过来凑凑热闹。你解释了这个发明，然后再次踏上秤，自豪地宣布结果：“161磅”。然后你犹豫了，感到困惑。

“几秒钟前它显示的是172磅，”你向同事抱怨。

“我从未说过它是准确的，”她回答道。

传感器是不准确的。这是过滤工作中一个巨大的动机。过去半个世纪发展起来的解决方案是通过提出一些非常基本、根本的问题来发展的，这些问题涉及到我们所知道的事物的本质以及我们如何知道这些事物。在我们尝试数学模型之前，让我们跟随那场发现之旅，看看它是否能增进我们对过滤的直觉感受。


### g-h过滤器

> **`g`是从预测值和测量值得到估计值的滤波因子，`h`是从预测值和测量值更新预测值导数的滤波因子**

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

```math
\begin{aligned}
\mathbf{x} &= \begin{bmatrix}x \\ \dot{x}\end{bmatrix} \\

x_i &= \hat{x_{i-1}} + \dot{x}dt \\

\Delta &= z_i -x_i \\

\dot{x} &= \dot{x} + h\cdot\frac{\Delta}{dt} \\

\hat{x_i} &= x_i +g\cdot\Delta 
\end{aligned}
```

$dxdt$相等的时候，过滤器结果相同。

由于加速度的存在，每个预测都落后于信号。无法通过调整速度或加速度来纠正这个问题，这被称为滞后误差或系统的系统性误差。

当滤波因子`g`较大时，我们更密切地跟随测量值，适合噪声小、预测不准。

当滤波因子`g`较小时，我们更密切地跟随预测值，适合噪声大、预测准确。

`h`影响`\dot{x}`更新时对预测值和测量值的重视程度。

`g`和`h`不应该设置过小，否则可能会在测试集中表现良好但是在真实环境中表现很差。

过大的`g`会导致过滤器曲线不平滑。

过大的`h`会导致过滤器对测量噪声过于敏感。

滤波器是被设计的，而不是被临时选择的。

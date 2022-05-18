---
title: ML-程序练习-Dragon
tags:
  - ML
  - 练习
abbrlink: f7d4c741
date: 2021-10-27 17:28:22
---

# 回归问题

## 前期

假设已有某样例，参数为w=1.477, b=0.089，即为$y=1.477x+0.089$

## 过程分析

### 数据采样

首先我们需要模拟一些带有真实样本观测误差的数据（因为真实情况是真实模型我们已经知道了），所以我们在这里给模型添加误差自变量$\epsilon$，其采样自均值为0，标准差为0.01的高斯分布：
$$
y=1.477x+0.089+\epsilon,\epsilon\sim N(0, {0.01}^2)
$$
通过随机采样100次获得训练数据集

```python
data = []
for i in range(100):
    x=np.random.uniform(-10., 10.)
    eps=np.random.normal(0., 0.01)
    y=1.477*x+0.089+eps
    data.append([x,y])
data=np.array(data)
```

### MSE

计算每个点的预测值与真实值之间差的平方并累加，获得均方误差损失值

```python
def mse(b, w, points): # 误差计算
    totalError=0
    for i in range(0, len(points)):#循环迭代
        x=points[i, 0]
        y=points[i, 1]
        totalError+=(y-(w*x+b))**2
    return totalError/float(len(points))# 求均方差
```

最后的误差和除以样本总数得到平均误差

### 梯度计算

这里首先需要推导一下梯度的表达式
$$
\frac{\alpha L}{\alpha w}=\frac{\alpha \frac{1}{n}\sum_{i=1}^{n}{(wx^{(i)}+b-y^{(i)})}^2}{\alpha w}=\frac{1}{n}\sum_{i=1}^{n}\frac{\alpha{(wx^{(i)}+b-y{(i)})}^2}{\alpha w}
$$
所以可以得到
$$
\frac{\alpha L}{\alpha w}=\frac{2}{n}\sum_{i=1}^n(wx^{(i)}+b-y^{(i)}·x^{(i)})
$$
同理，可以推导得到
$$
\frac{\alpha L}{\alpha b}=\frac{1}{n}\sum_{i=1}^n2(wx^{(i)}+b-y^{(i)})·1
$$
所以，我们只需要计算出上述两个值，平均以后即可得到偏导数

```python
def stepdown_gradient(b_current, w_current, points, lr):
    b_gradient=0
    w_gradient=0
    M=float(len(points))#样本总数
    for i in range(0, len(points)):
        x=points[i, 0]
        y=points[i, 1]
        b_gradient+=(2/M)*((w_current * x + b_current) - y)
        w_gradient+=(2/M)*x*((w_current*x + b_current)-y)
    new_b=b_current-(lr*b_gradient)
    new_w=w_current-(lr*w_gradient)
    return [new_b,new_w]
```

### 梯度更新

我们可以根据计算出的误差函数在w和b处的梯度后，根据$x^`=x-\eta\triangledown f$更新w和b的值。对数据集的所有样本训练一次称为一个Epoch，共循环迭代num_iterations个Epoch

```python
def gradient_descent(points, starting_b, starting_w, lr, num_iterations):
    b=starting_b
    w=starting_w

    for step in range(num_iterations):
        b, w=stepdown_gradient(b, w, np.array(points), lr)
        loss=mse(b, w, points)
        if step%50==0:
            print(f"iteration:{step}, loss:{loss}, w:{w}, b:{b}")
    return [b, w]
```

## 完整程序

```python
def mse(b, w, points): # 误差计算
    totalError=0
    for i in range(0, len(points)):#循环迭代
        x=points[i, 0]
        y=points[i, 1]
        totalError+=(y-(w*x+b))**2
    return totalError/float(len(points))# 求均方差
def stepdown_gradient(b_current, w_current, points, lr):
    b_gradient=0
    w_gradient=0
    M=float(len(points))#样本总数
    for i in range(0, len(points)):
        x=points[i, 0]
        y=points[i, 1]
        b_gradient+=(2/M)*((w_current * x + b_current) - y)
        w_gradient+=(2/M)*x*((w_current*x + b_current)-y)
    new_b=b_current-(lr*b_gradient)
    new_w=w_current-(lr*w_gradient)
    return [new_b,new_w]
def gradient_descent(points, starting_b, starting_w, lr, num_iterations):
    b=starting_b
    w=starting_w

    for step in range(num_iterations):
        b, w=stepdown_gradient(b, w, np.array(points), lr)
        loss=mse(b, w, points)
        if step%50==0:
            print(f"iteration:{step}, loss:{loss}, w:{w}, b:{b}")
    return [b, w]


data = []
for i in range(100):
    x=np.random.uniform(-10., 10.)
    eps=np.random.normal(0., 0.01)
    y=1.477*x+0.089+eps
    data.append([x,y])
data=np.array(data)
lr=0.01
initial_b=0
initial_w=0
num_iterations=1000
[b, w]=gradient_descent(data, initial_b, initial_w, lr, num_iterations)
loss=mse(b, w, data)
print(f'Fina loss:{loss}, w:{w}, b:{b}')
```

运行结果：

```
iteration:0, loss:6.162441874953508, w:1.0617677882731775, b:-0.014516689518537094
iteration:50, loss:0.0017523594804364892, w:1.4762089816223927, b:0.04897703734558919
iteration:100, loss:0.00033386053656463924, w:1.4766149652009066, b:0.0747027092487452
iteration:150, loss:0.00014236473287524616, w:1.4767641324874572, b:0.08415488632085935
iteration:200, loss:0.00011651300912947552, w:1.476818939825868, b:0.08762782384952462
iteration:250, loss:0.0001130230547401269, w:1.476839077246164, b:0.08890385740094789
iteration:300, loss:0.0001125519147202384, w:1.476846476176817, b:0.08937270016328336
iteration:350, loss:0.00011248831133360896, w:1.4768491947064997, b:0.08954496329503976
iteration:400, loss:0.00011247972494608194, w:1.4768501935540335, b:0.08960825655441457
iteration:450, loss:0.00011247856579317109, w:1.476850560552563, b:0.08963151188851563
iteration:500, loss:0.00011247840930879765, w:1.476850695395886, b:0.08964005640920385
iteration:550, loss:0.00011247838818357833, w:1.4768507449402855, b:0.08964319585383505
iteration:600, loss:0.00011247838533169705, w:1.4768507631439864, b:0.0896443493547688
iteration:650, loss:0.00011247838494669628, w:1.476850769832426, b:0.0896447731763553
iteration:700, loss:0.00011247838489472176, w:1.4768507722899058, b:0.08964492889771788
iteration:750, loss:0.00011247838488770622, w:1.4768507731928378, b:0.08964498611316783
iteration:800, loss:0.00011247838488675786, w:1.4768507735245948, b:0.0896450071353812
iteration:850, loss:0.00011247838488663068, w:1.4768507736464898, b:0.08964501485940428
iteration:900, loss:0.00011247838488661222, w:1.4768507736912766, b:0.08964501769738008
iteration:950, loss:0.00011247838488661079, w:1.476850773707732, b:0.08964501874011474
Fina loss:0.00011247838488660875, w:1.4768507737137073, b:0.08964501911873728
```

所以，当迭代100次之后，w和b的值就已经比较接近真实模型了。

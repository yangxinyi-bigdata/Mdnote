# Matplotlib

> 画图是挺难学的一部分,一直没找到什么感觉,应该如何记录呢?

## 第一步: 导入库以及一些基本的配置:

Matplotlib 有一个容易让人混淆的特性，就是它的两种画图接口： 一个是便捷的MATLAB 风格接口，另一个是功能更强大的面向对象接口.

1. 第一种是MATLAB用户风格的替代品,许多语法都和MATLAB类似, 这种接口主要位于pylot接口当中.
2. 主要的绘图函数都处于matplotlib.pyplot子库中, 因此我们通常直接导入这个子库

```python
import matplotlib.pyplot as plt
%matplotlib inline
```

>  inline的作用是不用show直接画出图

### 为了能够正常的显示中文字体,添加代码

```python
plt.rcParams['font.sans-serif']=['SimHei']
plt.rcParams['axes.unicode_minus'] = False
```

## 第二步: 基本的画图函数

### 设置画图的风格

使用某一种风格样式

```python
plt.style.use("")
```

使用这个命令查看都有哪些风格可以设置

```python
plt.style.available
```

pyplot子库中的plot函数是最基础的绘图函数,但是也很强大.

plot函数,需要两组数值

1. 第一个参数:x值,包含x坐标(横坐标)的列表或者数组
2. 第二个参数:y值,包含y坐标(纵坐标)的列表或者数组
3. 如果只传递了一个y值,那么会以索引值作为对应的x值

这样虽然确实画出图来了, 但是还有很多不符合我们要求的地方, 因此我们需要自定义很多图像的设置

现在让我们想想都有哪些功能可能, 是我们实际工作中需要用到的功能.

首先我们可以先设置一下图片

## `plt.figure()`里面可以设置很多属性

### 图片太小了看不清,我想弄的大一些

`plt.figure(figsize(20,10))`

这里的单位默认是英寸,具体多大自己试去吧

### 图像清晰度太差了,我想让它成为高清图像

```python
plt.figure(dpi=200)
```

dpi是像素密度

### 我想改一下背景颜色

```python
plt.figure(facecolor= '')
```
| | 颜色代码|
|--|--|
|b |蓝色|
|g| 绿色|
|r |红色|
|c| 青色|
|m| 品红|
|y |黄|
|k| 黑|
|w| 白|

### 我想给图像添加标题,这是当然的

```python
plt.title('')
```



### 我想给x轴和y轴添加标签

```python
plt.xlabel('')
plt.ylabel('')
```

### 我想把多条线画在同一张图上面

直接传入一个多维的数据结构

### 我想在一张大图上面画多张子图

```python
ax1 = plt.subplot(2,2,1)
ax2 = plt.subplot(2,2,2)
ax3 = plt.subplot(2,2,3)
ax4 = plt.subplot(2,2,4)
```





### 针对每一张图设置属性

```python
ax1.plot()
ax1.set_xlabel()
ax1.text
```

```python
fig, axes = plt.subplots(2, 2, subplot_kw=dict(polar=True))
axes[0, 0].plot(x, y)
axes[1, 1].scatter(x, y)
```



### 我想给图片添加网格

```python
plt.grid(True)
```

去掉网格当然就是False

### 我不想让坐标轴从0开始

想让图像显得更大一些
changes *x* and *y* axis limits such that all data is shown. 
我想让所有的数据都是可见的,通过缩小限值的方法

```python
plt.axis('tight')
```



### 我不想显示坐标轴线和标签

```python
plt.axis('off')
```

### 我想自己手动设置坐标轴的限值

```python
[xmin,xmax,ymin,ymax]

#也可以通过
plt.xlim(-1,20)
plt.ylim(-1,1)
#来设置坐标轴的限值
```



### 我想让x轴和y轴的刻度相同, 即使这样会让图形显得不那么平衡



```python
plt.axis('equal')
```



### 我想在图片上指定的位置添加文字信息说明

```python
matplotlib.pyplot.text(x, y, s, fontdict=None, withdash=False, **kwargs)
```

参数解读 : 

1. x, y：表示坐标
2. s：字符串文本；
3. fontdict：字典，可选；
4. kw：fontsize=12,
   - 对齐方式: 水平对齐 horizontalalignment=‘center’、ha=’cener’
   - 竖直对齐: verticalalignment=’center’、va=’center’

```python
fig = plt.figure(figsize=(8,6),dpi=100)
fig.text(x, y, s, *args, **kwargs)
fig.text(0.5,0.5,'son of beach',fontsize=18)
```

这个x,和y参数默认是相对于整张图

### 我想让所有数据都可见的

```python
plt.axis('image')
```



### 我想让将图形保存下来

```python
fig.savefig('my_figure.png')
```

### 我想显示我保存下来的图形

```python
from IPython.display import Image
Image('my_figure.png')
```




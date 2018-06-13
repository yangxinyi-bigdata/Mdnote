# Python基础学习笔记

老师的语言组织能力有点弱

显得有点不自信

Anaconda安装了很多数据分析用的包

Python是一门开源的编程语言

简单直观的语言，而且与主要竞争者一样强大

代码像纯英语那样容易理解

适用于短期开发的日常任务。

## Python的设计哲学

1. 优雅
2. 明确
3. 简单

## Python语言的特点

完全面向对象的语言

拥有强大的标准库

提供了大量的第三方模块，覆盖领域丰富。

## Python语言的优缺点

1. 优点

   简单易学，免费开源，面向对象，丰富的库，可扩展性

2. 缺点

   运行速度，代码无法加密

## Pthon的数据相关应用

1. 数据采集
2. 数据库连接
3. 数据清洗
4. 数据分析
5. 数据可视化，现在Python数据可视化发展的还可以，图形优化做了很多，有很多交互式的模块。
6. 机器学习和深度学习（首选）











# Python 语法学习总结

## For, While循环语句中else的用法总结

本文讨论Python的`for…else`和`while…else`等语法，这些是Python中最不常用、最为误解的语法特性之一。

Python中的`for`、`while`等循环都有一个可选的`else`分支（类似`if`语句和`try`语句那样），在循环迭代正常完成之后执行。换句话说，如果我们不是以除正常方式以外的其他任意方式退出循环，那么`else`分支将被执行。也就是在循环体内没有`break`语句、没有`return`语句，或者没有异常出现。

下面我们来看看详细的使用实例。

### **一、 常规的 if else 用法**

```python
x = True
if x:
 print 'x is true'
else:
 print 'x is not true'

```

### **二、if else 快捷用法**

这里的` if else `可以作为三元操作符使用。

```python
mark = 40
is_pass = True if mark >= 50 else False
print "Pass? " + str(is_pass)
```



### **三、与 for 关键字一起用**

在满足以下情况的时候，`else `下的代码块会被执行：

​     1、`for `循环里的语句执行完成

​     2、`for `循环里的语句没有被 `break `语句打断

```python
# 打印 `For loop completed the execution`
for i in range(10):
 print i
else:
 print 'For loop completed the execution'
# 不打印 `For loop completed the execution`
for i in range(10):
 print i
 if i == 5:
 break
else:
 print 'For loop completed the execution'

```

### **四、与 while 关键字一起用**

和上面类似，在满足以下情况的时候，`else `下的代码块会被执行：

​     1、`while `循环里的语句执行完成

​     2、`while `循环里的语句没有被 `break `语句打断

```python
# 打印 `While loop execution completed`
a = 0
loop = 0
while a <= 10:
 print a
 loop += 1
 a += 1
else:
 print "While loop execution completed"
# 不打印 `While loop execution completed`
a = 50
loop = 0
while a > 10:
 print a
 if loop == 5:
 break
 a += 1
 loop += 1
else:
 print "While loop execution completed"

```

### **五、与 try except 一起用**

和 `try except` 一起使用时，如果不抛出异常，`else`里的语句就能被执行。

```python
file_name = "result.txt"
try:
 f = open(file_name, 'r')
except IOError:
 print 'cannot open', file_name
else:
 # Executes only if file opened properly
 print file_name, 'has', len(f.readlines()), 'lines'
 f.close()

```

### **总结**

关于Python中循环语句中else的用法总结到这就基本结束了，这篇文章对于大家学习或者使用Python还是具有一定的参考借鉴价值的，希望对大家能有所帮助，如果有疑问大家可以留言交流。

## python中enumerate（）的用法

在同时需要index和value值的时候可以使用 enumerate。 

先出一个题目：1.有一 list= [1, 2, 3, 4, 5, 6]  
请打印输出：
0, 1 
1, 2 
2, 3 
3, 4 
4, 5 
5, 6 
打印输出， 
2.将 list 倒序成 [6, 5, 4, 3, 2, 1] 
3.将a 中的偶数挑出 *2 ，结果为 [4, 8, 12] 

这个例子用到了python中enumerate的用法。顺便说一下enumerate在for循环中得到计数的用法，enumerate参数为可遍历的变量，如 字符串，列表等； 返回值为enumerate类。

示例代码如下所示：

 

问题1.2.3.一同解决，代码如下:

```python
list=[1,2,3,4,5,6]

for i ,j in enumerate(list)

　　print(i,j)

list2=list[::-1]

list3=[i2 for i in list if  not i%2 ]//i%2==0证明i为偶数，not 0说明为真，也就是说i为偶数的时候i2

print（list2，list3）

>>>0,1

>>>1,2

>>>2,3

>>>3,4

>>>4,5

>>>5,6

>>>[6,5,4,3,2,1]

>>>[4,8,12]

```

下列分别将字符串，数组，列表与字典遍历序列中的元素以及它们的下标：

一，字符串：

for i,j in enumerate('abcde'):  

　　 print i,j  

 

\>>>0,a

\>>>1,b

\>>>2,c

\>>>3,d

\>>>4,e

 

二，数组：

for i,j in enumerate(('a','b','c')):  

　　print i,j  

 

输出结果为：

\>>>0 a 

\>>>1,b

\>>>2,c

 

三，列表：

 

**案例在开头已经说过。**

四，字典：

for i,j in enumerate({'a':1,'b':2}):  

　　print i,j  

 

输出结果为：

\>>>0 a 

\>>>1,b





## Numpy

科学计算包 

numpy一些不好记的但是可能要用到的语法笔记

```python
np.vstack((a,b))  # 把两个矩阵纵向叠加到一起
# 叠加到一起的数组必须要有相同的维度, all the input arrays must have same number of dimensions
```

```python
np.hstack((a,b)) # 把两个矩阵横向叠加到一起
```

对于任何输入数组，函数`row_stack`相当于[`vstack`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.vstack.html#numpy.vstack)。 

函数[`column_stack`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.column_stack.html#numpy.column_stack)将1D数组作为列叠加到2D数组中。它相当于仅用于二维数组的[`hstack`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.hstack.html#numpy.hstack) 

在复杂情况下，[`r_`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.r_.html#numpy.r_)和[`c_`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.c_.html#numpy.c_)可用于通过沿一个轴叠加数字来创建数组。它们允许使用范围文字（“：”）

```python
>>> np.r_[1:4,0,4]
array([1, 2, 3, 0, 4])
```

当以数组作为参数使用时，[`r_`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.r_.html#numpy.r_)和[`c_`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.c_.html#numpy.c_)类似于其默认行为中的[`vstack`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.vstack.html#numpy.vstack)和[`hstack`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.hstack.html#numpy.hstack)，但是允许一个可选参数给出要沿其连接的轴的编号。 

```python
np.r[a, b]
>>> array([[ 4.,  8.],
       [ 1.,  4.],
       [ 8.,  6.],
       [ 7.,  8.]])
```

数组向下取整/数组向上取整

```python
a = np.floor(10*np.random.random((2,12)))
b = np.ceil([1.1, 2.3])
```

拆分数组

使用[`hsplit`](https://yiyibooks.cn/__trs__/xx/NumPy_v111/reference/generated/numpy.hsplit.html#numpy.hsplit)，可以沿其水平轴拆分数组，通过指定要返回的均匀划分的数组数量，或通过指定要在其后进行划分的列： 

```python
np.hsplit(a,3) 
np.hsplit(a,(3,4))
```

### numpy中复制和视图

1. 完全不复制

   ```python
   b = a  # 这样赋值操作, a和b是完全一样的东西, 改动其中一个会两个都变化
   ```

2. #### 视图或浅复制

   ```python
   c = a.view()
   #通过这种方式创建的视图, 共享相同的数据, 但是格式结构可以改变, 可以有不同的形状, 维度等等, 但是其中一个数据被改变, 则两个数据都会改变.
   ```

3. #### 深复制

   `copy`方法生成数组及其数据的完整拷贝。 

   ```python
   d = a.copy()   
   # 这样d 和a 变成两个完全不相关的东西了, 这就叫深复制(deepcopy)
   ```

numpy广播规则/Broadcasting规则

Broadcasting允许通用函数以有意义的方式处理具有不完全相同形状的输入。 

Broadcasting的第一个规则是，如果所有输入数组不具有相同数量的维度，则“1”将被重复地添加到较小数组的形状，直到所有数组具有相同数量的维度。 

### 花式索引和索引技巧

NumPy提供了比常规Python序列更多的索引能力。正如我们前面看到的，除了通过整数和切片进行索引之外，还可以使用整数数组和布尔数组进行索引。

#### 使用索引数组索引

```python
>>> a = np.arange(12)**2                       # the first 12 square numbers
>>> i = np.array( [ 1,1,3,8,5 ] )              # an array of indices
>>> a[i]                                       # the elements of a at the positions i
array([ 1,  1,  9, 64, 25])
>>>
>>> j = np.array( [ [ 3, 4], [ 9, 7 ] ] )      # a bidimensional array of indices
>>> a[j]                                       # the same shape as j
array([[ 9, 16],
       [81, 49]])
```

#### 使用布尔数组索引

当我们用（整数）索引数组索引数组时，我们提供了要选择的索引列表。使用布尔索引，方法是不同的；我们明确地选择数组中的哪些元素我们想要的，哪些不是。

The most natural way one can think of for boolean indexing is to use boolean arrays that have *the same shape* as the original array:

```python
>>> a = np.arange(12).reshape(3,4)
>>> b = a > 4
>>> b                                          # b is a boolean with a's shape
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]], dtype=bool)
>>> a[b]                                       # 1d array with the selected elements
array([ 5,  6,  7,  8,  9, 10, 11])
```

此属性在赋值时非常有用：

```python
>>> a[b] = 0                                   # All elements of 'a' higher than 4 become 0
>>> a
array([[0, 1, 2, 3],
       [4, 0, 0, 0],
       [0, 0, 0, 0]])
```

你可以查看以下示例，了解如何使用布尔索引生成[Mandelbrot集](http://en.wikipedia.org/wiki/Mandelbrot_set)的图像：

```python
import numpy as np
import matplotlib.pyplot as plt
def mandelbrot( h,w, maxit=20 ):
    """Returns an image of the Mandelbrot fractal of size (h,w)."""
    y,x = np.ogrid[ -1.4:1.4:h*1j, -2:0.8:w*1j ]
    c = x+y*1j
    z = c
    divtime = maxit + np.zeros(z.shape, dtype=int)
# ...
    for i in range(maxit):
        z = z**2 + c
        diverge = z*np.conj(z) > 2**2            # who is diverging
        div_now = diverge & (divtime==maxit)  # who is diverging now
        divtime[div_now] = i                  # note when
        z[diverge] = 2                        # avoid diverging too much
# ...
    return divtime
plt.imshow(mandelbrot(400,400))
plt.show()

```



![../_images/quickstart-1.png](https://yiyibooks.cn/__trs__/xx/NumPy_v111/_images/quickstart-1.png)

第二种使用布尔索引的方法更类似于整数索引;对于数组的每个维度，我们给出一个一维布尔数组，选择我们想要的切片：

```python
>>> a = np.arange(12).reshape(3,4)
>>> b1 = np.array([False,True,True])             # first dim selection
>>> b2 = np.array([True,False,True,False])       # second dim selection
>>>
>>> a[b1,:]                                   # selecting rows
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[b1]                                     # same thing
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[:,b2]                                   # selecting columns
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10]])
>>>
>>> a[b1,b2]                                  # a weird thing to do
array([ 4, 10])
```

请注意，1D布尔数组的长度必须与你要切片的维度（或轴）的长度一致。在前面的示例中，`b1`是rank为1的数组，其长度为3（`a`中*行*的数量），`b2`（长度4）适合于索引`a`的第二个rank（列）。

## Pandas

Python Data Analysis Library 或 pandas 是基于NumPy 的一种工具，该工具是为了解决数据分析任务而创建的。Pandas 纳入了大量库和一些标准的数据模型，提供了高效地操作大型数据集所需的工具。pandas提供了大量能使我们快速便捷地处理数据的函数和方法。你很快就会发现，它是使Python成为强大而高效的数据分析环境的重要因素之一。 

## Matplotlib



## Python3与Python2比较

Python3对于新手入门更好，更改了很多Python2的弊端，优化了很多。

## Python 错误和异常

### 1. 语法错误(Syntax Errors)

语法错误, 也就是解析时错误。当我们写出不符合python语法代码时， 在解析时会报SytaxError， 并且会显示出错的哪一行， 并且用小箭头指明最早探测到错误的位置。：

```python
>>> while Ture
  File "<stdin>", line 1
    while Ture
             ^
SyntaxError: invalid syntax
```

### 2. 异常 (Exceptions)

即使语句或表达式在语法上是正确的, 但在尝试运行时也可能发生错误, 运行时错误就叫做异常(Exception). 异常并不是致命问题, 因为我们可以在程序运行中对异常进行处理.

```python
>>> 10 * (1 / 0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero

>>> 2 + x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined

>>> 2 + '2'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```



上面显示了三种Exceptions的类型：ZeroDivisionError，NameError，TypeError，它们都是内置异常的名称。标准异常的名字就是内建的标识符（但并不是关键字）。 

## python标准异常

| 异常名称                  | 描述                                               |
| ------------------------- | -------------------------------------------------- |
|                           |                                                    |
| BaseException             | 所有异常的基类                                     |
| SystemExit                | 解释器请求退出                                     |
| KeyboardInterrupt         | 用户中断执行(通常是输入^C)                         |
| Exception                 | 常规错误的基类                                     |
| StopIteration             | 迭代器没有更多的值                                 |
| GeneratorExit             | 生成器(generator)发生异常来通知退出                |
| StandardError             | 所有的内建标准异常的基类                           |
| ArithmeticError           | 所有数值计算错误的基类                             |
| FloatingPointError        | 浮点计算错误                                       |
| OverflowError             | 数值运算超出最大限制                               |
| ZeroDivisionError         | 除(或取模)零 (所有数据类型)                        |
| AssertionError            | 断言语句失败                                       |
| AttributeError            | 对象没有这个属性                                   |
| EOFError                  | 没有内建输入,到达EOF 标记   **好像是读取异常**     |
| EnvironmentError          | 操作系统错误的基类                                 |
| IOError                   | 输入/输出操作失败                                  |
| OSError                   | 操作系统错误                                       |
| WindowsError              | 系统调用失败                                       |
| ImportError               | 导入模块/对象失败                                  |
| LookupError               | 无效数据查询的基类                                 |
| IndexError                | 序列中没有此索引(index)                            |
| KeyError                  | 映射中没有这个键                                   |
| MemoryError               | 内存溢出错误(对于Python 解释器不是致命的)          |
| NameError                 | 未声明/初始化对象 (没有属性)                       |
| UnboundLocalError         | 访问未初始化的本地变量                             |
| ReferenceError            | 弱引用(Weak reference)试图访问已经垃圾回收了的对象 |
| RuntimeError              | 一般的运行时错误                                   |
| NotImplementedError       | 尚未实现的方法                                     |
| SyntaxError               | Python 语法错误                                    |
| IndentationError          | 缩进错误                                           |
| TabError                  | Tab 和空格混用                                     |
| SystemError               | 一般的解释器系统错误                               |
| TypeError                 | 对类型无效的操作                                   |
| ValueError                | 传入无效的参数                                     |
| UnicodeError              | Unicode 相关的错误                                 |
| UnicodeDecodeError        | Unicode 解码时的错误                               |
| UnicodeEncodeError        | Unicode 编码时错误                                 |
| UnicodeTranslateError     | Unicode 转换时错误                                 |
| Warning                   | 警告的基类                                         |
| DeprecationWarning        | 关于被弃用的特征的警告                             |
| FutureWarning             | 关于构造将来语义会有改变的警告                     |
| OverflowWarning           | 旧的关于自动提升为长整型(long)的警告               |
| PendingDeprecationWarning | 关于特性将会被废弃的警告                           |
| RuntimeWarning            | 可疑的运行时行为(runtime behavior)的警告           |
| SyntaxWarning             | 可疑的语法的警告                                   |
| UserWarning               | 用户代码生成的警告                                 |

## 处理异常（try…except…）

我们可以使用try…except…语句来处理异常。try语句块中是要执行的语句，except语句块中是异常处理语句。一个try语句可以有多个except语句，用以指定不同的异常，但至多只有一个会被执行。

语法：

以下为简单的*try....except...else*的语法：

```python
try:
<语句>        #运行别的代码
except <名字>：
<语句>        #如果在try部份引发了'name'异常
except <名字>，<数据>:
<语句>        #如果引发了'name'异常，获得附加的数据
else:
<语句>        #如果没有异常发生
```

try的工作原理是，当开始一个try语句后，python就在当前程序的上下文中作标记，这样当异常出现时就可以回到这里，try子句先执行，接下来会发生什么依赖于执行时是否出现异常。

- 如果当try后的语句执行时发生异常，python就跳回到try并执行第一个匹配该异常的except子句，异常处理完毕，控制流就通过整个try语句（除非在处理异常时又引发新的异常）。
- 如果在try后的语句里发生了异常，却没有匹配的except子句，异常将被递交到上层的try，或者到程序的最上层（这样将结束程序，并打印缺省的出错信息）。
- 如果在try子句执行时没有发生异常，python将执行else语句后的语句（如果有else的话），然后控制流通过整个try语句。

### 实例

下面是简单的例子，它打开一个文件，在该文件中的内容写入内容，且并未发生异常：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print "Error: 没有找到文件或读取文件失败"
else:
    print "内容写入文件成功"
    fh.close()
```

以上程序输出结果：

```python
$ python test.py 
内容写入文件成功
$ cat testfile       # 查看写入的内容
这是一个测试文件，用于测试异常!!
```

```python
try:
    x = int(input('please input a integer:'))
    if 30 / x >5:
        print("Hello world!")
except ValueError:
    print('That was no valid number .Try again...')
except ZeroDivisionError:
    print("The divior can not be zero .Try again...")
except:
    print("Handling other exceptions...")
```

上面这段代码，当输入a(非数字）时，将抛出ValueError； 
当输入0时，将抛出ZeroDivisionError； 
当输入其他类型的异常时，将执行`except:`后的处理语句。 
如果在try执行时 ，出现了一个异常，该语句剩下部分将被跳过。并且如果该异常的类型匹配到了except后面的异常名，那么该except后的语句将被执行。*注意如果except后面没有跟异常名，表示匹配任何类型的异常，except：必须放在最后*。 
一个except可以同时包括多个异常名，但需要用括号括起来，如：

```python
>>> try:
...     a = input()
... except (Runtime,TypeError,NameError):
...     print("exception")
```



try…except…语句还可以有一个可选的else语句，else语句必须放在所有except语句之后，当没有异常发生的时候，else从句将被执行。 

## 使用except而带多种异常类型

你也可以使用相同的except语句来处理多个异常信息，如下所示：

```python
try:
    正常的操作
   ......................
except(Exception1[, Exception2[,...ExceptionN]]]):
   发生以上多个异常中的一个，执行这块代码
   ......................
else:
    如果没有异常执行这块代码
```





## 抛出异常

raise允许程序员强制地抛出一个特定的异常，例如：

```python
>>>raise NameError #抛出异常
TrackBack (most recent call last):
    File "<stdin>", line 1, in <moudle>
NameError
```

raise抛出的异常必须是一个异常实例或类（派生自Exception的类）

```python
>>> try:
...     raise NameError('HiThere')
... except NameError:
...     print('An exception flew by!')
...     raise
...
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: HiThere
```





## 清理动作（finally）

try语句有另一种可选的`finally`从句，用于自定义一些扫尾清理工作。 
与 else 从句的区别在于： else 语句只在没有异常发生的情况下执行，而 finally 语句则不管异常发生与否都会执行。准确的说，finally 语句总是在退出 try 语句前被执行，无论是正常退出、异常退出，还是通过break、continue、return退出。

```python
>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
... 
Goodbye, world!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
KeyboardInterrupt
```

**下面看一个案例:**

目标

请写出一个 MinutesToHours.py 脚本文件，实现一个函数 Hours()，将用户输入的 分钟数 转化为 小时数和分钟数，并要求小时数尽量大。讲结果以 XX H, XX M 的形式打印出来。(注意打印格式中的空格)

要求

```python
用户能够通过命令行输入分钟数，程序需要打印出相应的小时数和分钟数
如果用户输入的是一个负值，程序需要报错 ValueError
需要进行 try...except 操作来控制异常。如果异常，在屏幕上打印打印出 ValueError： Input number cannot be negative 提示用户输入的值有误
1234
```

代码如下

```python
#!/usr/bin/env python3
import sys
def Hours(m):
    H,M = divmod(m,60) #得到一个商和余数的元组
    return '{} H, {} M'.format(H,M)
try:
    m = int(sys.argv[1]) #传入的第一个参数
    if m < 0:
        raise ValueError
    else:
        print(Hours(m))
except ValueError:
    print('ValueError: Input number cannot be negative' )
```

## 异常的参数

一个异常可以带上参数，可作为输出的异常信息参数。

你可以通过except语句来捕获异常的参数，如下所示：

```python
try:
    正常的操作
   ......................
except ExceptionType, Argument:
    你可以在这输出 Argument 的值...
```

变量接收的异常值通常包含在异常的语句中。在元组的表单中变量可以接收一个或者多个值。

元组通常包含错误字符串，错误数字，错误位置。

### 实例

以下为单个异常的实例：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# 定义函数
def temp_convert(var):
    try:
        return int(var)
    except ValueError, Argument:
        print "参数没有包含数字\n", Argument

# 调用函数
temp_convert("xyz");
```

以上程序执行结果如下：

```python
$ python test.py 
参数没有包含数字
invalid literal for int() with base 10: 'xyz'
```



## 触发异常

我们可以使用raise语句自己触发异常

raise语法格式如下：

```python
raise [Exception [, args [, traceback]]]
```

语句中 Exception 是异常的类型（例如，NameError）参数标准异常中任一种，args 是自已提供的异常参数。

最后一个参数是可选的（在实践中很少使用），如果存在，是跟踪异常对象。

### 实例

一个异常可以是一个字符串，类或对象。 Python的内核提供的异常，大多数都是实例化的类，这是一个类的实例的参数。

定义一个异常非常简单，如下所示：

```python
def functionName( level ):
    if level < 1:
        raise Exception("Invalid level!", level)
        # 触发异常后，后面的代码就不会再执行
```

**注意：**为了能够捕获异常，"except"语句必须有用相同的异常来抛出类对象或者字符串。

例如我们捕获以上异常，"except"语句如下所示：

```python
try:
    正常逻辑
except Exception,err:
    触发自定义异常    
else:
    其余代码
```

### 实例

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# 定义函数
def mye( level ):
    if level < 1:
        raise Exception,"Invalid level!"
        # 触发异常后，后面的代码就不会再执行
try:
    mye(0)            # 触发异常
except Exception,err:
    print 1,err
else:
    print 2
```

执行以上代码，输出结果为：

```python
$ python test.py 
1 Invalid level!
```

## 用户自定义异常

通过创建一个新的异常类，程序可以命名它们自己的异常。异常应该是典型的继承自Exception类，通过直接或间接的方式。

以下为与RuntimeError相关的实例,实例中创建了一个类，基类为RuntimeError，用于在异常触发时输出更多的信息。

在try语句块中，用户自定义的异常后执行except块语句，变量 e 是用于创建Networkerror类的实例。

```python
class Networkerror(RuntimeError):
    def __init__(self, arg):
        self.args = arg
```

在你定义以上类后，你可以触发该异常，如下所示：

```python
try:
    raise Networkerror("Bad hostname")
except Networkerror,e:
    print e.args
```



## 东哥python课程学习笔记

### python中函数多个返回值

```python
def test(): # 我的需求时把abc都返回回来
    a = 11
    b = 22
    c = 22
    return a
    return b
    return c  # 如果这样写, 返回到a就已经结束函数了
num = test()
print(num)
'''
如果一个函数想要返回多个函数, 你想要
'''    
```

那我们怎么做才能让他返回多个值呢?

** 用一个列表封装多个变量的值

```python
def test(): # 我的需求时把abc都返回回来
    a = 11
    b = 22
    c = 22
    d = [a, b, c]
    return d  # 把三个变量封装起来一次性返回, 这是很有用的思想
num = test()
print(num[1], num[2], num[3 ])
'''
上面是第一种方法返回多个值
'''    
return [a,b,c]

num = test()  # 这样num就获得了这个列表

'''
第三种
'''
return a, b, c
# 这种方法相当于把abc封装成元组返回了
```



### 函数的四种类型

```python
def 函数名():
    pass

def 函数名():
    return xxx
def 函数名(参数):
    pass
def 函数名(参数):
    return xxx
```



### 函数的嵌套调用

``` python
def test1():
    pass

def test2():
    print("-----2-1----")
    print("heihei")
    print("-----2-2----")

def test3():
    print("-----3-1----")
    test2()
    print("-----3-2----")

test3()

```

#### 函数嵌套调用的应用

```python
def print_line(): ## 编写函数的时候, 不用急着写里面的实现方法
    pass
print_line()  # 先把这个框架写完, 然后回过头去再去填充里面的实现方法
```

```python
def print_line():
    print("-"*20)
print_line()   # 
```

函数利用的功能就是, 可以重复使用

```python
def print_5_line():
    i = 0
    while i < 5:
        print_line()
        i += 1
        
print_5_line()
```



软件一般都讲快速迭代, 意思是在我上一个版本上改, 完成一些新功能, 这就是迭代的思想.

```python
def jiahe(a, b, c):
	return a+b+c

input ("a=:")
input ("b=")
input ("c=")
jiahe(a, b, c)
```

```python
# 正常的编程模式是, 先写大框架, 等用到了函数的时候再去写函数
# 1. 获取3个数值
num1 = int(input())
num2 = int(input())
num3 = int(input())


```



# 

#Python爬虫

网络爬虫（又被称为网页蜘蛛，网络机器人），是一种按照一定的规则，
自动地抓取万维网信息的程序或者脚本。 另外一些不常使用的名字还
有模拟程序。

通用爬虫：搜索引擎，Google, Baidu, Bing等。

聚焦爬虫：自动、批量下载网页的程序，它根据既定的抓取目标，有选
择的访问万维网上的网页与相关的链接，获取所需要的信息。与通用爬
虫(general purpose web crawler)不同，聚焦爬虫并不追求大的覆
盖，而将目标定为抓取与某一特定主题内容相关的网页，为面向主题的
用户查询准备数据资源。

我们平时使用的爬虫就是聚焦爬虫.

## 爬虫涉及到的相关知识

1. 网络协议(特别是HTTP协议)
2. 编程技能(各类爬虫相关的库)(抓:Requests, 解析:BeautifulSoup, Selenium, Scrapy, pyspider)
3. 搜索途径
   - 通用类：google， baidu， bing 搜索引擎等
   - 特定问答：stackoverflow等
   - 其它有用的工具：GitHub等（借力）

爬虫有三步, 第一步是**抓**(requests.get), 第二步是**析**(BeautifulSoup, re, xpath, css, selector) , 第三步是**存**(df.to_csv, pymysql)

### 不同的请求方法:

![1526312726328](pictures/43)

### HTTP状态码

| 1XX： | 信息性状态码（服务器正在处理请求）                           |
| ----- | ------------------------------------------------------------ |
| 2XX： | 成功状态码（请求正常处理完毕）                               |
| 3XX： | 重定向状态码（需要进行额外操作以完成请求）爬虫库一般会对重定向做处理；动态网页具有自动跳转的特点 |
| 4XX： | 客户端错误状态码（服务器无法处理请求）                       |
| 5XX： | 服务器错误状态码（服务器处理请求出错）服务器错误，通常可以进行一定次数的重连，然后放弃 |

### 常见的状态码

- 200 OK：客户端发来的请求在服务器端被正常处理
- 400 Bad Request: 报文存在语法错误，服务器无法理解（例如：前后端编程逻辑不一致导致）
- 403 Forbidden：请求资源的访问被服务器拒绝（服务器不用给出理由）需要验证登录，或者被封IP
- 404 Not Found: 服务器上没有请求的资源找不到，一般会跳过不爬
- 500 Internal Server Error: 服务器端执行请求时发生故障
- 503 Service Unavailable：服务器超负荷或者停机维护

### HTTP请求头信息

- Host：请求主机器名，可为IP也可为域名
- Accept-Encoding：可接受的文本压缩算法，如：gzip, deflate
- Accept-Language：支持语言，客户端浏览器的设置，如：zh-cn,
- zh;q=0.8,en-us;q=0.5,en;q=0.3
- User-Agent：浏览器信息，如：Mozilla/5.0 (Macintosh; Intel Mac OS
- X 10.7; rv:12.0) Gecko/20100101 Firefox/12.0
- Cookie：服务器在上次设置的COOKIE，包括作用域名，过期时间，键与值。
- 如：BAIDUID=49415814CDBBB4CE65EC50EE4BB65E9A:FG=1;
- expires=Wed, 07-Nov-42 07:03:34 GMT; path=/;
- domain=.baidu.com
- Referer：从一个连接打开一个新页面，新页面的请求一般会加此信息，标名
- 是从哪里跳过来的

### HTTP响应头信息

- Allow：服务器支持哪些请求方法，如GET、POST等。
- Cache-Control：从服务器到客户端的缓存机制，如：Cache-Control:
- max-age=3600
- Connection：HTTP连接策略，如：Connection: Keep-Alive
- Content-Encoding：响应资源所使用的编码类型，如：gzip，deflate
- Transfer-Encoding：数据在网络传输当中的编码方式，如：chunked
- Content-Type：响应内容的文档类型(html,json等），如: text/html;
- charset=utf-8
- Date：服务器端响应消息发出时的GMT时间。
- Last-Modified：服务器端响应内容文档的最后改动时间。
- Set-Cookie：设置和所访问页面关联的Cookie。

### 异常的知识

HttpError

如果不是2开头的, 就是异常了.

> HTTPError
> 如果 HTTP 请求返回了不成功的状态码， Response.raise_for_status() 会抛出一个 HTTPError 异常。
> ConnectionError
> 遇到网络问题（如：DNS 查询失败、拒绝连接等）时，Requests 会抛出一个 ConnectionError 异常。
> ConnectTimeout
> 若请求超时，Requests 则会抛出一个 Timeout 异常。

### 网页结构介绍

**网页分成三个层次，即：结构层(HTML)、表示层(CSS)、行为层(Javascript)。**

1. HTML ：超文本标记语言（ Hyper Text Markup Language ），是用来描述网页的一种语言。
2. CSS ：层叠样式表（ Cascading Style Sheets) ，定义如何显示 HTML  元素，语法为： selector {property ： value} ( 选择符 { 属性：值 })
3. JavaScript 是一种脚本语言，其源代码在发往客户端运行之前不需经过编译，而是将文本格式的字符代码发送给浏览器由浏览器解释运行。
4. 对于一个网页，HTML定义网页的结构，CSS描述网页的样子，JavaScript设置对浏览器事件的响应（点击按钮，输入文本等）。
5. HTML就像 一个人的骨骼、器官，而CSS就是人的皮肤，有了这两样就构成了一个植物人，加上javascript就可以对外界刺激做出反应，可以思考、运动等等。

### 动态网址

同一个网页可以呈现出不同的内容.

### 编码

- ASCII  ask码
- Unicode 包含所有语言 
- 和 UTF-8编码 Unicode的优化

在python中只有Unicode可以正常打印

## 基础Python爬虫库（requests/bs4）

Requests 构建HTTP请求方法

1. requests.get() # 获取html页面
2. requests.head() # 获取html页面头信息
3. requests.post() # 提交post请求
4. requests.put() # 提交put请求
5. requests.patch() # 提交局部修改
6. requests.delete() # 提交删除请求

- 字符串在Python内部的表示是unicode编码，因此，在做编码转换时，通常需要以unicode作为中间编码，即先将其他编码的字符串解码（decode）成unicode，再从unicode编码（encode）成另一种编码。 
- decode是将其他编码的字符串转换成unicode编码，如str1.decode('gb2312')，表示将gb2312编码的字符串str1转换成unicode编码。 
- encode是将unicode编码转换成其他编码的字符串，如str2.encode('gb2312')，表示将unicode编码的字符串str2转换成gb2312编码。 
- 因此，转码的时候一定要先搞明白，字符串str是什么编码，先decode成unicode，然后再encode成其他编码。 

---

```python
# r = requests.get(url)
# r: 返回一个包含服务器资源的Response对象
# 构建一个Request对象，向服务器提交请求

# requests.get(url, params = None, **kwargs)
# url: 页面地址
# params： 额外参数
# **kwargs: 控制访问的参数（12个,allow_redirects,timeout等）
```

### 反爬虫相关知识

- 设置User Agent

有一些网站不喜欢被爬虫程序访问，所以会检测连接对象，如果是爬虫程序，也就是非人点击访问，它就会不让你继续访问，所以为了要让程序可以正常运行，需要隐藏自己的爬虫程序的身份。此时，我们就可以通过设置User Agent的来达到隐藏身份的目的，User Agent的中文名为用户代理，简称UA。

User Agent存放于Headers中，服务器就是通过查看Headers中的User Agent来判断是谁在访问。在Python中，如果不设置User Agent，程序将使用默认的参数，那么这个User Agent就会有Python的字样，如果服务器检查User Agent，那么没有设置User Agent的Python程序将无法正常访问网站。

Python允许我们修改这个User Agent来模拟浏览器(PC端或移动端)访问。

- 常见的User Agent

1.Android

Mozilla/5.0 (Linux; Android 4.1.1; Nexus 7 Build/JRO03D) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Safari/535.19

Mozilla/5.0 (Linux; U; Android 4.0.4; en-gb; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30

Mozilla/5.0 (Linux; U; Android 2.2; en-gb; GT-P1000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1

2.Firefox

Mozilla/5.0 (Windows NT 6.2; WOW64; rv:21.0) Gecko/20100101 Firefox/21.0

Mozilla/5.0 (Android; Mobile; rv:14.0) Gecko/14.0 Firefox/14.0

3.Google Chrome

Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36

Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile Safari/535.19

4.iOS

Mozilla/5.0 (iPad; CPU OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3

Mozilla/5.0 (iPod; U; CPU like Mac OS X; en) AppleWebKit/420.1 (KHTML, like Gecko) Version/3.0 Mobile/3A101a Safari/419.3

上面列举了Andriod、Firefox、Google Chrome、iOS的一些User Agent，直接copy就能用。

#### 设置代理IP

```thon
# 采集时为避免被封IP，经常会使用代理。
# requests有相应的proxies属性。
# 西刺代理：http://www.xicidaili.com/
```



# 弄到有道翻译那块

```python
# import requests
# import BeautifulSoup

# r = requests.get(url)
# r.encoding = "utf8"
# bs = BeautifulSoup(r.text, "lxml")

### 1)利用属性访问方法查找需要的信息（返回Tag）
# bs.tag
# bs.tag1.tag2

### 2)利用find,findall方法查找需要的信息
# bs.find 返回的是Tag类型
# bs.find("tag")
# bs.find(["tag1","tag2"])
# bs.find("tag", {"attr":"value"})
# bs.find("tag", {"attr":["value1", "value2"]})

# bs.findAll 返回的是列表，里面元素是Tag类型
# bs.findAll("tag")
# bs.findAll(["tag1","tag2"])
# bs.findAll("tag", {"attr":"value"})
# bs.findAll("tag", {"attr":["value1", "value2"]})

### 3)利用select方法查找需要的信息（返回列表）

### CSS选择器
# bs.select('tag')            # 匹配所有名为<tag>的元素
# bs.select('#idvalue')             # 匹配所有id为idvalue的元素
# bs.select('.classvalue')            # 匹配所有class为classvalue的元素
# bs.select('tag1 tag2')           # 匹配所有tag1标签内tag2的元素
# bs.select('tag[attr]')                # 匹配所有tag标签，属性为attr的元素
# bs.select('tag[attr =value]')            # 匹配所有tag标签，属性为attr的值为value的元素

```

## 模拟用户点击行为

Selenium与PhantomJS工具(模拟用户浏览器行为)

- Selenium与PhantomJS的安装：
- pip install selenium
- PhantomJS下载，安装，设置PATH

### Selenium+PhantomJS 

Selenium：自动化web测试解决方案，完全模拟真实的浏览器环境，可以模拟所有的用户操作 

- Selenium也是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。
- <https://seleniumhq.github.io/selenium/docs/api/py/api.html>

PhantomJS ：一个没有图形界面的浏览器

- PhantomJS是一个基于webkit的javascript API。它使用QtWebKit作为它核心浏览器的功能，使用webkit来编译解释执行JavaScript代码。任何你可以在基于webkit浏览器做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如CSS选择器、支持Web标准、DOM操作、JSON、html5、Canvas、SVG等，同时也提供了处理文件I/O的操作，从而使你可以向操作系统读写文件等。PhantomJS的用处可谓非常广泛，诸如网络监测、网页截屏、无需浏览器的 Web 测试、页面访问自动化等。
- <http://phantomjs.org/download.html>
- phantomjs.exe文件路径加入环境变量

JavaScript对于页面的操纵：

demo

baidu.com

```
var container = document.getElementById('qrcode');
container.remove();

var buttons = document.getElementsByClassName('mnav');
Button = buttons[0]
Button.click();
```

##### 利用Selenium + Chrome Driver模拟用户操作浏览器

- chromedriver与chrome版本映射表：<https://blog.csdn.net/huilan_same/article/details/51896672>
- chromedriver下载地址：<http://chromedriver.storage.googleapis.com/index.html>
- chromedriver.exe文件路径加入环境变量



直播标题 主播 观看人数 游戏名  前十页



# Scikit-Learn入门

聚类算法非常重要: DBSCAN, 层次聚类.

Scikit-Learn提供了非常丰富的API.

如果你掌握了其中一种模型的用法, 其他模型可以非常平滑的过度上去.

## 1. 数据表示

 基本的数据表就是二维网格数据，其中的每一行表示数据集中的每个样本，而列列表示构成每个样本的
相关特征。例如之前所使用的iris鸢尾花数据集，行数表示数据集中记录的鸢尾花总数，每列列数据表示每个样本某个特征的量量化值，同时对于有监督学习算法的数据集⽽而⾔言，还需要有⼀一列列特征充当标签列列。同时，在读取数据的过程中，我们常常把数据读取保存为DataFrame格式，从而方便对数据集本身进行探索和预处理理。

````python
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
````

````python
iris = pd.read_csv('iris.txt', header=None)
````

同时，将DF作为数据接口，在进行后续数据格式转换时也非常方便。例如在Scikit-Learn中，很多模型需要以Numpy数组作为输入数据，此时可以使用Pandas的pa.values将原始DF转化为算法库要求的array数据格式。同时，对于Scikit-Learn而言，要求数据集特征列和标签列单独存放至两个数组中，而数据集的特征列往往有多列，因此输入特征往往以二维数组进行表示，也被称为特征矩阵，而存放标签列的数组，往往也被称为目标数组。

## 2. 特征矩阵(features matrix)

特征矩阵通常被记为变量X, 它是维度为[n_samples, n_features]的二维矩阵, 通常可以用Numpy数据或者Pandas的DataFrame来表示, 



# PySpark知识点总结:

在企业里面, 一旦用惯了的环境和软件, 是非常不愿意换的, 因为一旦换了就有可能出现很多不必要的问题.

只有在没有办法完成现在的工作的时候, 才会考虑换新的软件.

## python2.7和python3.5共存

1. 安装anaconda2, 官网下载
2. 配置anaconda2的环境变量
3. 打开cmd, 输入python确认环境变成python2.7, 即可表示成功

也可以使用这种更灵活的方法

https://blog.csdn.net/l_s_f123/article/details/77841973

在这个页面的介绍方法

````python
conda create -n python27 python=2.7 anaconda
````

上面的代码创建了一个名为python27的python2.7的环境，最后一个参数表示安装anaconda下python2.7的所有默认包，这个参数时可选的。 
截止到这里，我们实现了在win10环境上，python2和python3的共存。 
下面介绍python2和python3版本的相互转换。 
我们进入cmd环境，现在默认的python版本时python3.6。只需要一行简单的代码就可以转为python2.7的环境。

```python
activate python27
```

此时，本窗口下的python版本变为了python2.7。那么，你肯定猜到了恢复到python3.6的命令：

```
deactivate python271
```

其实呢，一般没有必要恢复到原环境。只要打开一个新的cmd窗口，默认的python版本就是python3.6。

![1528511858618](pictures/112)

Pycharm安装方法

首先下载开发版, 安装后, 先用老师发的"pyharm激活"文档激活方法, 将开发版本激活.

激活后选择python9创建一个新项目, 选择路径.

然后在设置里面的Project Interpreter, 点击后面的设置图标, add local, 选择需要的python2或者3解释器.选择ok

将 C:\Users\Administrator\PycharmProjects\PySpark\spark-1.6.3-bin-hadoop2.4\python\lib下的两个包 

py4j以及pyspark拷贝到C:\Anaconda2\Lib\site-packages目录下 

### 传统的分析思路

1. 对文件源进行分类, 并对数据进行分类
   1. 本项目中我们分成cs_interaction| influence| keyword_interaction| menu_interaction| userinfo
2. 设计表的结构, 产生ddl语句
3. 使用大数据框架Spark对源文件进行分析
4. 将分析到的结果插入到目标数据库中.

### 将某些数据直接生成词云

1. 将原始数据直接用Spark框架分析处理, 统计出关键词, 并用词云的可视化工具进行展示
2. 分析结果作为对象插入到数据库中, 方便后续的分析挖掘.

root的密码010025 
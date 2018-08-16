

# Python Math模块函数说明

## **0，常量**

math.pi         π = 3.141592...
math.e          e = 2.718281...

## **1，数值计算函数**

math.ceil(x)            返回≥x的最小整数

math.floor(x)           返回≤x的最大整数

math.copysign(x,y)      返回与y同号的x值

math.fabs(x)            返回x的绝对值

math.factorial(x)       返回x的阶乘，即x!，x必须为非负整数

math.fmod(x,y)          返回x对y取模的余数(x决定余数符号)，与x%y不同(y决定余数符号)

   例：   math.fmod(100, -3)   -->  1.0

​         math.fmod(-100, 3)   --> -1.0

​         100 % -3    -->    -2

​        -100 %  3    -->     2

math.frexp(x)           返回元组(m,e)，根据 x = m*(2**e)

math.fsum(iterable)     返回数组的和，比内置函数sum要精确

math.isfinite(x)        若x是有限数，返回True

math.isinf(x)           若x是无穷大，返回True

math.isnan(x)           若x非数，返回True

math.ldexp(x,i)         返回x*(2**i)的结果

math.modf(x)            返回元组(fractional,integer)，分别为x的小数部分和整数部分

math.trunc(x)           返回x的整数部分



## **2，乘方/对数函数**

math.exp(x)             返回e**x

math.expm1(x)           返回e**x - 1

math.log(x[,base])      返回x的对数，base默认的是e

math.log1p(x)           返回x+1的对数，base是e

math.log2(x)            返回x关于2的对数

math.log10(x)           返回x关于10的对数

math.pow(x,y)           返回x**y

math.sqrt(x)            返回x的平方根

## **3，三角函数**

math.sin(x)             返回x的正弦，x用弧度制表示

math.cos(x)             返回x的余弦

math.tan(x)             返回x的正切

math.asin(x)            返回x的反正弦，结果用弧度制表示

math.acos(x)            返回x的反余弦

math.atan(x)            返回x的反正切

math.atan2(y,x)         返回atan(y/x)

math.hypot(x,y)         返回sqrt(x*x + y*y)

## **4，角度，弧度转换函数**

math.degrees(x)         弧度 –> 角度

math.radians(x)         角度 -> 弧度

## **5，双曲线函数**

math.acosh(x)           返回x的反双曲余弦

math.asinh(x)           返回x的反双曲正弦

math.atanh(x)           返回x的反双曲正切

math.cosh(x)            返回x的双曲余弦

math.sinh(x)            返回x的双曲正弦

math.tanh(x)            返回x的双曲正切
# MarkDown使用手册

# Welcome to Leanote! 欢迎来到Leanote!

## 1. 排版

**粗体** *斜体*
**粗体这样的一段话是可以的**

 ~~这是一段需要删除的东西~~
~~这是一段错误的文本。~~

引用:
> 引用这个好像挺简单的

> 引用Leanote官方的话, 为什么要做Leanote, 原因是...

 有序号的列表: 
 1.列表1很好
 2.列表の很好
 4.列表可以需要
 5.诶扥扫繁森


有充列表:

  1. 支持Vim
  2. 支持Emacs

无序列表:

 - 项目1
 - 项目2


无序列表:

 - 项目1
 - 项目2


## 2. 图片与链接

图片:
![leanote](http://leanote.com/images/logo/leanote_icon_blue.png)
链接:
[这是百度的连接](http://www.baidu.com) 
[这是去往Leanote官方博客的链接](http://leanote.leanote.com)

## 3. 标题

以下是各级标题, 最多支持5级标题

```
# h1
## h2
### h3
#### h4
##### h4
###### h5
```

```
我觉得代码一定要写在这里面, 用来做笔记
```

这样的话简直很麻烦


## 4. 代码

示例:

    function get(key) {
        to
    }
     
    function get(key) {
        return m[key];
    }


​    
    function get(key) {
        return m[key];
    }


​    
    def a : 
    {
        print(ln)
    }

代码高亮示例:

``` python
def b: 
   print b
   if b = 10: 
      print a
   elif: 
      c = a
```

``` javascript
    df 
```

``` python
    so
```

``` javascript
/**
* nth element in the fibonacci series.
* @param n >= 0
* @return the nth element, >= 0.
*/
function fib(n) {
  var a = 1, b = 1;
  var tmp;
  while (--n >= 0) {
    tmp = a;
    a += b;
    b = tmp;
  }
  return a;
}
 
document.write(fib(10));
```

```python
class Employee:
   empCount = 0
 
   def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1
```

# 5. Markdown 扩展

Markdown 扩展支持:

* 表格
* 定义型列表
* Html 标签
* 脚注
* todo list
* 目录
* 时序图与流程图
* MathJax 公式

## 5.1 表格

| 列名 |     星期 |     有一些 |
| ---- | -------: | ---------: |
| 课程 |     数学 | 认可度更高 |
| 课程 |     语文 | 北方那几个 |
| 年龄 | 教育机构 |     甲骨文 |


| Item     | Value  |
| -------- | ------ |
| Computer | \$1600 |
| Phone    | \$12   |
| Pipe     | \$1    |

可以指定对齐方式, 如Item列左对齐, Value列右对齐, Qty列居中对齐

| Item     |  Value | Qty  |
| :------- | -----: | :--: |
| Computer | \$1600 |  5   |
| Phone    |   \$12 |  12  |
| Pipe     |    \$1 | 234  |


## 5.2 定义型列表

名词 1
:   定义 1（左侧有一个可见的冒号和四个不可见的空格）

测试1
:  我们知道这是什么吗
然后呢这个到底是什么
我测试一下

代码块 2
:    这是代码块的定义（左侧有一个可见的冒号和四个不可见的空格）

        代码块（左侧有八个不可见的空格）

所以我想再试试
:   我看到的这个是定义

        因为我学的太杂old的

代码块 2
:   这是代码块的定义（左侧有一个可见的冒号和四个不可见的空格）

        代码块（左侧有八个不可见的空格）


## 5.3 Html 标签

支持在 Markdown 语法中嵌套 Html 标签，譬如，你可以用 Html 写一个纵跨两行的表格：

    <table>
        <tr>
            <th rowspan="2">值班人员</th>
            <th>星期一</th>
            <th>星期二</th>
            <th>星期三</th>
        </tr>
        <tr>
            <td>李强</td>
            <td>张明</td>
            <td>王平</td>
        </tr>
    </table>
     
    <table>
        <tr>
            <th rowspan="2">值班人员</th>


<table>
    <tr>
        <th rowspan="2">我是cda</th>
        <th>星期一</th>
        <th>星期二</th>
        
    </tr>
    <tr>
       
        <th>星期一</th>
        <th>喜庆而</th>
    </tr>
    <tr>
     <th rowspan="3">我是ddd</th>
        <th>所以说</tr>
        <th>到底是什么</th>
    
    </tr>

</table>


<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

**提示**, 如果想对图片的宽度和高度进行控制, 你也可以通过img标签, 如:

<img src="http://leanote.com/images/logo/leanote_icon_blue.png" width="50px" />

## 5.4 脚注

杨悦[^footnote]来创建一个脚注
[^footnote]: 18946351860是一款强大的开源云笔记产品.

Leanote[^footnote]来创建一个脚注
[^footnote]: Leanote是一款强大的开源云笔记产品.

## 5.5 todo list


Leanote 近期任务安排:


说别人傻

- [] 怎么可能啊
- [] 我们寝室全考试
- [] 我们

你不好怎么knee个

- [] 所以说为什么必须用空格
- [] 还是搞不懂
- [] 这样还是简单, 狗狗就知道了
- 
------------------
- [] bbs 维护
- [] Desktop 发布新版
    - [] Markdown编辑器添加Todo list
    - [x] 修复白屏问题
    - [ ] 修复issue3
- [ ] Leanote 维护
    - [ ] 修复issue4

## 5.6 目录
[TOC]



通过 `[TOC]` 在文档中插入目录, 如:

[TOC]

## 5.7 时序图与流程图

```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

流程图:

```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?
 
st->op->cond
cond(yes)->e
cond(no)->op
```

> **提示:** 更多关于时序图与流程图的语法请参考:

> - [时序图语法](http://bramp.github.io/js-sequence-diagrams/)
> - [流程图语法](http://adrai.github.io/flowchart.js)

## 5.8 MathJax 公式

$ 表示行内公式： 

质能守恒方程可以用一个很简洁的方程式 $E=mc^2$ 来表达。

$$ 表示整行公式：

$$\sum_{i=1}^n a_i=0$$

$$\sum_{i=4}^{n+1} a_k=0$$

所以说这些公式也是一行啊

$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$

$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

更复杂的公式:
$$
\begin{eqnarray}
\vec\nabla \times (\vec\nabla f) & = & 0  \cdots\cdots梯度场必是无旋场\\
\vec\nabla \cdot(\vec\nabla \times \vec F) & = & 0\cdots\cdots旋度场必是无散场\\
\vec\nabla \cdot (\vec\nabla f) & = & {\vec\nabla}^2f\\
\vec\nabla \times(\vec\nabla \times \vec F) & = & \vec\nabla(\vec\nabla \cdot \vec F) - {\vec\nabla}^2 \vec F\\
\end{eqnarray}
$$

访问 [MathJax](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) 参考更多使用方法。
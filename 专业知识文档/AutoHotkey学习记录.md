# AutoHotkey 学习记录

第一节课学习了安装新的版本, 这个版本的中文文档非常好用, 大大减少了我读英文文档理解不透彻的问题, 这个就已经帮助很大了.

第二节课: 学习了查看帮助文档和调试两大功能, 利用编辑器, 只要输入一个函数会自动弹出需要参数, 而且在函数上面输入F1键, 就可以自动在帮助文档中搜索对应的函数, 里面的解说基本上已经很详细了.
调试的话有一个直接运行F5, 也有dubug功能, 在debug中可以设置断点, 和eclipse是一样的, 观察代码运行情况, 变量的list也在旁边有功能.

第三节课: 学习了简单的热键和热字符串触发设定操作, 这个我以前基本上就已经会了, 但是在对应窗口中的操作还不太会.
现在终于搞明白了, 具体实现代码如下

```javascript
#IfWinActive ahk_class Notepad++
::abc::
MsgBox , zhehineirong
return
#IfWinActive
```

在窗口监视器中, 显示出来的有窗口标题(Window Title)和窗口类(Class)以及程序(Process)
我刚开始只是复制了自己以为的那一部分, 其实应该不管有什么奇怪的代码全部复制下来就对了
`*new 2 - Notepad++ [Administrator]
ahk_class Notepad++
ahk_exe notepad++.exe`
例如下面这个, `*new 2 - Notepad++ [Administrator]`就应该全部复制下来
`ahk_class Notepad++`也是全部复制下来, 这几个对应的范围分别更大, 具体多大很好分辨.

**这样我以后就可以很好的对应不同的窗口进行编辑了**



## 循环结构, 有三个

### for

只针对对象

对象的定义方法

```python
Banana := {"Shape": "Elongated", "Color": "Yellow", "Taste": "Delicious", "Price": 3}

k := Banana["Shape"]

for k, v in Banana{
	s .= k "=" v "`n"
	MsgBox, % s
}
```

> **注: 在AHK中转移符用 `表示  `\`n 表示换行符**

### loop

可以直接指定循环次数, 也可以用内置变量 a_index , 这个变量每次循环加1

```python
Loop, 3
{
    MsgBox, Iteration number is %A_Index%.  ; A_Index 将为 1, 2, 接着 3
    Sleep, 100
}

Loop
{
    if a_index > 25
        break  ; 终止循环
    if a_index < 20
        continue ; 跳过后面并开始下一次重复
    MsgBox, a_index = %a_index% ; 这里将仅显示数字 20 到 25
}
```

### while

# 错误整理

今天在编辑代码的时候出错

第一个错误, `return `刚开始用的是大写`Return`, 结果不能正常返回

```python
#If  WinActive("ahk_exe 360se.exe") or  WinActive("ahk_exe chrome.exe") 
{
; 这部分是用于在360浏览器中关闭括号自动补全, 主要是为了使用Jupyter notebook, 已经自带的括号补全功能.
; 关闭括号自动补全
~$(::

~$[::

~${::

~$"::

~$'::
return
}
```

第二个错误, ahk_exe Typora.exe 忘了加引号

```python
#if WinActive ("ahk_exe Typora.exe")
{
;这部分是用于markdown 中快速输入python代码
::``::````python{Enter}
return
}
```


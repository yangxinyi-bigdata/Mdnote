# AutoHotkey 学习记录

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


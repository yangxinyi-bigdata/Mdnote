# AHK学习记录

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
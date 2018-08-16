# Anaconda

## 为什么要使用Anaconda

### Anaconda介绍

如果单独使用Python解释器: 大量第三方的库之间有相互的依赖关系, 管理起来会非常复杂.
Anaconda是一个开源的Python发行版本，其包含了conda、Python之外的180多个科学包及其依赖项。
为了管理方便, 我们使用Anaconda作为平时使用的集成环境

### Anaconda安装

官网下载64位Python3, 可能要配置环境变量

Conda可作为管理数据分析各类包的主要工具.

| conda管理包语句：                   |                        |
| ----------------------------------- | ---------------------- |
| conda update conda                  | 升级conda到当前的版本  |
| conda install somepackage           | 安装某个包             |
| conda uninstall(remove) somepackage | 卸载某个包             |
| conda list                          | 查看安装过的包         |
| conda search somepackage            | 在Anaconda上查找某个包 |
| conda update --all                  | 升级所有的安装的包     |
| conda -h                            | 查看帮助文档           |

完整conda命令集: H:\FangCloudV2\个人文件\大数据资料同步公司端\Markdown笔记\conda 命令集.md

更改conda安装源--清华大学
`conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/`

`conda config --set show_channel_urls yes`

## Jupyter notebook

### 设置Jupyter notebook默认工作目录

打开Windows的cmd，在cmd中输入jupyter notebook --generate-config

1. 可以看到jupyter_notebook_config.py文件路径 
2. 找到此文件打开 
3. 在文件中搜索内容c.NotebookApp.notebook_dir 
4. 把前边的井号键删除 
5. 在引号中填入自己的工作目录 例: D:\pycode

### Markdown简单介绍,操作界面, 快捷键类似Vim

具体在Jupyter notebook中讲解

### 更换Jupyter notebook主题

#### 更换主题原因

​    使用jupyter notebook的默认主题久了容易产生视觉疲劳
    且白色背景对眼睛伤害更大
    所以修改主题还是必要的

#### 安装代码

​    pip install --upgrade jupyterthemes

#### 更换主题方法

##### 查看主题

 jt -l

##### 查看都有什么主题

​      chesterish
      grade3
      gruvboxd
      gruvboxl
      monokai
      oceans16
      onedork
      solarizedd
      solarizedl

##### 修改主题

​      jt -t 主题名 -N -T 
        -N 是header -T 是toolbar


#### Jupyter notebook 中添加目录插件

原因:  notebook的内容多了以后，经常会需要往前查看内容， 但鼠标滑动寻找内容太容易眼花且效率很低。

我们可以通过添加目录插件来帮助我们提高效率

安装 jupyter_contrib_nbextensions
`pip install jupyter_contrib_nbextensions`
配置 nbextension
`jupyter contrib nbextension install --user`

启动jupyter notebook

选择 Nbextensions

勾选 Table of Contents

可选: 

##### Collapsible headings插件

将标题内部的内容全部折叠起来

##### Code folding

代码折叠插件,允许你将缩进内容折叠起来,节省屏幕空间

#### jupyter notebook 重要配置

这个可以让jupyter对所有内部行都能够输出结果
from IPython.core.interactiveshell import InteractiveShell InteractiveShell.ast_node_interactivity = "all"
Options: 'all', 'last', 'last_expr', 'none', 'last_expr_or_assign' Default: 'last_expr'
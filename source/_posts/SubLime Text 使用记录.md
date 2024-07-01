---
title: SubLime Text 使用记录
date: 2022-01-10T09:10:57.000Z
updated: '2024-07-01 21:24:59'
categories:
  - 笔记
tags:
  - Tools
---
> 参考文档[https://blog.csdn.net/qq_19520193/article/details/123434266](https://blog.csdn.net/qq_19520193/article/details/123434266)


## 安装与破解
官网：[http://www.sublimetext.com/](http://www.sublimetext.com/)
Win + R 运行 sysdm.cpl  打开系统属性 配置环境变量
编辑 “Path”，增加 Sublime Text 的安装目录（例如 D:\Program Files\Sublime Text 3）
这是为了能在软件里使用 subl命令

## 插件集合
> Package Control 官网[https://packagecontrol.io/](https://packagecontrol.io/)


## Package Control
> 非常简单

Open Sublime Text and go to Toools > Install Package Control 

## 汉化
> 不过不需要

![image.png](/images/56892bf04c2b2bc00e9d27667f4e0602.png)

## Anaconda(配python 的插件)
> Anaconda插件可以检查你的代码是否书写错误符合、是否满足PEP8规范，有效提高开发效率。
如果代码行出现白线，点击该行就能在左下角查找问题原

```json
{"anaconda_linting":false}
```

## 打开终端插件 Terminus
> 非常简单

Open Sublime Text and go to Toools > Install Package Control 

#### 设定打开终端
Preferences > Package settings> Terminus > command palette
```json
[
   {
        "caption": "Terminal (panel)",
        "command": "terminus_open",
        "args"   : {
           "cmd": "bash",
           "cwd": "${file_path:${folder}}",
           "title": "Command Prompt",
           "panel_name": "Terminus"
        }
   },
]
//windows 换成 cmd.exe
```

#### 设定快捷键
Preferences >  Package settings> Terminus > key Bindings
```json
[
   {
       "keys": ["alt+1"],
       "command": "terminus_open",
       "args" : {
           "cmd": "bash",
           "cwd": "${file_path:${folder}}",
           "panel_name": "Terminus"
       }
   }
]
//windows 换成 cmd.exe
```

## diff 插件
> diffy
> 地址：[https://packagecontrol.io/packages/Diffy](https://packagecontrol.io/packages/Diffy)


## 常用快捷

## 界面操作
| Ctrl+Shift+P  | 打开命令面板（Esc退出） |
| --- | --- |
| Ctrl + `  | 打开Sublime Text控制台（Esc退出） |
| Ctrl + K, Ctrl + B | 组合键，显示或隐藏侧栏 |
| Alt | 光标调到菜单栏 |


## 文档编辑
| Ctr+Shift+D | 复制粘贴光标所在行 |
| --- | --- |
| Ctrl+/ | 用//注释当前行。 |
| Ctrl+Shift+/： | 用/**/注释。 |
| Ctrl + Shift + Enter |  在当前行上面增加一行并跳至该行 |
| Ctrl + ←/→ | 进行逐词移动 |
| Ctrl + Shift + ←/→ | 进行逐词选择 |
| Ctrl + Shift + ↑/↓ |  移动当前行（文件会被修改） |
| Ctrl+KK  | 从光标处删除至行尾 |
| Ctrl+K Backspace | 从光标处删除至行首 |
| Ctrl+Z | 撤销 |
| Ctrl+Y | 恢复撤销 |
| Ctrl+ Shift+J | 合并行（已选择需要合并的多行时） |
| 选中 |  |
| Ctrl+ D | 选中所有相同的词 |
| Ctrl+L | 选择光标所在整行 |
| Ctrl+X | 删除光标所在行 |
| 跳转 |  |
| Ctrl + H | 调出替换框进行替换 |
| Ctrl + F |  调出搜索框 |
| F12 | jump to Definition |
| Ctrl + G | 输入行号以跳转到指定行 |
| Ctrl+M | 跳转到括号另一半。 |
| 窗口 |  |
| Ctrl + N | 当前窗口创建一个新标签 |
| Ctrl + Shift + N |  创建一个新窗口 |
| Ctrl + W | 关闭标签页,如果没有标签页了，则关闭该窗口 |
| Ctrl+Shift+W | 关闭所有打开文件 |
| 屏幕 |  |
| F11 | 切换普通全屏 |
| Shift + F11 | 切换无干扰全屏 |
| Alt + Shift + 2 | 进行左右分屏 |
| Alt + Shift + 8 | 进行上下分屏 |
| Alt + Shift + 5 | 进行上下左右分屏（即分为四屏） |
| Ctrl + 数字键 |  跳转到指定屏 |
| Ctrl + Shift + 数字键 | 将当前屏移动到指定屏 |


## Python语言环境
> 本机已配置了Python，只需在Sublime 上安装辅助插件
> 参照[https://www.yuque.com/u22436206/mw1d9v/bwti6b7wahrrs8n3#dDKPk](#dDKPk)


## 设定用户配置
Preferences-> Package settings-> 

## C++ 配置
> 安装MinGW 官网：


## 控制台乱码解决
```json
{
    "cmd": ["g++", "-Wall", "${file}", "-fexec-charset=gbk", "-o","${file_path}/${file_base_name}"],
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:?(.*)$",
    "working_dir": "${file_path}",
    "selector": "source.c",
    "variants":
    [
        {
            "name": "Run",
            "cmd": ["cmd","/C","g++", "-Wall", "${file}", "-fexec-charset=gbk", "-o","${file_path}/${file_base_name}", "&&","start","cmd","/c", "${file_path}/${file_base_name} & pause"]
        }
    ]
}
```




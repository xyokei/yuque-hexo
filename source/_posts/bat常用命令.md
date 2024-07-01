---
title: bat常用命令
date: 2021-10-16T12:12:57.000Z
updated: '2024-07-01 20:17:47'
categories:
  - windows
tags:
  - bat
---
[命令]/? 会打出 该命令如何使用及其参数

## 基础命令

1. **echo:**
```bash
echo [{on|off}] [message]
#eg:
echo off  #关闭回显，后面所有命令不再显示
echo hello #打开回显，显示hello
```

2. **@:**

加在当前行头，当前行命令不显示，仅影响当前行

3. **goto：**
```bash
#eg:
if{...} =={} goto xxx
:xxx
# xxx 标签名，前面加:表示这个是标签
```

4. **Rem:**
```bash
#注释
Rem Message
```

5. **Pause**

   按任意键继续，相当于阻塞当前执行过程
```bash
pause > nul #将pause指向nul，即屏蔽回显输出
```

6. **Call**

从一个批处理程序调用另一个批处理程序，且父批处理程序不会终止
作用域：只在脚本或批处理文件中该命令有效，其余无效
**参数**

-    BatchParameters 批处理参数：从 %0 到 %9 或者变量
-    arguments:参数：从 %0 到 %9 或者变量
-    call [[Drive:][Path] FileName [BatchParameters]] [:label [arguments]]  
-    call d: \test\test1\filename  %1 :label

与goto调用label不同，call会回调到当前语句，goto调转后就不会再回到当前语句
变量适用范围：同一进程，变量互通

7. **start**

启动脚本及调用外部程序
作用域：在cmd、脚本、批处理文件下有效，即全范围
 相当于新启动一个脚本的操作，创建了另一个进程
 变量适用范围：同一进程，单向传值，

8. **if**
```bash
#eg: 
if"xx" == "a" format a:
if{xx} == {a} goto xxx
if [not] exist [path] edit xxx
if [not] errorlevel number (xxx) else if [not] errorlevel number
errorlevel  #称错误码或者返回码，通常dos程序运行结束会返回一个数字值表示运行状态
if 条件 （执行） else (执行)
```

9. **比较运算符**
```bash
EQU - 等于 常用 (==)
NEQ - 不等于 （if not 1 == 1. 没有!=）
LSS - 小于
LEQ - 小于或等于
GTR - 大于
GEQ - 大于或等于
```

10. **choice **

此命令为dos或者Windows外提供的外部命令
可以让用户输入一个字符串，从而可以运行不同的命令
```bash
#eg: 
choice /c: dme defrag,mem,end 
# /c:后接提示输入的字符，无空格，
```

11. **for**

三个关键字，for  in  do 
in和do之间的command表示的字符串或变量一个或多个，每个字符串或变量，即为一个元素，
每个元素之间，用空格键、跳格键、逗号、分号或等号分隔

- 通用

    默认文件 `for{%var | %%var} in (set) do command [command-parrameters]`
    **var** 变量，单一字母，变量名称是区分大小写的
>     注意：指定变量推荐用%% 而不是%

   **(set)**一个或一组文件，可为通配符

-  当命令扩展名被启用

     `for /d %var in (set) do command [command-parameters]`
     **/d** 之遍历子文件夹
    `for /r [[drive:]path] %var in (set) do command [command-paremeters]`
     **/r** 需要加上指定path ，深度遍历path下的文件包括子文件夹下的文件
    `for /L %var in (start,step,end) do command [command-paremeters]`
     (start,step,end) 以**step增量方式从start -> end** 
     eg: (1,1,5) (1 2 3 4 5)   (5,-1,1) (5 4 3 2 1)
```bash
for /f ["options"]  :解析文本专用 for循环每一行
#参数介绍
    options:
             delims=xx (以xx为被处理字符串的分割字符，但默认只提取第一次出现xx的符串) ,未指定delims=，则默认以空格或跳格作为分隔符 
              xx为符号列表
             eg: delims=,  以逗号为分割符， delims=., 以.或者，为截取字符
             定点提取：tokens=
             tokens=2  --提取第二节符串                      用1个变量接受
             tokens=2,5  --提取第二和第五节符串          用2个变量接受
             token=2-5,8 --提取2至5和第8节符串         用5个变量接受
             token=2,*     --提取第2节符串和2以后的所有符串，不同的是，2以后所有符串被拼接成一个字符串，共用2个变量接收
             变量顺序遵循字母顺序，即第一个接受变量字母决定了，后续所有接受变量的字母都决定了
#for循环里利用goto实现continue和break
```

12. **dir**
```bash
 dir /s /b /ad [path]   
```
/b使用空格式(没有标题信息或摘要)
/a显示具有指定属性的文件./ad表示显示目录
(属性d目录,r只读文件,h隐藏文件,a准备存档的文件,s系统文件,-表示否的前缀)
/s显示指定目录或要有子目录中的文件

## 其他命令

1. ping
2. telnet 使用前需按照
3. random随机数
4. exit 结束程序
5. shutdown -s 关机

## 字符串处理
%元字符:~起始位置，截取长度%（起始值从0开始，截取长度可选，省略逗号和截取长度，将截到结尾，截取长度<0倒数位截）

- **替换：**
```bash
set a="abcd1234"
echo %a% 显示："abcd1234"
set a=%a:1=kk% 替换“1”为“kk”
echo %a% 显示："abcdkk234"
```

- **合并：**
```bash
set str1 = %str1%%str2%
```

- **计算字符串长度**

用循环，判断字符串是否为空，为空退出，不为空，截掉一个字符 num++

## 注册表相关

1. **名称** 远程机器只有HKLM和HKU
```bash
HKCR： HKEY_CLASSES_ROOT
HKCU： HKEY_CURRENT_USER
HKLM： HKEY_LOCAL_MACHINE
HKU： HKEY_USERS
HKCC： HKEY_CURRENT_CONFIG
```
**返回值 0成功1失败**

2. **复制**
```bash
reg copy KeyName1 KeyName2 [/s] [/f]
#/s 复制指定子项下的所有子项和项。
```

3. **备份**
```bash
reg export KeyName FileName #仅支持本地操作
```

4. **导入**
```bash
reg import FileName
#添加内容 语法
reg add KeyName [/v EntryName|/ve] [/t DataType] [/s separator] [/d value] [/f]
reg add "file全path" /v 名称 /t 类型 /d “值” /f （/f->无需请求确认）
#值中有复杂符号 则 /d \"值\"
#/ve  指定添加到注册表中的项为空值。
```

5. **删除**
```bash
reg delete KeyName [{/v EntryName|/ve|/va}] [/f]
#/ve  指定只可以删除为空值的项。
#/va  删除指定子项下的所有项。使用本参数不能删除指定子项下的子项。
#修改后结束并刷新，可使修改内容生效 ,重启资源管理器
taskkill /f /im explorer.exe >nul
start "" "explorer.exe" 
```

## 系统服务
```bash
net stop 服务名
net start 服务名
sc config 服务名 start =auto    设置自动
sc config 服务名 start =demand   设置手动
sc config 服务名 start =disabled   设置禁用

#是否启用命令扩展
setlocal {enableextension | disableextensions} {enabledelayedexpansion | disabledelayedexpansion}
#变量延迟用 !a! 一对感叹号包起来
```

## 文件处理

1. **删除**
```bash
del /a /s /q /f d:\test\a.bat
#/f  强制删除只读文件。
#/s  从所有子目录删除指定文件。
#/q  安静模式。删除全局通配符时，不要求确认。
#/a  根据属性选择要删除的文件
rmdir /q /s d:\test\logs #删除该目录所有文件及其目录
```
**/AR、/AH、/AS、/AA**分别表示删除**只读、隐藏、系统、存档文件**，/A-R、/A-H、/A-S、 /A-A表示删除除只读、隐藏、系统、存档以外的文件

2. **创建**
```bash
#语法 
md[drive:]path
#语法：
xcopy a\* b /y /e /i /q
#/y：不弹出“确认是否覆写已存在目标文件”的提示
#/e：复制文件及子文件夹内所有内容，包括空文件夹（对比/s, /s不复制空文件夹）
#/i：如果b不存在并且复制超过一个文件则默认b是目录名
#/q：quiet，静默模式
```

## 提示符号
```bash
%cd% 当前执行的命令
start "" "..exe" 第一个参数是窗口显示的标题
copy  c:\*.* d:\back
```

1. **符号(@)** 

**@**在批处理中的意思是关闭当前行的回显。

2. ** 符号(>) **

**>**的意思是传递并覆盖。

3. **符号(>>) **

符号>>的作用与符号>相似，但他们的区别在于>>是传递并在文件末尾追加>>也可将回显传递给控制台。

4. **符号(|) **

**|**是一个管道传输命令意思是将上一命令执行的结果传递给下一命令去处理。

5. **符号(^) **

**^** 是对特殊符号 > 、<、 &、的前导字符。在命令中他将以上的3个符号的特殊动能去掉仅仅只吧他们当成符号而不使用他们的特殊意义。

6. **符号(&) **
&符号允许在一行中使用2个以上不同的命令，当第一个命令执行失败将不影响第2个命令的执行。
7. **符号(&&) **

&&符号也是允许在一行中使用2个以上不同的命令，当第一个命令执行失败后后续的命令将不会再被执行。

8. **符号(" ") **
" "符号允许在字符串中包含空格。进入一个特殊的目录可以用如下方法
```bash
c:\>cd “Program Files”
```

9. **符号（,）** 
**,**符号相当于空格。在某些特殊的情况下可以用,来代替空格使用。

c:\>dir,c:\ 

10.  **符号(;) **
;符号当命令相同的时候可以将不同的目标用;隔离开来但执行效果不变。如执行过程中发生错误则只返回错误报告但程序还是会继续执行。例： 
```bash
DIR C:\;D:\;E:\F:\ 
```

11. 其他 
```bash
set /p a=按回车键退出
cls 清屏
control.exe /name Microsoft.ProgramsAndFeatures #打开程序和功能
```

## 中文乱码
```bash
#把bat文件的编码改为ANSI，UTF-8在win10我这儿会中文显示乱码
rem for %%a in (copyfrom\*) do echo %a%
```

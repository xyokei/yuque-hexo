---
title: MarkDown常用
date: 2021-02-19T12:12:57.000Z
updated: '2024-07-01 21:21:21'
categories:
  - 笔记
tags:
  - Tools
---

## 标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题
```markdown
# 一级标题 
## 二级标题
### 三级标题
#### 四级标题
*斜体文本*
_斜体文本_
**粗体文本**
__粗体文本__
***粗斜体文本***
___粗斜体文本___

分割线

---

<u>下划线<u>
<mark>背景高亮</mark>

行内代码
`html`

<font color=Tomato size=5>浅红</font>
<font color=red size=5>红</font>
<font color=DarkViolet size=5>紫</font>
<font color=Blue size=5>绿</font>
<font color=DeepSkyBlue size=5>天空绿</font>

---
## 自查 颜色
<font color=#00ffff size=4>color=#00ffff size=4</font>
<font color=gray size=5>color=gray size=5</font>
<font color=maroon size=5>color=gray size=5</font>
<font color=grey size=5>color=gray size=5</font>
<font color=silver size=5>color=gray size=5</font>
<font color=lightgrey size=5>color=gray size=5</font>
<font color=HotPink size=5>color=gray size=5</font>
<font color=DeepPink size=5>color=gray size=5</font>
<font color=VioletRed size=5>color=gray size=5</font>
<font color=Purple size=5>color=gray size=5</font>
<font color=navy size=5>color=gray size=5</font>
<font color=Blue size=5>color=gray size=5</font>
<font color=DeepSkyBlue size=5>color=gray size=5</font>
<font color=LightSkyBlue size=5>color=gray size=5</font>
<font color=aqua size=5>color=gray size=5</font>
<font color=DarkTurquoise（#00CED1） size=5>color=gray size=5</font>
<font color=LightSeaGreen size=5>color=gray size=5</font>
<font color=YellowGreen size=5>color=gray size=5</font>
<font color=LawnGreen size=5>color=gray size=5</font>
<font color=GreenYellow size=5>color=gray size=5</font>
<font color=Yellow size=5>color=gray size=5</font>
<font color=Tomato size=5>color=gray size=5</font>
<font color=red size=5>color=gray size=5</font>
<font color=fuchsia size=5>color=gray size=5</font>
<font color=MediumOrchid size=5>color=gray size=5</font>
<font color=DarkViolet size=5>color=gray size=5</font>
```

## 文本
一般
~~删除~~
_斜体 Italic 中文好像没用_
**加粗 bold**
标记 monospace
带下划线的文本
```markdown
一般  
~~删除~~  
*斜体 Italic 中文好像没用*  
**加粗 bold**  
`标记 monospace`  
<u>带下划线的文本</u>
```

## 改行

- 行末加两个空格

## 列表
**无序列表**

- this one
- that one
- the other one

**有序列表**

1. first item
2. second item
3. third item

**嵌套列表**

1. First, get these ingredients:
   - carrots
   - celery
   - lentils
2. Boil some water.
3. Dump everything in the pot and follow
this algorithm:
   - find wooden spoon
   - manage pot
      - uncover pot
      - stir
      - cover pot
      - balance wooden spoon precariously on pot handle
   - wait 10 minutes
   - goto first step (or shut off burner when done)
4. Do not bump wooden spoon or it will fall.
```markdown
**无序列表**

* this one
* that one
* the other one

**有序列表**

1. first item
2. second item
3. third item

**嵌套列表**

1. First, get these ingredients:
   * carrots
   * celery
   * lentils
2. Boil some water.
3. Dump everything in the pot and follow  
   this algorithm:
   * find wooden spoon
   * manage pot
      * uncover pot  
      * stir  
      * cover pot  
      * balance wooden spoon precariously on pot handle  
   * wait 10 minutes
   * goto first step (or shut off burner when done)

* Do not bump wooden spoon or it will fall.
```

## 表格

- 语雀 的显示无表头，markdown有
| size | material | color |
| --- | --- | --- |
| 9 | leather | brown |
| 10 | hemp canvas | natural |
| 11 | glass | transparent |

**指定对齐方式**

| Item | Description | Value |
| --- | --- | --- |
| Computer
PC | Desktop PC | $1600 |
| Phone | iPhone 5s | $12 |
| Pipe | Steel Pipe | $1 |

**内嵌格式**

| Function name | Description |
| --- | --- |
| `help()` | Display the help window. |
| `destroy()` | **Destroy your computer!** |

```markdown
size | material     | color
---- | ------------ | ------------
9    | leather      | brown
10   | hemp canvas  | natural
11   | glass        | transparent

**指定对齐方式**

| Item      | Description | Value|
|:--------- |:-----------:|-----:|
| Computer<br>PC  | Desktop PC  |$1600 |
| Phone     | iPhone 5s   |  $12 |
| Pipe      | Steel Pipe  |   $1 |

**内嵌格式**

| Function name | Description                    |
| ------------- | ------------------------------ |
| `help()`      | Display the help window.       |
| `destroy()`   | **Destroy your computer!**     |
```

## 块
{% note info %}
这是信息
{% endnote %}
{% note success %}
这是成功
{% endnote %}
{% note warning %}
这是警告
{% endnote %}
{% note danger %}
这是危险
{% endnote %}
{% note default %}
这是提示
{% endnote %}
```markdown
:::info + Enter 这是信息
:::success + Enter 这是成功
:::warning + Enter 这是警告
:::danger + Enter 这是危险
:::tips + Enter 这是提示
```

## 引用
> 这是引用
> 
##### `引用可内嵌其他格式`

```markdown
> 这是引用  
> ##### `引用块可内嵌其他格式`
```

## 代码块
| Language | Code |
| --- | --- |
| Apache | apache, apacheconf |
| Bash | bash, sh, zsh |
| C# | csharp, cs |
| C | c, h |
| C++ | cpp, hpp, cc, hh, c++, h++, cxx, hxx |
| CoffeeScript | coffeescript, coffee, cson, iced |
| Diff | diff, patch |
| Go | go, golang |
| HTML, XML | xml, html, xhtml, rss, atom, xjb, xsd, xsl, plist, svg |
| HTTP | http, https |
| JSON | json |
| Java | java, jsp |
| JavaScript | javascript, js, jsx |
| Kotlin | kotlin, kt |
| Less | less |
| Lua | lua |
| Makefile | makefile, mk, mak, make |
| Markdown | markdown, md, mkdown, mkd |
| Nginx | nginx, nginxconf |
| Objective C | objectivec, mm, objc, obj-c, obj-c++, objective-c++ |
| PHP | php, php3, php4, php5, php6, php7, php8 |
| Plaintext | plaintext, txt, text |
| Python | python, py, gyp |
| Python REPL | python-repl, pycon |
| R | r |
| Ruby | ruby, rb, gemspec, podspec, thor, irb |
| Rust | rust, rs |
| SCSS | scss |
| SQL | sql |
| Shell | shell, console |
| TypeScript | typescript, ts |
| VB.Net | vbnet, vb |
| YAML | yml, yaml |


## 链接
[百度](www.baidu.com)
```markdown
[百度](www.baidu.com) + space
![图片](地址) + space
```


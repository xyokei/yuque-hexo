---
title: Hexo 小记
date: 2022-10-08T11:12:57.000Z
updated: '2024-07-01 21:35:12'
categories:
  - Hexo
tags:
  - Hexo
---
Hexo搭建的各种小问题，记录一下
<!-- more -->

##  安装环境

### 1.1 必备软件

- node
- npm 
- git
```bash
rem 测试
git version
node -v
npm -v
cnpm -v
```

### 1.2 更改安装模块位置
```powershell
#更改安装位置
npm config set prefix "E:\develop\nodejs\node_global\node_modules"
npm config set cache "E:\nodejs\node_cache"

#运行 npm list -global 查看
npm list -global
```


## hexo 新手版

### 2.1 创建仓库
仓库名必须 和符合 用户名.github.io 不然最后会404

### 2.2 本地拉hexo
```bash
npm install -g hexo-cli
npm install hexo-deployer-git

ssh-keygen -t rsa -C 'autumnsigni@gmail.com'
ssh-keygen -f hexo-deploy-key -t rsa -C "autumnsigni@gmail.com"
# 验证秘钥是否配置成功
ssh -T git@github.com 
```

### 2.3 git账户设置

- 设置关联账号
```bash
git config --global user.name "xyokei"
git config --global user.email "autumnsigni@gmail.com"
```

- 生成秘钥
```
ssh-keygen -t rsa -C 'autumnsigni@gmail.com'
```

- 关联远程仓库
```bash
#安装hexo
npm install -g hexo-cli
#创建blog源
hexo init

#关联仓库
git init
git remote add origin git@github.com:xyokei/blog_source.git
git pull --rebase origin master #会报找不到分支错 不用管

#提交
git add .
git commit -m "init repo"
git push -u origin master

#解决failed to connect to github.com port 443
git config --global --unset http.proxy

git config --global --unset https.proxy
```

- 生成部署的公钥和私钥
```bash
#blog 源文件下运行 实际随便那里都行
ssh-keygen -f hexo-deploy-key -t rsa -C "autumnsigni@gmail.com"
#公钥放 github.io
#私钥放 私有仓库
```

## hexo + Github Action自动部署版
参考博文
> [https://blog.csdn.net/weixin_45377770/article/details/105228938](https://blog.csdn.net/weixin_45377770/article/details/105228938)

参考博文
> [https://blog.csdn.net/qq_41426117/article/details/108703295](https://blog.csdn.net/qq_41426117/article/details/108703295)
> [https://jeam.org/761615aa](https://jeam.org/761615aa)




- **Action 文件编写**

 	 **在本地生成的博客根目录下找到 .github这个文件夹，在里面创建名为 workflows的文件夹**
**    	 再建一个名为   xxx.yml的文件**
```yaml
# workflow name
name: Hexo Blog CI

# master branch on push, auto run
on: 
push:
branches:
- master

jobs:
build:
runs-on: ubuntu-latest
if: github.event.repository.owner.id == github.event.sender.id

steps:		# 获取博客源码和主题
- name: Checkout source
uses: actions/checkout@v2

- name: Setup Node.js
uses: actions/setup-node@v1
with:
node-version: '14'	# 使用node.js的版本（不要太高）

- name: Setup Deploy Private Key
env:
DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
run: |
mkdir -p ~/.ssh/
echo "$DEPLOY_KEY" > ~/.ssh/id_rsa 
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
ssh-keyscan github.com >> ~/.ssh/known_hosts

- name: Setup Git Infomation
run: | 
git config --global user.name 'xyokei' 
git config --global user.email 'autumnsigni@gmail.com'
npm install hexo-cli -g
npm install
npm install hexo --save
npm install hexo-deployer-git --save
npm install hexo-theme-kratos-rebirth --save
- name: Deploy Hexo 
run: |
hexo clean
hexo generate 
hexo deploy
```

- **再编写本地文件目录下的_config.yml文件**
```yaml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Hexo
subtitle: ''
description: ''
keywords:
author: John Doe
language: en
timezone: ''

# URL
## Set your site url here. 
## For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: http://example.com
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
enable: true # Open external links in new tab
field: site # Apply to the whole site
exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
enable: true
line_number: true
auto_detect: false
tab_replace: ''
wrap: true
hljs: false
prismjs:
enable: false
preprocess: true
line_number: true
tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
path: ''
per_page: 10
order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: kratos-rebirth

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
type: git
repository: git@github.com:xyokei/xyokei.github.io.git
branch: master
#name: xyokei
#email: autumnsigni@gmail.com
```
Action整完后若访问xxx.github.io404，就稍等下，github pages反应慢

## hexo + github Action +  yuque
参考博文
[https://www.yuque.com/1874w/1874.cool/roeayv](https://www.yuque.com/1874w/1874.cool/roeayv)
[https://www.zhwei.cn/hexo-github-actions-yuque/](https://www.zhwei.cn/hexo-github-actions-yuque/)
本来就是喜欢用语雀写东西，这下直接一步到位
github pages 参考这个  [https://www.yuque.com/hxfqg9/web/gtb5ck](https://www.yuque.com/hxfqg9/web/gtb5ck)
官方地址 ： [https://github.com/x-cold/yuque-hexo](https://github.com/x-cold/yuque-hexo)

##  写文档 和 主题

### 添加分类和标签博客参考
[https://juejin.cn/post/6844904081891262471](https://juejin.cn/post/6844904081891262471)
主题参考文档
github仓库 ：[https://github.com/Candinya/Kratos-Rebirth](https://github.com/Candinya/Kratos-Rebirth)

配置文档：

-  hexo 没有空分类，分类是在写文章时设
```bash
hexo new page categories
hexo new page tags
```

- 编写categories 文件夹下的index.md文件
```markdown
---
title: 添加分类
date: 2022-10-12 10:29:52
type: "categories"	
---
```
```markdown
---
title: tags
date: 2022-10-12 11:21:24
type: "tags"
---
```
```markdown
---
title: git学习笔记
date: 2021-01-19 12:12:57
categories: 
- 笔记
- JAVA
tags:
- JAVA
- GIT
---
# 一、理论
## 1.1 思想
```

### 更改站点图标和头像
```markdown
- 带图床的
site_logo: https://ga.candinya.com/avatar/a7f9e15fef26e0a540edc977e21cb3eb
avatarUri: https://ga.candinya.com/avatar/a7f9e15fef26e0a540edc977e21cb3eb

- 使用本地文件
site_logo: /images/avatar.webp
avatarUri: /images/avatar.webp

- 各种配置信息直接参照以下配置文件吧
site_logo: /images/avatar.webp
avatarUri: /images/avatar.webp

topMenu:
  - label: 首页
    icon: home
    url: /
    
  - label: 分类
    icon: bookmark
    url: /categories  
    
  - label: 标签
    icon: tags
    url: /tags

  - label: 归档
    icon: archive
    url: /archives/
    
  - label: 关于我
    icon: user-circle
    url: /about/
    
  - label: 友链
    icon: paw
    url: /friends/
    
  - label: 链接
    icon: link
    submenu:
      - label: 作者博客
        url: https://candinya.com
      - label: 项目链接
        url: https://github.com/Candinya/Kratos-Rebirth

vendors:
  # 常用CDN基地址:
  # https://unpkg.com/
  # https://cdn.jsdelivr.net/npm/

  # 以下一行代码指定 npm cdn 基地址，该 CDN 应该兼容 jsdelivr 使用的路径格式
  # npm_cdn: https://cdn.jsdelivr.net/npm/
  packages:
    hexo-theme-kratos-rebirth: 
      # 以下一行代码用于对某个包单独配置 cdn 基地址，使用 null 表示不从 cdn 获取此包
      # cdn_url: null
      # cdn_url: https://cdn.jsdelivr.net/npm/hexo-theme-kratos-rebirth@latest/
    aplayer:
      # 以下一行代码指定所使用的库版本
      # 默认情况下会使用主题开发组进行开发调试时所使用的版本（通常而言也是最适合的版本）
      # 因此该值不建议修改
      # 
      # 注意：如果选择其他版本，则必须配置 npm_cdn 或 为包单独配置 cdn_url 或 修改 vendors 目录，
      # 这是因为主题自带的库只含有默认版本，必须从 cdn 加载其他版本或由用户手动提供其他版本的相关文件
      #
      # 警告：随意切换版本可能造成主题无法正常工作
      # 
      # version: 1.10.1
      files:
        "dist/APlayer.min.css":
          # 以下代码用于单独对某个文件进行映射，以便于使用不符合 jsdelivr 路径格式的CDN（或其他特殊情形）
          # 使用 null 表示不重定向（默认行为）
          # relocate: null
          # relocate: https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css

          # 以下代码用于指定 SRI 以增强安全性
          # 注意，仅有部分组件支持 SRI，不支持的组件会忽略此值
          # integrity: sha256-6Y7CJDaltoeNgk+ZftgCD9jLgmGv4xKUo8nQ0HgAwVo=

friends:
  list:
  - name: "百度"
    bio: "喵~"
    avatar: "../images/avatar.webp"
    link: "https://baidu.com"

# Footer 页脚配置
group_link: 
contact:
  weibo: 
  mail: 
  telegram: CandyUnion
  twitter: 
  facebook: 
  linkedin: 
  fediverse:
    instance: 
    username: 
  github: xyokei
  rss:
timenotice: 本站已运行
icp: 
psr:

posts:
  donate: true
  comments: 
    provider: gitalk

gitalk:
  clientID: 'GitHub Application Client ID'
  clientSecret: 'GitHub Application Client Secret'
  # repo: 'Kratos-Rebirth-Demo'
  # owner: 'Candinya'
  # admin: 
  #   - 'Candinya'
  # distractionFreeMode: false



# JavaScript 相关配置
## 由于Javascript被压缩后难以有效编辑与生成，因而为了简化操作，将相关的配置项在生成文件时以独立的json文件写出，调用时使用fetch API获取。
jsconfig:
  main:
    cover:
      randomAmount: 20 # 表示从1~20
      baseUrl: null # 使用默认值（不单独配置），将跟随 vendors 中的相关设定选择是否使用 cdn
      filenameTemplate: "thumb_{no}.webp" 
    createTime: "2022/10/12 17:03:12"
    donateBtn: "支持我~"
    scanNotice: "扫一扫，好不好？"
    qr_alipay: "/images/alipayqr.webp"
    qr_wechat: "/images/wechatpayqr.webp"
    siteLeaveEvent: false
    leaveTitle: "{{{(>_<)}}}哦哟，崩溃啦~"
    returnTitle: "(*´∇｀*)欸，又好啦~"
    copyrightNotice: "该内容采用 CC BY-NC-SA 4.0 许可协议，著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。"
    expire_day: 30
    topNavScrollToggle: false
```

### 预备的目录
```markdown
--分类 
|-编程
  |-JAVA
  |-C/C++
  |-PYTHON
  |-C#
  |-PL/SQL
  |-
|- 语言
	|-英语
     |-单词
     |-小句子
     |-文章
  |-日语
|- LeetCode
  |-数据结构
  |-算法
|- 工作
|- 面试
|- 心迹
|- 生活碎片
--标签
|- Linux
|- Shell
|- Windows Bat
|- VMware
|- 笔记
```


## 主题
从Action安装主题 切换到 github 的 主题目录 
目的 ： 想修改下背景图

1. git clone 主题到本地，修该yml后 访问 页面 为空
{% note info %}
原因：thems下文件夹名一定要与config.yml里的名字一致
{% endnote %}

2. themes下source的图片生成不出来
{% note info %}
把主题下的图片移到根目录 下的source里
{% endnote %}

3. 话说我不就想自己 搞图片 的，直接在source里新建图片文件夹感觉就行了，也不用再移动主题了，不想做了，感觉可以

## 选择
目前 yuque的自动同步也可以。本地编辑也可以，唯一的问题是
搭建七牛云图床时 自动上传的图片 文件损坏 ，找不到解决方法了

1. 还是本地手动上传吧 ，然后替换连接，可以用`python` 来搞定
2. yuque只写不带 图片的，比如记一些 学习 的笔记 和代码 之类的

或者直接 在上传后在yuque里插cdn连接

## 问题
问题描述 : 上传图片到七牛云时,图片一直是空白的小方块 
**解决:**

1. 测试
> 想法:应该是取图片的时候,图片损坏了
> 把yuque-hexo 里的util的代码扒出读取图片的部分,开始找

```javascript
'use strict';

const superagent = require('superagent');
const fs =require('fs');

async function img2Buffer(imgUrl) {
  return await new Promise(async function(resolve) {
    try {
      await superagent
        .get(imgUrl)
        .buffer(true)
        .parse(res => {
          const buffer = [];
          res.on('data', chunk => {
            buffer.push(chunk);
          });
          res.on('end', () => {
            const data = Buffer.concat(buffer);
            resolve(data);
          });
        });
    } catch (e) {
      out.warn(`invalid img: ${imgUrl}`);
      resolve(null);
    }
  });
}

async function img2Cdn() {
	const imgBuffer = await img2Buffer('https://cdn.nlark.com/yuque/0/2022/png/22879009/1665974821037-8d78a0bb-f54f-4e24-a8c9-5162b3a52044.png');
	console.log(imgBuffer.toString());
}

img2Cdn();
```
上述代码 运行 `node app.js` 后出现以下问题
![image.png](/images/1593180c156641591b0daa186b8eb759.png)
报了403错,由于superagent不会将4xx何5xx当成错误,所以,就一直往下走了,
试着修改下
```javascript
'use strict';

const superagent = require('superagent');
const fs =require('fs');

async function img2Buffer(imgUrl) {
  return await new Promise(async function(resolve) {
    try {
      await superagent
        .get(imgUrl)
        .set('User-agent','Mozilla 5.10')
        .buffer(true)
        .parse(res => {
          const buffer = [];
          res.on('data', chunk => {
            buffer.push(chunk);
          });
          res.on('end', () => {
            const data = Buffer.concat(buffer);
            resolve(data);
          });
        });
    } catch (e) {
      out.warn(`invalid img: ${imgUrl}`);
      resolve(null);
    }
  });
}

async function img2Cdn() {
	const imgBuffer = await img2Buffer('https://cdn.nlark.com/yuque/0/2022/png/22879009/1665974821037-8d78a0bb-f54f-4e24-a8c9-5162b3a52044.png');
	console.log(imgBuffer.toString());
}

img2Cdn();
```
![image.png](/images/3f7d1d084062a09697719377c57a7884.png)
看样子好像好了，回家测试下

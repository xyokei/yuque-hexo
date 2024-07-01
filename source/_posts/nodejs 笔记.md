---
title: nodejs 笔记
date: 2022-10-12T12:12:57.000Z
updated: '2024-07-01 19:49:01'
categories:
  - Hexo
tags:
  - Hexo
  - nodejs
---
为了改bug学的,随便记记
<!-- more -->

## HelloWorld
> 前提必备 已安装nodejs

1. 创建一个`app.js`文件 写入以下代码
```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

2. 用`node app.js`运行, 访问 [http://localhost:3000](http://localhost:3000) 

---
title: Blog前期准备
date: 2020-06-10T13:10:57.000Z
updated: '2024-07-01 21:07:33'
categories:
  - 项目随笔
tags:
  - Java
  - Springboot
---

## 环境构筑

### IDEA

   1. 自定义maven地址（作用域只到单项目，未全部默认）
   2. Redis 配置，msi一键无脑，修改密码redis.windows-service.conf,   找到requirepass foobared字样，在其后面追加一行，输入requirepass 12345  
   3. mysql 压缩包配置，
```sql
#解压后 配环境变量
#MYSQL_HOME
#%MYSQL_HOME%\bin\
mysqld -install
net start mysql
net stop mysql
```

   4. 


### Visual Code

   1. npm install 下载依赖后直接 npm run dev

## 实践
![](/images/048b20100bb42b2e24dc245f7ce4e2f2.jpeg)

数据模型

![](/images/9ee571355dd586f751f632d9675b2f26.jpeg)
![](/images/f3f818dadeea4ad2323faf7b141d1128.jpeg)
![](/images/16c468c16d38509cd0b3812b924c5a5f.jpeg)

## 问题

1. lombok自动生成数据库对应entity

答：mybatis+lombok逆向工程

2. 做完反思，整体架构，如何设计库表，代码设计思想，业务抽象以及建模
3. 深入细节又能跳出盒子的双重思维，既能够宏观全局又能够微观局部，既能把大的东西分解也能把它集成回去，既能够刨根究底也能够不求甚解，置身产品之中又能够跳出产品，做用户视角的业务思考

业务建模以及业务到技术的分解转化能力，随时都是业务和需求导向，技术是为业务服务的，没有盲目的技术积累，只有合理的技术采用，没有理想化的业务模型，只有验证过的技术
学会分解和理解事物的一种思维

4. 对于响应头字段的各个含义 如vary 等等
5. 分页插件的使用GitHub的PageHelper
6. MarkDownToHtml的插件使用
7. 版本兼容
   1. Springboot 2.6版本 
   2. pageHelper 分页1.4 后解决2.6循环依赖问题
8. 心态崩了
   1. typecho 主题[https://github.com/chakhsu/pinghsu](https://github.com/chakhsu/pinghsu)

## 储备知识

### mybatis
id 主键映射
result 非主键映射
association 对象
collection 集合
行内
property JavaBean的属性名
jdbcType JDBC类型, JDBC类型为CUD操作时列可能为空时进行处理
javaType Java类的全限定名，或别名
typeHandler 数据库与Java类型匹配处理器

### SpringBoot
注解
Repository 对应数据访问层
Mapper 注解待研究
AutoWired弹出警告，三种注入方式？

- Configuration
   1. 作用

用于定义配置类，替代xml文件

   2. 使用注意事项

   3. 底层怎么走的

启动时加载配置类

### 跨域
参考文章
[https://blog.csdn.net/Elsa15/article/details/106737158](https://blog.csdn.net/Elsa15/article/details/106737158)
http请求时 所访问的接口的协议，域名，端口号不是同一个，就会产生跨域问题
前后端分离必然有跨域
浏览器具有同源保护策略，是一种安全机制。
但是在一些情况下，这种安全策略却能成为一种阻碍。
就是我们在做前后端分离的时候就会出现跨域的问题，
前后端分离后，前端和后端就是不同的源，这个时候浏览器就会阻止
前端请求到后端，所以后端就会出现接收不到前端请求的情况，出现403错误


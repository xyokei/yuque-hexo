---
title: SpringBoot
date: 2020-09-10T10:10:57.000Z
updated: '2024-07-01 20:29:12'
categories:
  - Java
tags:
  - SpringBoot
---

## starter启动器
在该项目中执行以下 mvn 命令查看器依赖树。
```
mvn dependency:tree
```
 看包：Spring Boot 导入了 springframework、logging、jackson 以及 Tomcat 等依赖，而这些正是我们在开发 Web 项目时所需要的。

### spring-boot-starter-parent
spring-boot-starter-parent 是所有 Spring Boot 项目的父级依赖，它被称为 Spring Boot 的版本仲裁中心，可以对项目内的部分常用依赖进行统一管理。
```xml
<!--SpringBoot父项目依赖管理-->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.4.5</version>
  <relativePath/>
</parent>
```
![](/images/01fb89b5e4da7d2f911226ae1dedfa17.jpeg)
以上配置中，部分元素说明如下：

- dependencyManagement ：负责管理依赖；
- pluginManagement：负责管理插件；
- properties：负责定义依赖或插件的版本号。

## YAML语法

### YAML 的语法如下：

- 使用缩进表示层级关系。
- 缩进时不允许使用 Tab 键，只允许使用空格。
- 缩进的空格数不重要，但同级元素必须左侧对齐。
- 大小写敏感。

### YAML 支持以下三种数据结构：

- 对象：键值对的集合
- 数组：一组按次序排列的值
- 字面量：单个的、不可拆分的值、


## 配置文件

### 优先级
Spring Boot 启动时会扫描以下 5 个位置的  application.properties 或 apllication.yml 文件，并将它们作为 Spring boot 的默认配置文件。

1. file:./config/*/
2. file:./config/
3. file:./
4. classpath:/config/
5. classpath:/

`注：file: 指当前项目根目录；classpath: 指当前项目的类路径，即 resources 目录。`
 以上所有位置的配置文件都会被加载，且它们优先级依次降低，序号越小优先级越高。其次，位于相同位置的 application.properties 的优先级高于 application.yml。

所有位置的文件都会被加载，高优先级配置会覆盖低优先级配置，形成互补配置，即：

- 存在相同的配置内容时，高优先级的内容会覆盖低优先级的内容；
- 存在不同的配置内容时，高优先级和低优先级的配置内容取并集。

### 外部配置文件
> 使用范围 jar包的配置文件无法更改，用外部的

```bash
mvn clean package
```

- spring.config.location 
   - jar包的配置文件失效
```shell
java -jar springbootdemo-0.0.1-SNAPSHOT.jar  --spring.config.location=D:\myConfig\my-application.yml
```

- spring.config.additional.location
   - 附加文件优先级最高，然后取并集

## 日志框架

### 统一日志
> 我们在使用 Spring Boot 时，同样可能用到其他的框架，例如 Mybatis、Spring MVC、 Hibernate 等等，这些框架的底层都有自己的日志框架，此时我们也需要对日志框架进行统一。

1. Spring Boot 的核心启动器 spring-boot-starter 引入了 spring-boot-starter-logging，使用 IDEA 查看其依赖关系

SpringBoot 底层使用 slf4j+logback 的方式记录日志，当我们引入了依赖了其他日志框架的第三方框架（例如 Hibernate）时，只需要把这个框架所依赖的日志框架排除，即可实现日志框架的统一，示例代码如下。
```xml
<dependency>
  <groupId>org.apache.activemq</groupId>
  <artifactId>activemq-console</artifactId>
  <version>${activemq.version}</version>
  <exclusions>
    <exclusion>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

### 默认配置
Spring Boot 默认使用 SLF4J+Logback 记录日志，并提供了默认配置，即使我们不进行任何额外配，也可以使用 SLF4J+Logback 进行日志输出。
常见的日志配置包括日志级别、日志的输入出格式等内容

| 序号 | 日志级别 | 说明 |
| --- | --- | --- |
| 1 | trace | 追踪，指明程序运行轨迹。 |
| 2 | debug | 调试，实际应用中一般将其作为最低级别，而 trace 则很少使用。 |
| 3 | info | 输出重要的信息，使用较多。 |
| 4 | warn | 警告，使用较多。 |
| 5 | error | 错误信息，使用较多。 |

在 application.properties 中，修改 Spring Boot 日志的默认配置，代码如下。
```yaml
#日志级别
logging.level.net.biancheng.www=trace
#使用相对路径的方式设置日志输出的位置（项目根目录目录\my-log\mylog\spring.log）
#logging.file.path=my-log/myLog
#绝对路径方式将日志文件输出到 【项目所在磁盘根目录\springboot\logging\my\spring.log】
logging.file.path=/spring-boot/logging
#控制台日志输出格式
logging.pattern.console=%d{yyyy-MM-dd hh:mm:ss} [%thread] %-5level %logger{50} - %msg%n
#日志文件输出格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} === - %msg%n
```

### 自定义配置
> 留着，现在不学


## spring-boot-starter-web
> Spring Boot Web 快速开发


### Spring Boot静态资源映射
Spring Boot 默认为我们提供了 3 种静态资源映射规则：

- WebJars 映射
- 默认资源映射
- 静态首页（欢迎页）映射

#### WebJars 映射
> 官网：[https://www.webjars.org/](https://www.webjars.org/)


### 默认静态资源映射
当访问项目中的任意资源（即“/**”）时，Spring Boot 会默认从以下路径中查找资源文件（优先级依次降低）：

1. classpath:/META-INF/resources/
2. classpath:/resources/
3. classpath:/static/
4. classpath:/public/


这些路径又被称为静态资源文件夹，它们的优先级顺序为：classpath:/META-INF/resources/ > classpath:/resources/ > classpath:/static/ > classpath:/public/ 。

当我们请求某个静态资源（即以“.html”结尾的请求）时，Spring Boot 会先查找优先级高的文件夹，再查找优先级低的文件夹，直到找到指定的静态资源为止。

## Thymeleaf 
> 留着，可能不学


## 国际化
> 国际化（Internationalization 简称 I18n，其中“I”和“n”分别为首末字符，18 则为中间的字符数）是指软件开发时应该具备支持多种语言和地区的功能。换句话说就是，开发的软件需要能同时应对不同国家和地区的用户访问，并根据用户地区和语言习惯，提供相应的、符合用具阅读习惯的页面和数据，例如，为中国用户提供汉语界面显示，为美国用户提供提供英语界面显示。

在 Spring 项目中实现国际化，通常需要以下 3 步：

1. 编写国际化资源（配置）文件；
2. 使用 ResourceBundleMessageSource 管理国际化资源文件；
3. 在页面获取国际化内容。

### 1. 编写国际化资源文件
在 Spring Boot  的类路径下创建国际化资源文件，文件名格式为：基本名_语言代码_国家或地区代码，例如 login_en_US.properties、login_zh_CN.properties。

以 spring-boot-springmvc-demo1为例，在 src/main/resources 下创建一个 i18n 的目录，并在该目录中按照国际化资源文件命名格式分别创建以下三个文件，

- login.properties：无语言设置时生效
- login_en_US.properties ：英语时生效
- login_zh_CN.properties：中文时生效
> 详细的留着 


## Spring Boot拦截器精讲
> 参考[http://c.biancheng.net/spring_boot/default-config.html](http://c.biancheng.net/spring_boot/default-config.html)


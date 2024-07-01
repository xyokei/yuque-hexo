---
title: Spring
date: 2020-08-20T10:10:57.000Z
updated: '2024-07-01 20:26:43'
categories:
  - Java
tags:
  - Spring
---

## 基本概念和使用

- MVC
   - M：Model **模型**，用于处理业务逻辑及储存业务数据。
   - V：View **视图**，展示业务数据，并提供与用户互动功能的画面。
   - C：Control **控制器**, 处理用户请求，指定模型处理用户请求，并将模型处理结果反映到指定视图。

![](/images/d5c226311e15e9041733190b6a0e9598.jpeg)

- 处理流程

![](/images/ffd7b72d5b3e979fba668ea4f1eb09db.jpeg)

- 文件类型
| No. | 文件类型 | 例 | 说明 |
| --- | --- | --- | --- |
| 1 | controller | _DemoController.java_ | controller类，担当控制器的角色 |
| 2 | Model/Form | _DemoModel.java_/_DemoForm.java_ | 绑定页面数据的Java bean，**这里的Model不是MVC中的M** |
| 3 | service | _DemoService.java_ | 业务处理类，一般会调用数据库操作类 |
| 4 | Entity | _DemoEntity.java_ | 数据库表映射类 |
| 5 | Dao/Repository | _DemoDao.java_/_DemoRepository.java_ | 数据库操作类，对数据库进行增删改查。 |
| 6 | css | _main.css_ | 页面样式文件 |
| 7 | js | _demo.js_ | 页面javascript文件 |
| 8 | html | _demo.html_ | 页面html文件 |
| 9 | Framework | _Application.java_ | Springboot启动入口文件，启动时会对这个文件所在目录的同级及下级目录文件进行扫描，并放入Spring容器。 |
| 10 | Framework | _Application.yml_ | Springboot配置文件 |
| 11 | Maven | _pom.xml_ | Maven工程配置文件 |

> Model  3.4.5
> View 6.7.8
> Controll 1


##  Controller
```java
/**
 * 基于注解的controller实例
 */
//在类上加上@Controller, 告诉Spring这是一个Controller。
@Controller
public class DemoController {

    /**
     * init
     */
    @GetMapping("/init") // 对应前端Get 方法
    // @ResponseBody 一般用于异步请求
    public String init(@ModelAttribute("demoViewModel") DemoViewModel model) {

        // 业务处理

        // 指定视图 xx 对应 视图文件夹底下xx.html文件
        /* 方法返回值指定了一个视图，在demo中，init这个字符串对应index.html文件，
          表示这个请求最终返回给用户的视图是index.html */
        /*  如果在这个方法上加上@ResponseBody，表示这个方法返回的值不是指定某一个视图，
            返回值的本身就是给用户请求的最终结果。*/
        return "init";
    }  
}
```

- 在**public**方法上加上`@GetMapping`，表示这个方法对应一个请求，例如`http://localhost/init`，

        `@GetMapping`里的值就是请求的路径。

   - 映射路径的注解还有@RequestMapping，@PostMapping等。
   - 这个注解同样的可以用于类
- 方法参数前加上@ModelAttribute，表示这个参数与请求的参数绑定，这个参数的值就是请求里参数的值，一般用于绑定复杂参数。
   - @ModelAttribute    
   - 绑定请求参数有多种方式，根据项目要求选择不同的方式。
- 在实际项目中，Controller类都会继承自interface。下面的**DemoController**是一个interface。
```java
  @Controller
  public class DemoControllerImpl implements DemoController {
      //...
  }
```

##  Service
```java
@Service
//生成带有必需参数的构造函数
@RequiredArgsConstructor 
public class DemoServiceImpl implements DemoService {

    //此注解 Spring Bean里只有一个实例，多个会报错
    //依赖注入 核心 测试？ mock
    @Autowired
    private DemoRepository demoRepository;

    @Override
    public List<DemoEntity> findAllUsers() {
        // get user info
        return demoRepository.findAll();
    }
}
```

- 在类上加上`@Service`, 告诉Spring这是一个Service。
- `@Autowired`表明这是一个需要注入的对象，spring会去容器的寻找这个对象对应的实例。
   - UserRepository是持久层的interface，根据所使用的持久层框架的不同，写法也不同，本示例使用的是[Spring Data JPA](http://10.6.3.63:10070/#!spring/./spring_jpa.md#%E4%BB%80%E4%B9%88%E6%98%AFSpring_Data_JPA)。
- findAllUsers是一个检索用户情报的方法，里面调用了持久层提供的检索方法。

##  view

- 视图模板引擎 Thymeleaf， 前后端分离应该不用这个，了解就行
```java
<table>
  <thead>
    <tr>
      <th th:text="#{msgs.headers.name}">Name</th>
      <th th:text="#{msgs.headers.price}">Price</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="prod: ${allProducts}">
      <td th:text="${prod.name}">Oranges</td>
      <td th:text="${#numbers.formatDecimal(prod.price, 1, 2)}">0.99</td>
    </tr>
  </tbody>
</table>
```

- 与一般的html相比，Thymeleaf增加了很多th:开头的标签属性，最大的作用是与Java bean的字段绑定，在视图解析的时候将数据显示在视图中，以及用户请求时将请求内容传递给Java bean。

##  application.yml

##  目录结构
```sql
|-source
|  |-demo
|    |-action
|    |-service
|      |-impl
|  |-mvcfroamework
|    |-annoation
|    |-servlet
|-resources
|-webapp
```

##   配置

   1. 注解配置
```sql
首先测试
注解：
    Autowired
    Controller
    RequestMapping
    RequsetParam
    Service
```

##  思路
```sql
    
思路：
  a. 调init()方法
    -加载配置文件
    -扫描包路径，获取相关类
    -实例化扫描到的类放入IOC容器中，利用反射
    -DI依赖注入
    -初始化HandlerMapping
  b. 运行阶段
    -运行调度方法
    -重写doPost
    -重写doGet
```

# 二、实践
> 参照[https://blog.csdn.net/weixin_40160543/article/details/106425699](https://blog.csdn.net/weixin_40160543/article/details/106425699)


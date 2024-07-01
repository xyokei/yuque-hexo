---
title: SQL题目
date: 2020-09-10T10:10:57.000Z
updated: '2024-07-01 20:59:35'
categories:
  - SQL
tags:
  - 做题
  - SQL
---

## [175. Combine Two Tables](https://leetcode.cn/problems/combine-two-tables/)
> 内联 = where 条件 不写inner 用 where 指定条件 交集
> 左 右 外联 拼接

```sql
SELECT 
    a.firstname, a.lastname, b.city, b.state
FROM
    person a
LEFT JOIN address b
ON a.personid = b.personid
```

## [180. 连续出现的数字](https://leetcode.cn/problems/consecutive-numbers/)
> 关键 连续，去重
> 方法1：表自己与自己 取子集，
> LAG 函数访问 子集 上一行的某个列 数据
> LEAD 访问 子集下一行某列 数据

```sql
select distinct num 
from (
 select id,
       num,//当前id 的num
       lag(num) over (order by id) as num_p,//当前id 的上个 num 值
       lead(num) over(order by id) as num_n //当前id 的下个 num 值
 )a
where a.num = a.num_p and a.num = a.num_n
```

## [181. 超过经理收入的员工](https://leetcode.cn/problems/employees-earning-more-than-their-managers/)
```sql
SELECT 
    a.name AS employee
FROM
    employee a,
    employee b
WHERE
    a.managerid IS NOT NULL
AND a.managerid = b.id
AND a.salary > b.salary
```

## [182. 查找重复的电子邮箱](https://leetcode.cn/problems/duplicate-emails/)
```sql
SELECT 
    email
FROM
    (SELECT 
        COUNT(email) AS num, email
    FROM
        person
    GROUP BY email) a
WHERE
    a.num > 1
```

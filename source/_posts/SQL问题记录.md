---
title: SQL问题记录
date: 2022-10-20T00:00:00.000Z
updated: '2024-07-01 21:38:27'
categories:
  - SQL
tags:
  - SQL
---
SQL遇到的各种小问题，记录一下
<!-- more -->

## UNION ORDER BY 报错
UNION 两个SELECT 后不能使用用 order by  ，解决，分别再套一个select后union
```sql
SELECT UT.TABLE_NAME,
       UT.COLUMN_NAME,
       UT.COLUMN_ID,
       UT.DATA_TYPE,
       NVL(UT.DATA_PRECISION,UT.DATA_LENGTH),  -- 数字型取整数精度
       UC.COMMENTS
FROM USER_TAB_COLUMNS  UT,       -- 列视图
     USER_COL_COMMENTS UC
WHERE UT.TABLE_NAME = UPPER('TABLE_NAME') -- 表名必须大写 加个upper('表名'
AND   UT.TABLE_NAME = UC.TABLE_NAME
AND   UT.COLUMN_NAME = UC.COLUMN_NAME
ORDER BY COLUMN_ID
UNION 
SELECT UT.TABLE_NAME,
       UT.COLUMN_NAME,
       UT.COLUMN_ID,
       UT.DATA_TYPE,
       NVL(UT.DATA_PRECISION,UT.DATA_LENGTH),  -- 数字型取整数精度
       UC.COMMENTS
FROM USER_TAB_COLUMNS  UT,       -- 列视图
     USER_COL_COMMENTS UC
WHERE UT.TABLE_NAME = UPPER('TABLE_NAME1') -- 表名必须大写 加个upper('表名'
AND   UT.TABLE_NAME = UC.TABLE_NAME
AND   UT.COLUMN_NAME = UC.COLUMN_NAME
ORDER BY COLUMN_ID
```
```sql
SELECT * FROM (
SELECT UT.TABLE_NAME,
       UT.COLUMN_NAME,
       UT.COLUMN_ID,
       UT.DATA_TYPE,
       NVL(UT.DATA_PRECISION,UT.DATA_LENGTH),  -- 数字型取整数精度
       UC.COMMENTS
FROM USER_TAB_COLUMNS  UT,       -- 列视图
     USER_COL_COMMENTS UC
WHERE UT.TABLE_NAME = UPPER('TABLE_NAME') -- 表名必须大写 加个upper('表名'
AND   UT.TABLE_NAME = UC.TABLE_NAME
AND   UT.COLUMN_NAME = UC.COLUMN_NAME

UNION 
SELECT UT.TABLE_NAME,
       UT.COLUMN_NAME,
       UT.COLUMN_ID + 100,
       UT.DATA_TYPE,
       NVL(UT.DATA_PRECISION,UT.DATA_LENGTH),  -- 数字型取整数精度
       UC.COMMENTS
FROM USER_TAB_COLUMNS  UT,       -- 列视图
     USER_COL_COMMENTS UC
WHERE UT.TABLE_NAME = UPPER('TABLE_NAME1') -- 表名必须大写 加个upper('表名'
AND   UT.TABLE_NAME = UC.TABLE_NAME
AND   UT.COLUMN_NAME = UC.COLUMN_NAME
)
ORDER BY COLUMN_ID
```

## merge 更新写法
```plsql
MERGE INTO [TARGET TABLE] A
USING [TABLE] B -- B表可以select复合
ON (
    A.XXX = B.XXX AND 
    A.XXX = B.XXX
   )
WHEN MATCHED THEN
    UPDATE SET A.XXX = B.XXX
              ,A.XXX = B.XXX
WHEN NOT MATCHED THEN
    INSERT 
        (
          XXX
          ,XXX
          ,XXX
        )
    VALUES
        (
          B.XXX
          ,B.XXX
          ,B.XXX
        )
```

## 聚合函数与聚合关键字having
> 非聚合字段必须出现在GROUP BY子句中或在聚合函数中使用
> 使用聚合函数不一定要having
> 使用having时不一定要聚合函数

```plsql
--后边必须要有a，否则报错。
select a,sum(b) from A group by a 
```

## 没有主键，只有给定条件，更新表中按顺序排的第一条数据
```plsql
WITH A AS(
select  item1,
        item2,
        item4
from A
WHERE 
     --给定条件
order by item1 desc,item4 desc
)

update A
    set (A.item1,
         A.item2)
    =(select item1 + B,
             item2 -B
      from A
      where rownum = 1
    )
where A.ITEM1 = (select item1
                 from A
                where rownum = 1)
AND A.item4 = (select item4
                 from A
                where rownum = 1)
AND
   --给定条件
```

## 求表里每组item1的和 及 该组按item2降序的第一条数据
> 实际问题：
> 求一个学生成绩总和 和当前学生 所有科目 成绩最高的科目
> 当然有主键的话用id+课程id更好做了 直接按这个group by

```plsql
SELECT
  SUM(ITEM1) OVER (PARTITION BY ITEM3, ITEM4) AS sum_ITEM1,
  ROW_NUMBER() OVER (PARTITION BY ITEM3, ITEM4 ORDER BY ITEM2 DESC) AS row_num
FROM T_A
WHERE 
--给定条件
---------------
SUM(ITEM1) OVER (PARTITION BY ITEM3, ITEM4 ORDER BY ITEM2 DESC) AS sum_ITEM1,
--如果带上 ORDER BY ITEM2 DESC 就是降序累计和
```

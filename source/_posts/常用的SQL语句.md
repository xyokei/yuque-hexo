---
title: 常用的SQL语句
date: 2022-10-20T13:12:57.000Z
updated: '2024-07-01 21:38:12'
categories:
  - SQL
tags:
  - Tools
---

## 常用

### 查表的索引
```sql
-- INDEX_TYPE： normal，unique，full text
-- 1. normal： 普通索引
-- 2. unique： 唯一不重复索引
-- 3. full text： 全文搜索的索引
SELECT T.*,
       I.INDEX_TYPE         -- 索引类型
FROM USER_IND_COLUMNS T,    -- 索引列表
     USER_INDEXES I         -- 索引表
WHERE T.INDEX_NAME = I.INDEX_NAME  --索引名
AND   T.TABLE_NAME   ='TABLE_NAME'
```

### 查表的主键
```sql
-- USER_CONS_COLUMNS: 约束字段视图
-- USER_CONSTRAINTS: 约束表视图
-- CONSTRAINT_TYPE C O P R U V
-- P primary key
-- R forgin key
-- U unique

SELECT CU.*
FROM USER_CONS_COLUMNS CU,
     USER_CONSTRAINTS AU
WHERE CU.CONSTRAINT_NAME = AU.CONSTRAINT_NAME --项目名
AND   AU.CONSTRAINT_TYPE = 'P' -- 约束类型
AND   CU.TABLE_NAME = 'TABLE_NAME'
```

### 查表列和属性
```sql
SELECT *
FROM USER_TAB_COLUMNS          -- 列视图
WHERE TABLE_NAME='TABLE_NAME'; -- 表名必须大写 加个upper('表名')
```

## 例子

### 查指定表的主键  字段名  字段顺序   类型  长度 字段注释
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
```

---
title: SQL笔记
date: 2021-10-12T13:12:57.000Z
updated: '2024-07-01 21:38:05'
categories:
  - SQL
tags:
  - 笔记
---
SQL的学习记录，一些优化和基本知识
<!-- more -->

## 优化

- 避免 select *  理由：查出不必要的数据浪费内存和时间
- 用union all 替代 union 理由：union会走排重需要遍历，排序和比较，没有重复数据使用简直浪费
- 小表驱动大表 关键字 in  exists
- in 查出来放缓存，内存遍历比较快
- exists需要判断每条数据是否在exist条件里
-  查询数据时，数据量越小速度越快，
- in 先查in条件，exists 先查外条件
- not in  全表扫描未有索引 不如not exists 
- 避免全表扫描
- null为判断条件
- OR 为判断条件 改完union all（数量千万级）

## MySQL
> 没用过，了解下


## 索引

1. **聚集索引：**
   1. 聚集索引一个表只能有一个，而非聚集索引一个表可以存在多个
   2. 聚集索引物理上连续存在，非聚集索引逻辑上连续
   3. 经常出现在关键字order by、group by、distinct后面的字段，建立索引。
   4. 如果建立的是复合索引，索引的字段顺序要和这些关键字后面的字段顺序一致，否则索引不会被使用。
2. **索引存储结构：**
:::tips
**有序数组**：查改快，增删慢
**单链表**：查改慢，增删快
查找方式：二分查找，效率较快
`二分查找+链表` 诞生了BST
`BST`（Binary Search Tree）二分查找树
 定义：所有左节点小于父节点，所有右节点大于父节点
 问题：查找耗时和深度有关 解决此问题诞生AVL
`AVL`（AVL Tree）平衡二叉树
 定义：二分查找树的左右子树深度不超过1
 AVL 在插入和更新数据时，对二分查找树进行`左旋或右旋`来进行调整 ，进而实现AVL
索引里应该有什么
 索引的键值
 磁盘地址
 左右子节点的引用（用来判断往哪里查）
 问题：InnoDB（MySQL的数据库引擎）操作磁盘的最小的单位是一页（一个磁盘块）大小16KB(16384字节)
 单个节点存放上面三个内容撑死几十个字节
 每次访问节点，就进行一次IO，空间浪费
 即：节点数据少，索引寻找需要的数据，则需访问更多节点（因为遍历），IO次数过多，时间越多
 解决：
    单个节点存储多个数据
 `多路平衡查找树`（B Tree）
 路（阶，度）数 m 概念：每个节点的分叉数，即子树数量
 关键字（= 存放的索引键值）数 为 N = m-1
    m/2<=当前节点存放的关键字<=m-1
 当存放的 关键字数 =m 发生`分裂`
当存放的 关键字数 =m/2 -1 发生`合并`
节点的分裂和合并，其实就是InnoDB页的分裂和合并。
`B+树（加强版多路平衡查找树`
定义：
         关键字数和度数相等 ，即`节点数和度数相等`
         根节点和枝节点不存储数据地址，只有叶子节点才存储。即查到叶子才能取得数据
         每个叶子节点增加了一个指向相邻叶子节点的指针 
            即：最后一个数据指向下一个叶子节点第一 个数据（形成了有序链表）
         左闭右开区间检索数据
         
举例：设一条记录1KB，一个叶子节点（一页）存储16条记录（地址）
           索引字段设bigint，8字节，指针大小在InnoDB源码 为 6 字节
           则 非叶子节点可存放的 16KB(16384B) /14B = 1170个度
           设树深度为2 （深度从0开始）
           则  根节点 = 1
                 非叶子节点 = 1170
                 叶子节点  = 1170*1170  = 1368900 度
                 叶子节点存放数据 = 1170 * 1170  *16 =21902400 ~2千万条数据
 InnoDB B+ 深度一般为1~3层
 千万级数据存储 最多访问3次磁盘就足够了
:::


## 优化 基于Oracle的PL/SQL优化
> 有些是SQL通用的

1. 把数组形式 改成 RECORD 
2、能直接写值就直接写，不用动态参数传递
       答：一般来说直接写SQL的性能是高于拼字符串的，因为如果执行拼字符串的需要内部自动调动 
              oracle机制，先解析字符串映射成SQL语句然后再执行
3、创建的work TABLE 指定 主键 目的是为了使用索引
4、sql拼接直接一句搞定，不要多句一起写
5、可以直接写 SQL的 直接写，不用 EXECUTE IMMEDIATE
6 BULK COLLECT INTO 写法
7、对应第3条，如何使用索引
8、数据量大的时候 根据数据库选择优化where条件 
9、创建work table 优化逻辑
```plsql
DECLARE
  V_SQL VARCHAR2(32767);
BEGIN
 --1. 把数组形式 改成 RECORD 有什么说法吗
 --2、能直接写值就直接写，不用入参
 --   直接写值为什么比入参快啊
 --ex 1 原
  V_SQL := 'INSERT TABLE_NAME (
                ITEM1
                ,ITEM2
            ) VALUES(
                :ITEM1
                ,:ITEM2
            )';
  EXECUTE IMMEDIATE V_SQL USING 1, VARIABLE_1
 --改
  V_SQL := 'INSERT TABLE_NAME (
                ITEM1
                ,ITEM2
            ) VALUES(
                1
                ,:ITEM2
            )';
  EXECUTE IMMEDIATE V_SQL USING VARIABLE_1
 --4、sql拼接直接一句搞定，不要多句一起写
 --ex 2原
  V_SQL := 'selsect ';
  V_SQL := V_SQL || ' item1';
  V_SQL := V_SQL || ',item2';
  V_SQL := V_SQL || ' FROM TABLE_NAME';
 --改
  V_SQL := 'selsect 
                item1
                ,item2
            from table_name
            ';
 --3、创建的work TABLE 指定 主键 目的是为了使用索引
  EXECUTE IMMEDIATE'
  CREATE TABLE TABLE_NAME(
    ITEM VARCHAR2(200 BYTE)
  )';
  EXECUTE IMMEDIATE'
  CREATE TABLE TABLE_NAME(
   ITEM   VARCHAR2(200 BYTE),
   PRIMAY KEY (ITEM)
  )';
 --5、可以直接写 SQL的 直接写，不用 EXECUTE IMMEDIATE
 --6 BULK COLLECT INTO 写法
 --ex 原
  EXECUTE IMMEDIATE'
  SELECT ITEM FROM TABLE_NAME
  WHERE ITEM <> ''XX''
  ORDER BY ITEM
  ' BULK COLLECT INTO VAR_TABLE_ARR;
  FOR I IN 1..VAR_TABLE_ARR.COUNT LOOP
 --用i获取数据
  END LOOP;
 --改
  FOR VAR_TABLE_ARR IN(
    SELECT
      ITEM
    FROM
      TABLE_NAME
    WHERE
      ITEM <> 'XX'
    ORDER BY
      ITEM
  ) LOOP
 --用变量获取数据
  END LOOP END;
 --改2
  OPEN CUR_TABLENAME_ARR FOR'
    SELECT ITEM
    FROM TABLE_NAME
    ORADER BY ITEM 
  ' LOOP FETCH CUR_TABLENAME_ARR INTO REC_TABLE;
  EXIT WHEN CUR_TABLENAME_ARR%NOTFOUND
 --XXXXX
  END LOOP;
 --7、对应第3条，如何使用索引
 --原
  SELECT
    COUNT(1)
  FROM
    TABLE_NAME
 --改
    SELECT
      COUNT(1)
    FROM
      TABLE_NAME
    GROUP BY
      (ITEM)
  --8、数据量大的时候 根据数据库选择优化where条件 
  --9、创建work table 优化逻辑
```
> 针对mysql，其条件执行顺序是 从左往右，自上而下；
> 针对orcale，其条件执行顺序是从右往左，自下而上

- 自下而上 ->数据量较少的表尽量放在后面。即小表驱动大表
- 从右往左-> 过滤掉最大数量记录的条件写最右

1、FROM 子句：执行顺序为从后往前、从右到左。数据量较少的表尽量放在后面。
2、WHERE子句：执行顺序为自下而上、从右到左。将能过滤掉最大数量记录的条件写在WHERE 子句的最右。
3、GROUP BY：执行顺序从左往右分组，最好在GROUP BY前使用WHERE将不需要的记录在GROUP BY之前过滤掉。
4、HAVING 子句：消耗资源。尽量避免使用，HAVING 会在检索出所有记录之后才对结果集进行过滤，需要排序等操作。
5、SELECT子句：少用*号，尽量取字段名称。ORACLE 在解析的过程中, 通过查询数据字典将*号依次转换成所有的列名, 消耗时间。
6、ORDER BY子句：执行顺序为从左到右排序，消耗资源

从右往左-> 表数据量小的放右边
自下而上-> 两表关联此条件后数据量下的放最下边
sql从下往上编译，数据量大的时候性能就会有差别
**排除越多的条件放在第一个。**

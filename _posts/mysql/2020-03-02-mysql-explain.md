---
layout: post
title: mysql explain 命令详解
author:  shenfh
catalog: true
date:  2020-03-02 07:17:17
tags:
    - mysql 
header-img: img/blog-0.jpg
---


MySQL 提供了一个 explain 命令, 可以对 SELECT 语句进行分析, 并输出执行参数，通过这些执行参数我们可以知道哪些地方需要优化。

explain 执行结果如下

```
explain select * from t_user;
+----+-------------+--------+------------+------+---------------+------+---------+------+-------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows  | filtered | Extra |
+----+-------------+--------+------------+------+---------------+------+---------+------+-------+----------+-------+
|  1 | SIMPLE      | t_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 37648 |   100.00 | NULL  |
+----+-------------+--------+------------+------+---------------+------+---------+------+-------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```

##  输出参数说明

### id
执行编号，标识select所属的行。如果在语句中没子查询或关联查询，只有唯一的select，每行都将显示1。否则，内层的select语句一般会顺序编号，对应于其在原始语句中的位置

#### select_type
 查询类型。他可以是以下值
 
 
| 类型 |  说明 |  备注 |
| --- | --- | --- |
| simple | 简单SELECT,不使用UNION或子查询等 |  |
| primary | 最外面的查询 或者 主查询，在有子查询的语句中，最外面的select查询就是primary |  |
| subquery | 子查询 |  |
| derived | 派生表,该临时表是从子查询派生出来的，位于from中的子查询 |  |
| union | 位于union中第二个及其以后的子查询被标记为union，第一个就被标记为primary如果是union位于from中则标记为derived |  |
| union result | union之后的结果 |  |
| dependent union | unoin 中的第二个或随后的 select 查询，依赖于外部查询的结果集 |  |
| dependent subquery | 子查询中的第一个 select 查询，依赖于外部 查询的结果集|  |

### table
对应行正在访问哪一个表，表名或者别名

### type

type显示的是访问类型，是较为重要的一个指标，结果值从好到坏依次是：
`system`,`const`,`eq_ref`,`ref` ,`range`,`index`,`ALL` 

* `system` 表只有一行记录(等于系统表),这是const类型的特列
* `const` 表示通过索引一次就找到了,const用于比较primary key或者unique索引。因为只匹配一行数据，所以很快。如将主键置于where列表中，MySQL就能将该查询转换为一个常量
* `eq_ref` 唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描
* `ref` 非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一宗索引访问，它返回所有匹配某个单独值的行，然而它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体
* `range` 只检索给定范围的行，使用一个索引来选择行。key列显示使用了哪个索引。一般就是在where语句中出现了between、<、>、in等的查询。这种范围扫描索引比权标扫描要好，因为它只需要开始于索引的某一点，而结束于另一点，不用扫描全部索引。
* `index` <font color=red>Full Index Scan</font>, index与ALL区别为index类型只遍历索引数。这通常比ALL快，因为索引文件通常比数据文件小。(虽然all和index都是读全表，但index是从索引中读取，而all是从硬盘中读取)
* `all` <font color=red>Full Table Scan</font>，将遍历全表以找到匹配的行.


### possible_keys
显示可能应用在这张表中的索引，一个或多个。查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用。

### key
实际使用的索引。如果为NULL，则没有使用索引。查询中若使用了覆盖索引，则该索引仅出现在key列表中。

### key_len
表示索引中使用的字节数，可通过该列计算查询中使用的索引长度。在不损失精确性的情况下，长度越短越好。key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的。

### ref
显示索引的哪一列被使用了，如果可能的话，是一个常数。哪些列表或常量被用于查找索引列上的值。

### rows
根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数。

### Extra
查询优化器执行查询的过程中对查询计划的重要补充信息


| 类型 | 说明 |
| --- | --- | 
| Using filesort	 | MySQL有两种方式可以生成有序的结果，通过排序操作或者使用索引，当Extra中出现了Using filesort 说明MySQL使用了后者，但注意虽然叫filesort但并不是说明就是用了文件来进行排序，只要可能排序都是在内存里完成的。大部分情况下利用索引排序更快，所以一般这时也要考虑优化查询了。使用文件完成排序操作，这是可能是ordery by，group by语句的结果，这可能是一个CPU密集型的过程，可以通过选择合适的索引来改进性能，用索引来为查询结果排序。 | 
| Using temporary | 用临时表保存中间结果，常用于GROUP BY 和 ORDER BY操作中，一般看到它说明查询需要优化了，就算避免不了临时表的使用也要尽量避免硬盘临时表的使用。 |
| USING index | 表示相应的select操作中使用了覆盖索引(Covering Index),避免访问了表的数据行，效率不错。如果同时出现using where，表明索引被用用来执行索引键值的查找；如果没有同时出现using where，表明索引用来读取数据而非执行查找动作。|
| Using where | 表使用了where过滤 |
| using join buffer | 使用了连接缓存 | 
|  distinct | 优化distinct操作，在找到第一匹配的元祖后即停止找同样值的动作 |



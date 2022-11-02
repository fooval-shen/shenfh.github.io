---
layout: post
title: MySQL note
author:  shenfh
catalog: true
date:  2021-04-10 00:10:10
tags:
    - mysql
header-img: img/blog-0.jpg
---


## MySQL Enviroment

```shell
 select version();
+-----------+
| version() |
+-----------+
| 8.0.29    |
+-----------+
```

## JSON Supports

### Add JSON column

```shell
CREATE TABLE IF NOT EXISTS `test`
(
    `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
    `extra` JSON NOT NULL DEFAULT (JSON_OBJECT()) COMMENT 'extra value',
    `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'create time',
    PRIMARY KEY (`id`)
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci COMMENT = 'test' ROW_FORMAT = Dynamic;
```

### Insert JSON value
```shell
insert into test (extra) values (json_object('key1','value1','key2','value2'));

select * from test;
+----+--------------------------------------+---------------------+
| id | extra                                | created_at          |
+----+--------------------------------------+---------------------+
|  1 | {"key1": "value1", "key2": "value2"} | 2021-04-10 06:53:09 |
+----+--------------------------------------+---------------------+
1 row in set (0.00 sec)
```

### Select JSON value

```shell
 select extra->"$.key1" as value from test;
+----------+
| value    |
+----------+
| "value1" |
+----------+
1 row in set (0.00 sec)

mysql> select extra->>"$.key1" as value from test;
+--------+
| value  |
+--------+
| value1 |
+--------+
1 row in set (0.00 sec)
```

### Max JSON packages

The size of any JSON document stored in a `JSON` column is limited to the value of `max_allowed_package` system variable.

```shell
show VARIABLES like 'max_allowed_packet';
+--------------------+----------+
| Variable_name      | Value    |
+--------------------+----------+
| max_allowed_packet | 16777216 |
+--------------------+----------+
1 row in set (0.00 sec)
```

### Functions

#### `json_object()` 
Return a JSON object.

```shell
select json_object('key1','value1','key2','value2');
+----------------------------------------------+
| json_object('key1','value1','key2','value2') |
+----------------------------------------------+
| {"key1": "value1", "key2": "value2"}         |
+----------------------------------------------+
1 row in set (0.00 sec)
```

#### `json_array()` 
Return as JSON array.

```shell
select json_array('array1','array2','array3');
+----------------------------------------+
| json_array('array1','array2','array3') |
+----------------------------------------+
| ["array1", "array2", "array3"]         |
+----------------------------------------+
1 row in set (0.00 sec)
```

#### `json_type()` 
 Expects a JSON argument and attempts to parse it into a JSON value. It returns the value's JSON type if it is valid and produces an error otherwise.

```shell
select json_type('value1');
ERROR 3141 (22032): Invalid JSON text in argument 1 to function json_type: "Invalid value." at position 0.

mysql> select json_type('"value1"');
+-----------------------+
| json_type('"value1"') |
+-----------------------+
| STRING                |
+-----------------------+

```



### Partial Updates of JSON Values
 

#### `JSON_SET(json_doc, path, val[, path, val] ...)` 
 
Inserts or updates data in a JSON document and returns the result. Returns NULL if any argument is NULL or path, if given, does not locate an object. An error occurs if the `json_doc` argument is not a valid JSON document or any path argument is not a valid path expression or contains a `*` or `**` wildcard.

```
 update test set extra= json_set(extra,'$.key1','new_value1') where id =1 ;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from test;
+----+------------------------------------------+---------------------+
| id | extra                                    | created_at          |
+----+------------------------------------------+---------------------+
|  1 | {"key1": "new_value1", "key2": "value2"} | 2021-04-10 06:53:09|
+----+------------------------------------------+---------------------+
1 row in set (0.01 sec)
```

#### `JSON_INSERT(json_doc, path, val[, path, val] ...)`

Inserts data into a JSON document and returns the result. Returns NULL if any argument is NULL. An error occurs if the `json_doc` argument is not a valid JSON document or any path argument is not a valid path expression or contains a `*` or `**` wildcard.

#### `JSON_REPLACE(json_doc, path, val[, path, val] ...)`

Replaces existing values in a JSON document and returns the result. Returns NULL if any argument is NULL. An error occurs if the `json_doc` argument is not a valid JSON document or any path argument is not a valid path expression or contains a `*` or `**` wildcard.

```shell
mysq-> update test set extra= json_replace(extra, '$.key1','value100') where id = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from test;
+----+------------------------------------------------------------------+---------------------+
| id | extra                                                            | created_at          |
+----+------------------------------------------------------------------+---------------------+
|  1 | {"key1": "value100", "key2": "value2", "key3": "new_value1aaaa"} | 2021-04-10 06:53:09 |
+----+------------------------------------------------------------------+---------------------+
1 row in set (0.00 sec)
```

#### `JSON_REMOVE(json_doc, path[, path] ...)`

Removes data from a JSON document and returns the result. Returns NULL if any argument is NULL. An error occurs if the `json_doc` argument is not a valid JSON document or any path argument is not a valid path expression or is $ or contains a `*` or `**` wildcard.


```shell
mysql> update test set extra= json_remove(extra, '$.key3','$.key4');
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from test;
+----+----------------------------------------+---------------------+
| id | extra                                  | created_at          |
+----+----------------------------------------+---------------------+
|  1 | {"key1": "value100", "key2": "value2"} | 2021-04-10 06:53:09 |
+----+----------------------------------------+---------------------+
1 row in set (0.00 sec)

```
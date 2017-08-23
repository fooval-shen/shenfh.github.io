---
layout: post
title:  GitHub 高级搜索
author:     shefh
catalog:    false
date:       2017-08-22 11:17:17
tags:
    - Git
header-img: img/blog-0.jpg
---

在GitHub上使用高级搜索可以更精准搜索出自己需要的开源代码。

## 搜索语法
搜索格式为：

```
关键字  搜索条件  语言限制
```
例如：

```
AFN  stars:>1000 language:Objective-C 
```

###  搜索条件
* `stars` 
   * `stars: "100..150"`  搜索出星星在 100 和 150之间的数据.还可以使用 `n..*` 和 `*..n`
   *  `stars: > 1000`  搜索出星星大于 1000的所有数据。还是可以使用 `>=` `<` 以及`<=`

* `created` 根据创建时间搜索。可以使用算数符  `>`, `>=`, `<`, `<=`
  * `>YYYY-MM-DD`
  * `YYYY-MM-DD..YYYY-MM-DD` 按照区间搜索

* `NOT` 搜索不包含某个字符串的数据


  ```
  hello NOT world  //搜索包好hello 但是不包含world的数据
  ```

### 语言限制
语法 `language:Objective-C `  限制搜索结果的语言为`Objective-C`的数据。

```
stars:>1000 language:Objective-C

language:HTML stars:>10000
```

## 参考
[参考地址](https://help.github.com/articles/understanding-the-search-syntax/)



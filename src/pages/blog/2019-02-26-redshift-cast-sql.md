---
templateKey: blog-post
title: redshift cast sql
date: 2019-02-26T02:40:26.524Z
description: redshiftで型変換を行う方法
tags:
  - Redshift
  - SQL
---
## パターン1

```sql
SELECT CAST('0001' as integer)
```

## パターン2

```sql
SELECT '0001'::integer
```


## 参考
[CAST 関数および CONVERT 関数](https://docs.aws.amazon.com/ja_jp/redshift/latest/dg/r_CAST_function.html)

---
templateKey: blog-post
title: DjangoでDateTime フィールド同士をフォーマットを指定して比較する
date: 2019-02-07T01:47:08.095Z
description: SQLでは、 `DATE_FORMAT(entry_time, '%Y%m%d')` と書くところを、Djangoのqueryseでも指定したい
tags:
  - django
---

## 概要

SQLでは、 `DATE_FORMAT(entry_time, '%Y%m%d')` と書くところを、Djangoのqueryseでも指定したい

```sql
where (DATE_FORMAT(entry_time, '%Y%m%d') = DATE_FORMAT(update_time, '%Y%m%d'))
```


- Django 1.11
- Python 3.6.2
- mysqlclient 1.3.12

## コード

```python
queryset.extra(where=["DATE_FORMAT(entry_time, '%%Y%%m%%d') != DATE_FORMAT(update_time, '%%Y%%m%%d')",])
```

## 参考

- [Calling DATE_FORMAT() in MySQL from Django fails
](https://stackoverflow.com/questions/18136629/calling-date-format-in-mysql-from-django-fails)
- [How to add DateTimeField in django without microsecond
](https://stackoverflow.com/questions/46539755/how-to-add-datetimefield-in-django-without-microsecond?noredirect=1&lq=1)
- [Extra method containing %...](https://code.djangoproject.com/ticket/16889#comment:2)
- [素の SQL 文の実行](https://docs.djangoproject.com/ja/1.11/topics/db/sql/)


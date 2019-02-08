---
templateKey: blog-post
title: WordPressで管理画面のライブラリの表示が遅いのでpost_mime_typeのインデックスを貼ったら早くなった。
date: 2019-02-08T01:47:08.095Z
description: WordPressの管理画面、ライブラリのパフォーマンスをMySQLのindexで早くした話
tags:
  - WordPress
  - MySQL
---

## 概要
何年も運用しているとWordPressにアップロードしてるファイルがかなりの数になってきていて管理画面のライブラリ画面の表示が遅くなってきます。
MySQLのスロークエリーを眺めていると `SELECT DISTINCT post_mime_type FROM wp_posts WHERE post_type = 'attachment';`が特に遅かったりします。

`EXPLAIN` で確認して見ると Extraが `Using temporary` インデックスが効いてない模様

インデックスを貼って

```SQL
CREATE INDEX post_type_mime_type_idx ON wp_posts (post_type,post_mime_type);
``` 

`EXPLAIN` で確認して見ると Extraが `Using where; Using index` インデックスが効いてます。
調べてみるとWordPressのチケット31071にそのまんまの内容がありました。

## 教訓
WordPressのアップデートを怠っていると日々WordPressコミニティの改善の恩恵を受けれないので積極的に更新しよう！

## 参考
[media / post_mime_type related queries are very slow on larger sites](https://core.trac.wordpress.org/ticket/31071)
[漢(オトコ)のコンピュータ道: MySQLのEXPLAINを徹底解説!!](http://nippondanji.blogspot.jp/2009/03/mysqlexplain.html)


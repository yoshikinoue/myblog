---
templateKey: blog-post
title: Django dumpdata json output
date: 2019-02-14T03:06:17.323Z
description: |-
  Djangoのmanageコマンドに `dumpdata` , `loaddata` がある。
  開発環境で既に入力された設定データなどを本番環境に持ってくる時や有用
  テストデータを用意する時に使える。
tags:
  - Django
---
musqldumpでいいじゃないかってのはあるのですが、全部のデータは不要で、○○モデルに登録された設定データが必要って時がある。

## コマンド

### 出力

```
python manage.py dumpdata {app名.model名} --format=json --indent 2 > {ファイル名}
```

### ロード

```
python manage.py loaddata {ファイル名}
```

### 例

#### 出力

```
python manage.py dumpdata myapp.post --format=json --indent 2 > myapp.post.json
```

#### ロード

```
python manage.py loaddata /home/hoge/fuga/myapp.post.json
```

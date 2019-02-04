---
templateKey: blog-post
title: subversionのメモ
date: 2019-02-04T10:22:27.320Z
description: subversionのコマンドのメモ
tags:
  - subversion
---
## ブランチの確認
```
svn stat
```

## 監視除外
```
svn propset svn:ignore "*" tmp/

```

## 監視除外ファイルの確認
```
svn stat --no-ignore
```

## ログの確認
```
svn log -r '{20141201}:HEAD'
```

## 12345を逆マージ
```
svn merge -c -12345 .
```

## 管理除外設定

### ディレクトリの除外

```
svn propset svn:ignore ‘*’ [ディレクトリ名]
```

### デリレクトリの特定ファイルを除外したい場合

```
svn propset svn:ignore "*.log" logs/
```

### 除外ファイルの確認
```
svn status --no-ignore
```


## コンフリクトの解消
```
svn resolved [ファイル名]
```

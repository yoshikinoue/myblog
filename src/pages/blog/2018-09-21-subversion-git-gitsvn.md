---
templateKey: blog-post
title: Git-svnでSubversionからGitに移行方法
date: 2018-09-21T15:04:10.000Z
description: Subversionのリポジトリからcommitログを引き継いだ形でGitへ移行した際の手順です。また本番環境でのgitへの切替も含んでます。
tags:
  - git
  - Subversion
  - Gitsvn
---

## はじめに

Subversionのリポジトリからcommitログを引き継いだ形でGitへ移行した際の手順です。
また本番環境でのgitへの切替も含んでます。

## 注意点
一度、自分の手元のPCで作業用のディレクトリを作成して作業することをおすすめします。
間違ってもいきなり本番環境などでは作業しないように…。

## SVNからGitへの移行作業

### 標準的なリポジトリ構造なら下記コマンドの実行でスムーズに行きます。

一連の作業は`git svn` コマンドで作業していきます。

```
git svn clone -s --prefix=svn/ svn+ssh://svnserve@hogehoge.com/hoge

標準的な構成
trunk
branches/hoge
branches/hugahuga2
tags
```

### リポジトリが変則的な場合

Subversionのリポジトリ構成が変則的な場合、`git svn clone`（内部で行っている事）を手動で行うことで対応出来る。

- `git svn init` で初期化
- `.git/config` を編集し、リポジトリ構成を明示しておく
- `git svn fetch` で取得

```
git svn init -s --prefix=svn/ svn+ssh://svnserve@hogehoge.com/hoge

/* .git/configを編集 */
vim .git/config

------------------------------------------------------
[svn-remote "svn"]
        url = svn+ssh://svnserve@hogehoge.com/hoge
        fetch = trunk:refs/remotes/svn/trunk
        fetch = dev:refs/remotes/svn/dev　　//追記部分
        branches = branches/*:refs/remotes/svn/*
        tags = tags/*:refs/remotes/svn/tags/*
------------------------------------------------------

git svn fetch
```


### ブランチの確認

```
git branch -a

  remotes/svn/dev
  remotes/svn/trunk
```

### ブランチを作る

```
git checkout -b svn/trunk
git checkout -b develop svn/dev
```

### 除外ファイル

```
git svn show-ignore >> .gitignore
```

### 予め用意しておいた空のリモートリポジトリを追加＆push

```
git remote add origin ssh://git@hogehoge.com/hoge

git push -u origin --all
```


### 既存のディレクトリへの反映

さて、無事VCSをGitに移行出来たら、開発環境や本番環境もGitに変更しましょう。

```
git init
git remote add origin ssh://git@hogehoge.com/hoge
git checkout -b svn-to-git
git fetch origin
git checkout origin/master -- .gitignore //監視除外の為リモートから取得する。
git reset --hard origin/master
```

### リモートをブランチをローカルに作成

```
git checkout -b master origin/master
git checkout -b develop origin/develop
```

### 仮に作ったブランチを削除

```
git branch -d svn-to-git
SVN使わないから削除
rm -rf .svn
SVNのバージョンが古い場合各ディレクトリに.svnが存在するので下記で対応
find . -name .svn | xargs rm -rf
```

### 参考情報
- [仕事で使ってる巨大SVNレポジトリをGithubに移管するためにやったことまとめ · DQNEO起業日記]([http://dqn.sakusakutto.jp/2012/10/svn-git-github-migration.html](http://dqn.sakusakutto.jp/2012/10/svn-git-github-migration.html))
- [git-svnでSVN→Gitへの移行をやってみたログ]([http://qiita.com/hidekuro/items/4727715fbda8f10b6b11](http://qiita.com/hidekuro/items/4727715fbda8f10b6b11))

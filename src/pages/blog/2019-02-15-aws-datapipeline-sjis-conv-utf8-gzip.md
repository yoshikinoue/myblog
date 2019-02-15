---
templateKey: blog-post
title: AWS DataPipeline でsjisからutf8に変換してgzipで圧縮したい
date: 2019-02-15T02:47:08.095Z
description: shellコマンドを使ってsjisからutf8に変換してgzip圧縮を行う。
tags:
  - AWS
  - DataPipeline
---

## 処理の流れ

s3://inputlocation/ #ファイルがuploadされる場所

s3://outputlocation/ #utf8でgzipで圧縮したファイル置き場


### INPUT1_STAGING_DIR

```TIPS
    {
      "directoryPath": "s3://inputlocation/",
      "name": "S3InputLocation",
      "id": "S3InputLocation",
      "type": "S3DataNode"
    },
```

### OUTPUT1_STAGING_DIR

```json
    {
      "directoryPath": "s3://outputlocation/",
      "name": "S3OutputLocation",
      "id": "S3OutputLocation",
      "type": "S3DataNode"
    },
```

### shift-jis -> utf8 変換とgzip圧縮

```bash
#!/bin/bash
for file in `aws s3 ls s3://inputlocation/ | awk '{print $4}' | grep "^huga" `; do
  echo "${file}"
  iconv -f shift-jis -t utf-8 "${INPUT1_STAGING_DIR}/${file}" | gzip -c > "${OUTPUT1_STAGING_DIR}/${file}.gz"
done
```

### shellコマンドの解説

- `aws s3 ls s3://inputlocation/`
S3バケットのフォルダをls(ファイル一覧を出力)

- `awk '{print $4}'`
そのままだとファイル名以外の情報も出てくるのでawkでファイル名だけ取得

- `grep "^huga" `
hugaから始まるファイルを対象にしたいのでgrep

- `iconv -f shift-jis -t utf-8 "${INPUT1_STAGING_DIR}/${file}"`
iconvでsjisからutf8に変換

- gzip -c > "${OUTPUT1_STAGING_DIR}/${file}.gz"
gzip圧縮

forで回す際に${INPUT1_STAGING_DIR}でディレクトリ内のファイルでloopを回したかったが出来なかったので、直接 aws s3コマンドでS3バケットを指定している。

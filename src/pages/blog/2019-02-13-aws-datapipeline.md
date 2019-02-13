---
templateKey: blog-post
title: AWS DataPipeline S3 Redshift 読み込みに必要なものをインストール
date: 2019-02-13T02:47:08.095Z
description: AWS DataPipeline S3 Redshift 読み込みに必要な下準備、これを最初のShellCommandActivityに登録しておく
tags:
  - AWS
  - DataPipeline
---

## CSV読み込み、Redshift書き出しに必要なもの

DataPipelineで起動するEC2のaws-cliは古い場合があるのでアップデートを入れておく
Redshiftと連携するなら `postgresql` もインストールしておく

```bash
#!/bin/bash
echo 'aws-cli postgresql install start'
sudo yum update aws-cli -y
sudo yum install postgresql -y
(sudo easy_install pip) && (sudo pip install --no-cache-dir --upgrade awscli)
echo 'python3 install and pip requests boto3 install start'
sudo yum install -y python3 python3-devel && sudo python3 -m pip install --upgrade pip && sudo python3 -m pip install requests boto3
```

## `EC2ResourceObj` のimage id も明示的に指定したほうが良い。

- image id:ami-0a2de1c3b415889d2
- instance type:t2.micro

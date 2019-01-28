---
templateKey: 'blog-post'
title: 'Varnish6.1 install Enterprise Linux7'
date: 2018-10-29T15:04:10.000Z
description: How to Install Varnish6.1 Enterprise Linux7
tags:
  - Varnish
  - Linux
---
## Varnish6.1 install Enterprise Linux7

```bash
# epel repositories install
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# varnish61 repositories install
curl -s https://packagecloud.io/install/repositories/varnishcache/varnish61/script.rpm.sh | sudo bash
```

```bash
sudo vim /etc/yum.repos.d/varnishcache_varnish61.repo
```

edit /etc/yum.repos.d/varnishcache_varnish61.repo

varnish61/el/6/$basearch
↓
varnish61/el/7/$basearch

```/etc/yum.repos.d/varnishcache_varnish61.repo
[varnishcache_varnish61]
name=varnishcache_varnish61
baseurl=https://packagecloud.io/varnishcache/varnish61/el/7/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/varnishcache/varnish61/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[varnishcache_varnish61-source]
name=varnishcache_varnish61-source
baseurl=https://packagecloud.io/varnishcache/varnish61/el/7/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/varnishcache/varnish61/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```

```bash
sudo yum -q makecache -y --disablerepo='*' --enablerepo='varnishcache_varnish61'
```

```bash
$ yum search varnish
読み込んだプラグイン:langpacks, priorities, update-motd
epel                                                                        12756/12756
varnishcache_varnish61                                                              4/4
158 packages excluded due to repository priority protections
================================= N/S matched: varnish =================================
collectd-varnish.x86_64 : Varnish plugin for collectd
varnish-devel.x86_64 : Development files for varnish
varnish-docs.x86_64 : Documentation files for varnish
varnish-libs.x86_64 : Libraries for varnish
varnish-libs-devel.x86_64 : Development files for varnish-libs
varnish.x86_64 : High-performance HTTP accelerator

  Name and summary matches only, use "search all" for everything.
```

```bash
$ yum info varnish
読み込んだプラグイン:langpacks, priorities, update-motd
158 packages excluded due to repository priority protections
利用可能なパッケージ
名前                : varnish
アーキテクチャー    : x86_64
バージョン          : 6.1.1
リリース            : 1.el7
容量                : 1.8 M
リポジトリー        : varnishcache_varnish61/x86_64
要約                : High-performance HTTP accelerator
URL                 : https://www.varnish-cache.org/
ライセンス          : BSD
説明                : This is Varnish Cache, a high-performance HTTP accelerator.
                    :
                    : Varnish Cache stores web pages in memory so web servers don't have
                    : to create the same web page over and over again. Varnish Cache
                    : serves pages much faster than any application server; giving the

```

### install varnish

```
$ sudo yum install varnish -y
読み込んだプラグイン:langpacks, priorities, update-motd
amzn2-core                                                       | 2.4 kB  00:00:00
varnishcache_varnish61-source/signature                          |  836 B  00:00:00
varnishcache_varnish61-source/signature                          | 1.0 kB  00:00:00 !!!
varnishcache_varnish61-source/primary                            |  935 B  00:00:01
varnishcache_varnish61-source                                                       2/2
158 packages excluded due to repository priority protections
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ varnish.x86_64 0:6.1.1-1.el7 を インストール
--> 依存性の処理をしています: libjemalloc.so.1()(64bit) のパッケージ: varnish-6.1.1-1.el7.x86_64
--> トランザクションの確認を実行しています。
---> パッケージ jemalloc.x86_64 0:3.6.0-1.el7 を インストール
--> 依存性解決を終了しました。

依存性を解決しました

========================================================================================
 Package         アーキテクチャー
                               バージョン           リポジトリー                   容量
========================================================================================
インストール中:
 varnish         x86_64        6.1.1-1.el7          varnishcache_varnish61        1.8 M
依存性関連でのインストールをします:
 jemalloc        x86_64        3.6.0-1.el7          epel                          105 k

トランザクションの要約
========================================================================================
インストール  1 パッケージ (+1 個の依存関係のパッケージ)

総ダウンロード容量: 1.9 M
インストール容量: 7.2 M
Downloading packages:
警告: /var/cache/yum/x86_64/2/epel/packages/jemalloc-3.6.0-1.el7.x86_64.rpm: ヘッダー V3 RSA/SHA256 Signature、鍵 ID 352c64e5: NOKEY
jemalloc-3.6.0-1.el7.x86_64.rpm の公開鍵がインストールされていません
(1/2): jemalloc-3.6.0-1.el7.x86_64.rpm                           | 105 kB  00:00:00
(2/2): varnish-6.1.1-1.el7.x86_64.rpm                            | 1.8 MB  00:00:01
----------------------------------------------------------------------------------------
合計                                                       1.5 MB/s | 1.9 MB  00:01
file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7 から鍵を取得中です。
Importing GPG key 0x352C64E5:
 Userid     : "Fedora EPEL (7) <epel@fedoraproject.org>"
 Fingerprint: 91e9 7d7c 4a5e 96f1 7f3e 888f 6a2f aea2 352c 64e5
 Package    : epel-release-7-11.noarch (installed)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  インストール中          : jemalloc-3.6.0-1.el7.x86_64                             1/2
  インストール中          : varnish-6.1.1-1.el7.x86_64                              2/2
  検証中                  : varnish-6.1.1-1.el7.x86_64                              1/2
  検証中                  : jemalloc-3.6.0-1.el7.x86_64                             2/2

インストール:
  varnish.x86_64 0:6.1.1-1.el7

依存性関連をインストールしました:
  jemalloc.x86_64 0:3.6.0-1.el7

完了しました!
```

### installed vernish version check

```bash
$ varnishd -V
varnishd (varnish-6.1.1 revision efc2f6c1536cf2272e471f5cff5f145239b19460)
Copyright (c) 2006 Verdens Gang AS
Copyright (c) 2006-2015 Varnish Software AS
```

### systemd

```
sudo systemctl start varnish
sudo systemctl stop varnish
sudo systemctl enable varnish
```

### edit varnish.service

```
sudo vim /etc/systemd/system/varnish.service
```

```/etc/systemd/system/varnish.service
[Unit]
Description=Varnish Cache, a high-performance HTTP accelerator
After=network-online.target

[Service]
Type=forking
KillMode=process

# Maximum number of open files (for ulimit -n)
LimitNOFILE=131072

# Locked shared memory - should suffice to lock the shared memory log
# (varnishd -l argument)
# Default log size is 80MB vsl + 1M vsm + header -> 82MB
# unit is bytes
LimitMEMLOCK=85983232

# Enable this to avoid "fork failed" on reload.
TasksMax=infinity

# Maximum size of the corefile.
LimitCORE=infinity

ExecStart=/usr/sbin/varnishd -a :6081 -f /etc/varnish/default.vcl -s malloc,256m
ExecReload=/usr/sbin/varnishreload

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl restart varnish
```

### link

[varnishcache Installation instructions](https://packagecloud.io/varnishcache/varnish61/install#manual-rpm)

```
/usr/sbin/varnishd -f test.vcl -s malloc,128M -T 127.0.0.1:2000 -a :80 -a 0.0.0.0:8080,PROXY -P /home/hoge/logs/varnishd.pid -n fullkaiten -t 300 -p feature=+https_scheme
```
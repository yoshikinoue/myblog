---
templateKey: 'blog-post'
title: 'SSL/TLSプロキシサーバ Hitch インストール'
date: 2018-09-21T15:04:10.000Z
description: Varnishを使用していてSSL/TLS（https）を使いたいならシンプルな `Hitch` が心強い助けになる。
tags:
  - Hitch
  - Varnish
  - Centos
---
[公式サイト]([https://hitch-tls.org/](https://hitch-tls.org/))

Varnishを使用していてSSL/TLS（https）を使いたいならシンプルな `Hitch` が心強い助けになる。
※Varnish4.1以降でないとPROXY Protocolに対応してないので注意

## CentOSにhitchインストール
作業時点での最新バージョン
2016-01-26: Hitch 1.1.1 has been released. This is a maintenance release with various bug fixes.

```
Support for TLS1.0, TLS1.1 and TLS1.2
SNI, with and without wildcard certificates
Support for HAproxy's PROXY protocol
Safe for large installations: performant up to 15 000 listening sockets and 500 000 certificates.
Support for seamless run-time configuration reloads of certificates and listen endpoints.
```

## ソース取得
```
git clone [https://github.com/varnish/hitch](https://github.com/varnish/hitch)
cd ./hitch
./bootstrap
./configure
```

## configureするときとかに足りなかったパッケージ
```
$ sudo yum install python-docutils
$ sudo yum install libev-devel
$ sudo yum install openssl-devel
$ sudo yum install flex-devel
```

## configure
```
$ ./configure
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking for gcc... gcc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
checking for suffix of executables...
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for style of include used by make... GNU
checking dependency style of gcc... gcc3
checking for a BSD-compatible install... /usr/bin/install -c
checking whether make sets $(MAKE)... (cached) yes
checking for bison... bison -y
checking for flex... flex
checking lex output file root... lex.yy
checking lex library... none needed
checking whether yytext is a pointer... no
checking for rst2man... rst2man
checking whether to enable maintainer-specific portions of Makefiles... no
checking for ev_default_loop in -lev... yes
checking for SSL_CTX_free in -lssl... yes
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking for struct stat.st_mtim... yes
checking for struct stat.st_mtimespec... no
checking for stdlib.h... (cached) yes
checking for unistd.h... (cached) yes
checking for uid_t in sys/types.h... yes
checking for inline... inline
checking for int32_t... yes
checking for pid_t... yes
checking for size_t... yes
checking for ssize_t... yes
checking for uint32_t... yes
checking vfork.h usability... no
checking vfork.h presence... no
checking for vfork.h... no
checking for fork... yes
checking for vfork... yes
checking for working fork... yes
checking for working vfork... (cached) yes
checking for stdlib.h... (cached) yes
checking for GNU libc compatible malloc... yes
checking for stdlib.h... (cached) yes
checking for unistd.h... (cached) yes
checking for getpagesize... yes
checking for working mmap... yes
checking for inet_ntoa... yes
checking for accept4... yes
configure: creating ./config.status
config.status: creating Makefile
config.status: creating src/Makefile
config.status: creating config.h
config.status: executing depfiles commands

```


## make と make install
```
$ make
$ sudo make install
Making install in src
make[1]: ディレクトリ `/home/vagrant/hitch/src' に入ります
make  install-am
make[2]: ディレクトリ `/home/vagrant/hitch/src' に入ります
make[3]: ディレクトリ `/home/vagrant/hitch/src' に入ります
test -z "/usr/local/sbin" || /bin/mkdir -p "/usr/local/sbin"
  /usr/bin/install -c hitch '/usr/local/sbin'
make[3]: `install-data-am' に対して行うべき事はありません.
make[3]: ディレクトリ `/home/vagrant/hitch/src' から出ます
make[2]: ディレクトリ `/home/vagrant/hitch/src' から出ます
make[1]: ディレクトリ `/home/vagrant/hitch/src' から出ます
make[1]: ディレクトリ `/home/vagrant/hitch' に入ります
make[2]: ディレクトリ `/home/vagrant/hitch' に入ります
make[2]: `install-exec-am' に対して行うべき事はありません.
test -z "/usr/local/share/doc/hitch" || /bin/mkdir -p "/usr/local/share/doc/hitch"
 /usr/bin/install -c -m 644 hitch.conf.ex CHANGES.rst README.md '/usr/local/share/doc/hitch'
test -z "/usr/local/share/man/man8" || /bin/mkdir -p "/usr/local/share/man/man8"
 /usr/bin/install -c -m 644 hitch.8 '/usr/local/share/man/man8'
make[2]: ディレクトリ `/home/vagrant/hitch' から出ます
make[1]: ディレクトリ `/home/vagrant/hitch' から出ます
```

## hitch本体
```
/usr/local/sbin/hitch
```

## 設定ファイルを指定する場合
```
/usr/local/sbin/hitch --config=/hogehoge/hitch.conf

hitch --backend=[127.0.0.1]:80 --frontend=[*]:443 server.pem

./hitch --backend=0.0.0.0:80 \
    --frontend=[*]:443 \
    server.crt

hitch --backend=[127.0.0.1]:80 --frontend=[*]:443 example.pem\

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /home/hoge/ssl/hitch.key -out /home/hoge/ssl/nginx.crt
```

## プロセス443で起動させようとするとパーミッションエラーがでるからケーパビリティで特権を割り当てる
```
sudo setcap CAP_NET_BIND_SERVICE+ep /usr/local/sbin/hitch
sudo getcap /usr/local/sbin/hitch
```

```
$ sudo setcap -r /usr/local/sbin/hitch
$ sudo getcap /usr/local/sbin/hitch
```

[一般ユーザのプロセスをポート1024番未満でBindする方法]([http://roguer.info/2012/07/23/5727/](http://roguer.info/2012/07/23/5727/))

``
/usr/local/sbin/hitch --config=/home/hoge/hitch/hitch.conf
$ ps -aux | grep hitch
```

https://qiita.com/karumado/items/a2db63d1e5b2c9bdfa22 の転記

> Written with [StackEdit](https://stackedit.io/).
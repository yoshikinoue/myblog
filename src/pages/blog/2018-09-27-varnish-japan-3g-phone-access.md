---
templateKey: blog-post
title: Varnish4でガラケーIPアドレスのみアクセスを許可する
date: 2018-09-27T15:04:10.000Z
description: 古き良き日本のガラゲー携帯のIPからのアクセスだけを受けたい
tags:
  - Varnish
---

## 古き良き日本のガラゲー携帯のIPからのアクセスだけを受けたい
今時こんな情報を必要とする人いないと思うけど…。

## VCL

```c

acl docomo {
    include "docomo_iplist.vcl";
}

acl au {
    include "au_iplist.vcl";
}

acl softbank {
    include "softbank_iplist.vcl";
}

if (req.http.host ~ "^hogehoge.com") {
    if (
        !client.ip ~ admin &&
        !client.ip ~ docomo &&
        !client.ip ~ softbank &&
        !client.ip ~ au
    ) {
        return(synth(403,"Forbidden."));
    }
}
```

## docomo_iplist.vcl

```docomo_iplist.vcl
"210.153.84.0"/24;
"210.136.161.0"/24;
"210.153.86.0"/24;
"124.146.174.0"/24;
"124.146.175.0"/24;
"202.229.176.0"/24;
"202.229.177.0"/24;
"202.229.178.0"/24;
```

## ガラゲーIP確認先
- [Mobile Creation | ソフトバンク ]([http://creation.mb.softbank.jp/mc/tech/tech_web/web_ipaddress.html](http://creation.mb.softbank.jp/mc/tech/tech_web/web_ipaddress.html))
- [作ろうiモードコンテンツ：iモードセンタの各種情報 | サービス・機能 | NTTドコモ ]([https://www.nttdocomo.co.jp/service/developer/make/content/ip/](https://www.nttdocomo.co.jp/service/developer/make/content/ip/))
- [KDDI au: 技術情報 > IPアドレス帯域 ]([http://www.au.kddi.com/ezfactory/tec/spec/ezsava_ip.html](http://www.au.kddi.com/ezfactory/tec/spec/ezsava_ip.html))


https://qiita.com/karumado/items/847c0ccd0d9c2d95daff の転記

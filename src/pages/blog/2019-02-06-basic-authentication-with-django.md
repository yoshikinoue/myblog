---
templateKey: blog-post
title: Basic authentication with Django
date: 2019-02-06T01:47:08.095Z
description: DjangoでBasic認証を設定する。
tags:
  - django
---
一般には公開したくないがクライアントにテストページを見せる時に
Basic認証を掛ける場合がある。

ちなみにApacheやNginxを使ってる場合はWebサーバ側でBasic認証を設定したほうが楽です。

```middleware.py
from django.http import HttpResponse
from django.conf import settings

class BasicAuthMiddleware(object):    
    def unauthed(self):
        response = HttpResponse("""<html><title>Auth required</title><body>
                                <h1>Authorization Required</h1></body></html>""", mimetype="text/html")
        response['WWW-Authenticate'] = 'Basic realm="Development"'
        response.status_code = 401
        return response
    
    def process_request(self,request):
        if not request.META.has_key('HTTP_AUTHORIZATION'):
            
            return self.unauthed()
        else:
            authentication = request.META['HTTP_AUTHORIZATION']
            (authmeth, auth) = authentication.split(' ',1)
            if 'basic' != authmeth.lower():
                return self.unauthed()
            auth = auth.strip().decode('base64')
            username, password = auth.split(':',1)
            if username == settings.BASICAUTH_USERNAME and password == settings.BASICAUTH_PASSWORD:
                return None
            
            return self.unauthed()
```

```settings.py
BASICAUTH_USERNAME = 'ユーザ'
BASICAUTH_PASSWORD = 'パスワード'

MIDDLEWARE_CLASSES = (
    'middleware.BasicAuthMiddleware',   #追加
)
```

```wsgi.conf
#Basic認証を有効に
WSGIPassAuthorization On
```

下記サイトを参考にした。
- [http://d.hatena.ne.jp/yuheiomori0718/20130323/1364034960](http://d.hatena.ne.jp/yuheiomori0718/20130323/1364034960)
- [http://djangosnippets.org/snippets/2468/](http://djangosnippets.org/snippets/2468/)
- [http://asaby.hatenablog.com/entry/2012/12/03/215208](http://asaby.hatenablog.com/entry/2012/12/03/215208)

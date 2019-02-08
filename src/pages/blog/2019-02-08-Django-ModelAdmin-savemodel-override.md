---
templateKey: blog-post
title: django 管理画面でモデルの保存時に追加で処理したい
date: 2019-02-08T02:47:08.095Z
description: django 管理画面でモデルの保存時に追加で処理するためにsave_model関数をオーバーライドする
tags:
  - Django
---

`ModelAdmin` の `save_model` 関数オーバーライドすればできます。

データの更新時に日付だけ更新したい場合は、 `DateTimeField` の `auto_now=True` でモデル更新時に日付も更新することも可能です。
ただその場合は、`objects.filter().update()` では更新されない点を注意して下さい。

## model

```polls/models.py
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    update_date = models.DateTimeField('date updated')
    system_update_date = models.DateTimeField('date system_updated', auto_now=True)
```

## admin

```polls/admin.py
class QuestionAdmin(admin.ModelAdmin):
    """
    Questionモデルの管理画面
    """

    def save_model(self, request, obj, form, change):
        """
        save_model関数のオーバライド
        """

        obj.update_date = datetime.now() # update_date を現在の日時で更新

        super(QuestionAdmin, self).save_model(request, obj, form, change)

admin.site.register(Question,QuestionAdmin)
```

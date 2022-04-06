# kaminariの日本語化及びBootstrap4化
kaminariで日本語化したBootstrap形式のページネーション

## Bootstrap形式でページネーションを出力する
```shell
kaminari:views bootstrap4
```

### 日本語化する場合は以下が必要
注意点として他のメッセージも日本語化しなければならないので、必要に応じてfallbackを指定する
```ruby
# ローケールを日本語化
config.i18n.default_locale = :ja

# fallback
config.i18n.fallbacks = true
config.i18n.fallbacks = [:en]
```

```yaml
ja:
  views:
    pagination:
      first: "&laquo; 最初"
      last: "最後 &raquo;"
      previous: "&lsaquo; 前"
      next: "次 &rsaquo;"
      truncate: "..."
```

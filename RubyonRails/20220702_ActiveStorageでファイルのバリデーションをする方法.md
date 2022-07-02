# ActiveStorageでファイルのバリデーションをする方法

## has_one_attachedの場合
```ruby
  has_one_attached :avatars
  validate :avatar_type

  private

  def avatar_type
    if !avatar.blob.content_type.in?(%('image/jpeg image/png'))
      errors.add(:avatars, 'はjpegまたはpng形式でアップロードしてください')
    end
  end
```

`has_many_attached`の場合は、参考文献を参照してください

## 参考文献
[【Active Storage】ファイルアップロード時のバリデーション設定](https://qiita.com/sdkk-rails/items/8f5c006a1f052beb91ce)
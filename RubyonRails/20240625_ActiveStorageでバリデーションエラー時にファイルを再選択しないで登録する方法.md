# ActiveStorageでバリデーションエラー時にファイルを再選択しないで登録する方法
3.5 フォームのバリデーション
添付ファイルは、関連するレコードのsaveが成功するまでストレージサービスに送信されません。
つまり、フォームの送信がバリデーションに失敗すると、新しい添付ファイルは失われ、再度アップロードする必要があります。
ダイレクトアップロードではフォームの送信前に添付ファイルが保存されるので、これを使うことでバリデーションが失敗した場合でもアップロードが失われないようになります。
```ruby
    <%= f.label :image, '画像', class: 'col-sm-3 col-form-label' %>
    <div class="col-sm-9">
      <%if @post.image.attached? %>
         <%= image_tag @post.image, width: "200px" %>
      <% end %>
      <%= f.hidden_field :image, value: @post.image.signed_id if @post.image.attached? %>
      <%= f.file_field :image,direct_upload: true  %>
    </div>
```

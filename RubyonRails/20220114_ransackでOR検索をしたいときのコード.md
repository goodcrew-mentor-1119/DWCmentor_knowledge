# ransackでOR検索をしたいときのコード

m は、methodです。orかandを指定できます。
andも明示的に追加したい場合は、1を作成します。
```
group = {
  "0" => {
    m: "or",
    activity_monday_eq: params[:q][:activity_monday],
    activity_tuesday_eq: params[:q][:activity_tuesday],
    activity_wednesday_eq: params[:q][:activity_wednesday],
    activity_thursday_eq: params[:q][:activity_thursday],
    activity_friday_eq: params[:q][:activity_friday],
    activity_saturday_eq: params[:q][:activity_saturday],
    activity_sunday_eq: params[:q][:activity_sunday],
  }
}
# queryにgroupを与えると、ANDが上書きされOR検索として認識されます。
@q = Post.ransack(q: params[:q], g: group)
@results = @q.result
    
```

上記の場合、チェックボックスなどが引き継がれないので、以下のように対策します。
```
<div class="search_form">
  <%= search_form_for @q, url: search_posts_path do |f| %>
    <ul>
      <li class="search-nav-content">
        <p><%= f.label "営業日" %></p>
        <span class="checkbox-inline">
              <% %w[activity_monday activity_tuesday activity_wednesday activity_thursday activity_friday activity_saturday activity_sunday].each_with_index do |day,i| %>
                <%= f.check_box "#{day}".to_sym, {checked: params[:q]["#{day}".to_sym] ? true : false}, "true", nil%>
            <%= f.label day.to_sym, ["月", "火", "水", "木", "金", "土", "日"][i] %>
              <% end %>
            </span>
      </li>
      <li class="search-nav-content">
        <p><%= f.label "ジャンル" %></p>
        <%= f.collection_check_boxes :post_tags_tag_id_in , Tag.all, :id, :genre, {} do |c| %>
          <%= c.label {c.check_box + c.text} %>
        <% end %>
      </li>
      <li class="search-nav-content">
        <p><%= f.label :address_start, "所在地" %></p>
        <%= f.search_field :address_start, placeholder: "東京都" %>
      </li>
    </ul>
    <%= f.submit '検索' %>
  <% end %>
</div>
```

アソシエーション先で、データを検索したい場合は、:post_tags_tag_id_inなどとすればアソシエーションをたどることができます。
この場合、PostTag -> tag_idを探しに行きます。

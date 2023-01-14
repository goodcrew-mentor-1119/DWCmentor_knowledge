```
# latest: trueなどが追加しておきたい部分です。
<%= link_to hoge_path(q: params[:q].blank? ? nil, params[:q].permit!, latest: true) %>
```

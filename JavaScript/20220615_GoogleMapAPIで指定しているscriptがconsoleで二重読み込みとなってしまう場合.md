# GoogleMapAPIで指定しているscriptがconsoleで二重読み込みとなってしまう場合

```javascript
if(window.google){
    initMap();
} else{
    $.ajax('https://maps.googleapis.com/maps/api/js?key=<%= ENV["GOOGLE_MAP_API_KEY"] %>signed_in=false&callback=initMap&libraries-places', {
        crossDomain: true,
        dataType: 'script'
    });
}
```

### 仕組み
1. 画面全体でGoogleAPIがすでに有効ならば、initMap()をコール
2. まだ、GoogleAPIが読み込まれていない場合ajaxでscriptを読み込み反映させる

## 参考資料
 - https://stackoverflow.com/questions/33115375/prevent-google-maps-js-executing-multiple-times-caused-by-rails-turbolinks
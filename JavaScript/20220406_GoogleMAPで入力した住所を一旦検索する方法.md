# GoogleMAPで入力した住所を一旦検索する方法

```javascript
<script type="text/javascript">
    function initMap(){
        map = new google.maps.Map(document.getElementById('map'), {
        center: {lat: 35.6585805, lng: 139.7432442},
        zoom: 15,
    });
  };
    $(function(){
        $('#searchAddressBtn').click(function() {
        var address = $('#post_summary_post_outside_attributes_address').val();
        var geocoder = new google.maps.Geocoder();
        geocoder.geocode(
            {
                'address' : address,
                'region' : 'jp'
            },
            function (results, status) {
                if (status == google.maps.GeocoderStatus.OK) {
                    // 住所のデータを取得できた時
                    // 取得した座標をマップに反映
                    map.setCenter(results[0].geometry.location);
                    // 取得した座標をマーカーに反映
                    var pos = new google.maps.LatLng(results[0].geometry.location.lat(), results[0].geometry.location.lng());
                    var markerCenter = new google.maps.Marker({
                        position: pos,
                        map: map,
                    });
                } else {
                    // 失敗した時
                    alert('住所検索に失敗しました。<br>住所が正しいか確認してください');
                }
            });
        });
    });
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=<%= ENV['GOOGLE_MAP_KEY'] %>&callback=initMap" async defer></script>
```
# Turbolinksが邪魔をしてテンプレート内の外部scriptが複数回呼び出されてしまう問題を解決する方法

```html
<script src="hoge.js" data-turbolinks-eval="false"></script>
```

参考文献
 - https://stackoverflow.com/questions/38941286/rails-5-turbolinks-5-and-google-maps-how-to
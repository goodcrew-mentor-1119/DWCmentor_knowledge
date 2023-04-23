```
  $(window).on('scroll', function() {
    // タブ1のスクロールを処理する
    if ($('#tab1-list').hasClass('active')) {
      var scrollHeight = $(document).height();
      var scrollPosition = $(window).height() + $(window).scrollTop();
      if ((scrollHeight - scrollPosition) / scrollHeight <= 0.05) {
        $('.jscroll').jscroll({
          contentSelector: '.scroll-list',
          nextSelector: '.tab1 span.next:last a'
        });
      };
    }

    // タブ2のスクロールを処理する
    if ($('#tab2-list').hasClass('active')) {
      var scrollHeight2 = $(document).height();
      var scrollPosition2 = $(window).height() + $(window).scrollTop();
      if ((scrollHeight2 - scrollPosition2) / scrollHeight2 <= 0.05) {
        $('.jscroll_2').jscroll({
          contentSelector: '.scroll-list_2',
          nextSelector: '.tab2 span.next:last a'
        });
      };
    }

    // タブ3のスクロールを処理する
    if ($('#tab3-list').hasClass('active')) {
      var scrollHeight3 = $(document).height();
      var scrollPosition3 = $(window).height() + $(window).scrollTop();
      if ((scrollHeight3 - scrollPosition3) / scrollHeight3 <= 0.05) {
        $('.jscroll_3').jscroll({
          contentSelector: '.scroll-list_3',
          nextSelector: '.tab3 span.next:last a'
        });
      };
    }

    // タブ4のスクロールを処理する
    if ($('#tab4-list').hasClass('active')) {
      var scrollHeight4 = $(document).height();
      var scrollPosition4 = $(window).height() + $(window).scrollTop();
      if ((scrollHeight4 - scrollPosition4) / scrollHeight4 <= 0.05) {
        $('.jscroll_4').jscroll({
          contentSelector: '.scroll-list_4',
          nextSelector: '.tab4 span.next:last a'
        });
      };
    }
  });
```

カリキュラムにあるタブ機能の実装をした後、タブリンクにidを持たせ、そのactive状態を把握させる。  
すべてを独立したjScrollのものとして認識させて、対応することで解決します。
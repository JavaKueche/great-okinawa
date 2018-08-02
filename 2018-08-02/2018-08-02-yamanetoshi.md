### [やまねとしあき](https://twitter.com/yamanetoshi)

- [今日やること](https://github.com/JavaKueche/great-okinawa/issues/14)

### Github Pages なサイト不具合修正対応 (https://weblog.metacircular-evaluator.org)

Hugo を使用しております。不具合としては、一覧画面と詳細画面で同じレイアウトを遣っているのですが、一部の img な要素が一覧では正常表示されているものの、詳細画面ではリンク切れになっている、というものでした。

とりあえず `config.yaml` および `themes/Hugo-Octopress/layouts/partials/sidebar.html` に img の URL が記述されていたのでそちらを修正しています。

相対パスで記載していると詳細画面で正常に表示できないようで、Hugo に乗り換えた時点で出ていた不具合だったようですorz

### 一手ヨセコウ、本コウの違いについて確認

囲碁のルールにコウ、というものがあります。

- [コウ(wikipedia)](https://ja.wikipedia.org/wiki/%E3%82%B3%E3%82%A6)
- [コウのやさしい説明](http://www.pandanet.co.jp/igonyumon/06-01.htm)

最近、あまり正確に理解していなかったことが判りまして、時間を遣って確認してみようと思った次第です。

- [コウについて」(http://www5.plala.or.jp/hasebehp/igm/igm_kou.htm)

特に理解が微妙だったのが上の記事にも説明がありますが、本コウとヨセコウ、というものでした。

- [本コウとヨセコウ](https://blog.goo.ne.jp/liebestraum777/e/8affece3d7b708a458101ce2ac3b76d3)

確認した上記資料を踏まえて詰碁の問題を確認してみます。

### 前回の続き

どこからどこを読んでまとめているか、な記憶が飛んでてアレorz

### と思ったら

第一部、のメモになっていました。ので、第三部から着手します。以下を使います。

- https://colab.research.google.com/

### 第一章

は単純な 1 変量データの集計方法、とのこと。

- https://gist.github.com/yamanetoshi/0c70e54653a498a2ceb8069ee10b7e5c

### 第二章

多変量データについて、整然データの作り方など、とのこと。ファイルの読み込みでハマっています。とりあえず upload すれば良いらしい。

- https://qiita.com/uni-3/items/201aaa2708260cc790b8

おそらく、ハマッてるとこで timeup になるはず。

### 原因

これだった模様。

- [Upload local files using Google Colab](https://stackoverflow.com/questions/48420759/upload-local-files-using-google-colba)

upload できましたが二章は来週? になるのかどうか。
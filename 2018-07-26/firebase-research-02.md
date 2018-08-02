## データベースの構造化
- データをネストしない
  - fetchしたときに不必要に大量の下位ノードを取得してしまうかのうせいがある
  - そのため、なるべくデータ構造は平坦にする

## FirebaseでWebチャットアプリをデプロイするまで（1時間コース）
### qiitaの記事見つけたのでチャレンジ
https://qiita.com/st5757/items/9e651e8cffaa90681426

- チャットを試す
https://github.com/firebase/friendlychat-web

- 認証について
たくさんの認証方法から選べる
![](https://www.dropbox.com/s/c3xtq46ruwx9sd6/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202018-07-26%2020.42.10.png?dl=0)

- エラー発生
Error: An unexpected error has occurred.
  - 権限の問題だった
  - error.logができてた

- localhost:5000 でアクセスOK

- 上記は古い情報だったのでオフィシャルでやったほうがよさそう...

### オフィシャルの記事でお試し
https://codelabs.developers.google.com/codelabs/firebase-web/#0

- sign in のボタンがでた
![](https://www.dropbox.com/s/l9122ebmf5wgvbs/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202018-07-26%2021.14.49.png?dl=0)

- ログインできた
![](https://www.dropbox.com/s/tujezgf99cg59ox/.2018-07-26-000000.md.un~?dl=0)

- チャットの実装のまえに
  - 初期データの流し込み
    下記でOK  
    ![](https://www.dropbox.com/s/fttpe12abtpbzy7/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202018-07-26%2021.23.04.png?dl=0)
    

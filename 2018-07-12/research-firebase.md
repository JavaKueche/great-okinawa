# Firebaseについて調べる

## 背景
サーバーサイドをWAFとかでやるのだるくなってきたので、Firebaseにまかせてしまいたい

## Firebase
### Firebaseのサービスの種類
たくさんある.
アプリ開発に関しては下記
- Realtime Database
  アプリデータを瞬時に保存および同期
- Crashlytics
  リアルタイム クラッシュ レポートで問題に優先順位を付けて解決する
- Cloud Firestore ベータ版
  グローバル スケールでアプリデータを保存および同期
- Authentication
  ユーザーを簡単かつ安全に認証
- Cloud Functions ベータ版
  サーバーの管理なしでモバイル バックエンド コードを実行
- Cloud Storage
  Google の規模でファイルを保存、配信
- Hosting
  ウェブアプリ アセットをすばやく安全に配信
- Test Lab for Android
  Google がホストする端末でアプリをテスト
- Performance Monitoring ベータ版
  アプリのパフォーマンスに関する分析情報を取得

主要機能である **Realtime database** をつかってみる

### Realtime database 概要
```
NoSQL クラウド データベースでデータの保管と同期を行うことができます。データはすべてのクライアントにわたってリアルタイムで同期され、アプリがオフラインになっても、利用可能な状態を保ちます。

Firebase Realtime Database はクラウドでホスティングされるデータベースです。データは JSON として保存され、接続されているすべてのクライアントとリアルタイムで同期されます。iOS、Android、JavaScript SDK を使用してクロスプラットフォーム アプリを構築した場合、すべてのクライアントが、1 つの Realtime Database インスタンスを共有して、最新のデータによる更新を自動的に受信します
```

#### 主な機能
- リアルタイム. クライアントは常にデータを同期できる
- オフライン. オフラインでもデータは永続化され、ネットに繋がると同期する
- クライアント端末からアクセス可能. モバイルもウェブもOK
- 複数のデータベースにわたるスケーリング. Blanzeプランだとスケーリング可

#### その他のホスティング
- Firebase Remote Config は、アプリのアップデートをユーザーにダウンロードしてもらわなくてもアプリの動作と外観を変更できるように、デベロッパーが指定した Key-Value ペアを格納します。
- Firebase Hosting はウェブサイト向けの HTML、CSS、JavaScript に加えて、デベロッパーが提供するその他のアセット（画像、フォント、アイコンなど）もホストします。
- Cloud Storage は、画像、動画、音声などのファイルや、ユーザーが作成したその他のコンテンツを保存します。

#### データベースの選択
Firebaseには2つのデータベースが提供されている
- Realtime Database は Firebase の元のデータベースです。これは、リアルタイムでクライアント全体の状態を同期させる必要があるモバイルアプリ向けの効率的で低レイテンシのソリューションです。
- Cloud Firestore は、Firebase のモバイルアプリ開発用の新しい主力データベースです。直感的な新しいデータモデルで、Realtime Database の成果をさらに向上しています。Cloud Firestore は、Realtime Database よりも豊かで高速なクエリとスケールを備えています。
- データベースの構造化
Firebase Realtime Database のデータは JSON オブジェクトとして保存されます。つまり、このデータベースはクラウドにホストされた JSON ツリーであると考えることができます。SQL データベースとは異なり、テーブルやレコードはありません。

特に理由がなければ **Cloud Firestore** でよさそう


### Webアプリクライアントから実装するには

- Quickstartなsampleあり
https://github.com/firebase/quickstart-js/tree/master/firestore

- サンプルを動かす
```
yarn add -g firebase-tools
firebase login
firebase use --add # プロジェクトの選択
```

- 自分の場合はReact + redux + ES2016で試したい
```
yarn init
yarn add create-react-app
yarn add firebase
```

### 参考
- [Firebase Realtime DBを実践投入するにあたって考えたこと](https://qiita.com/1amageek/items/64bf85ec2cf1613cf507)


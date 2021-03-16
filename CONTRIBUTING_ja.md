maps-app-javascript へのコントリビュート
=================================

Esri では、どなたからの投稿も歓迎しています。投稿に関する[ガイドライン](https://github.com/esri/contributing)をご覧ください。

 1. [参加する](#参加する)
 2. [バグの報告](#バグの報告)
 3. [コードを提供する](#コードを提供する)

## 参加する

ArcGIS API for JavaScript で最高のマップ アプリを作るためには、サード パーティのパッチは絶対に欠かせません。しかし、maps-app-javascript の開発に参加する方法はそれだけではありません。[バグの発見と報告](#バグの報告)、[ドキュメントの改善](#ドキュメントの改善)、[GitHub での問題解決](https://github.com/Esri/maps-app-javascript/issues)の支援、[@ArcGISJSAPI](https://twitter.com/ArcGISJSAPI) へのツイート、そして同僚や友人に maps-app-javascript と [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/) についての情報を広めることで、プロジェクトを大いに支援することができます。

## バグの報告

プロジェクトの [issues ページ](https://github.com/Esri/maps-app-javascript/issues)にバグを報告する前に、まず、問題がアプリケーションコードではなく、maps-app-javascriptに起因するものであることを確認してください。次に、報告されている問題を検索し、すでに報告されている問題であれば、コメントに追加の詳細を追加してください。

また、maps-app-javascriptに関連する問題のみを報告してください。ArcGIS API for JavaScript に関連する問題の場合は、[Esri Tech Support](https://support.esri.com/contact-tech-support) に問い合わせるか、[GeoNet](https://geonet.esri.com/community/developers/web-developers/arcgis-api-for-javascript) の Esri コミュニティに質問してください。

maps-app-javascript の新しいバグを発見したことを確認した後、問題を作成する際に提供されている問題テンプレートを使用してください。

## コードを提供する

### 開発環境のセットアップ
開発環境を整えるために、[Readme](https://github.com/Esri/maps-app-javascript/blob/master/README.md) に記載されている説明をお読みください。

#### リポジトリのフォーク
まだの方は、https://github.com/Esri/maps-app-javascript のページある「[Fork](https://github.com/Esri/maps-app-javascript/fork)」ボタンをクリックしてください。

#### リポジトリをクローンする
リポジトリをクローンして、`npm install` を実行します。

* Windowsユーザーの方は、このプロジェクトのnpmモジュールをコンパイルするに [Windows-Build-Tools](https://github.com/felixrieseberg/windows-build-tools) をインストールする必要があります。
* `npm install --global --production windows-build-tools`

* `npm start` - アプリケーションをコンパイルして、ローカルサーバー（`http://localhost:8080/`）で実行します。

* `npm run build` - デプロイ用にアプリケーションをコンパイルします。

* `npm test` - ローカルの chrome ドライバで単体テストを実行します。

* `npm run serve` - アプリケーションのプロダクションビルドを実行しますが、ビルドされたアプリケーションがどのように動作するかを確認するために、ローカルでそれを提供します。

`npm run serve` は、`webpack-dev-server` の自己署名証明書を使って Service Workers が正しく動作するかどうかを完全にテストするために使用します。開発目的で適切なフラグを有効にして Chrome を実行する方法については、[こちらの記事](https://deanhume.com/testing-service-workers-locally-with-self-signed-certificates/)を参照してください。

#### リモートの設定
複製プロセスが作成したディレクトリ (maps-app-javascript) に移動し、ローカルの git がすべてのリモートとブランチを認識していることを確認します。

$ cd maps-app-javascript
# プロンプトに表示されているアクティブディレクトリを、新しくクローンされた "maps-app-javascript" ディレクトリに変更します。
$ git remote add upstream https://github.com/Esri/maps-app-javascript.git
# オリジナルのリポジトリを "upstream" という名前のリモートに割り当てます。
$ git fetch upstream
# ファイルに手を加えることなく、ローカルリポジトリに存在しない変更を取り込む
```

* [GeoNet](https://community.esri.com/groups/arcgis-example-apps) の Example Apps グループで Esri コミュニティに質問してみましょう。
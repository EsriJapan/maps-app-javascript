[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)

# Maps App JavaScript

このリポジトリでは、[ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/) にて構築された地図アプリをすぐに使用できる [Maps App](https://developers.arcgis.com/example-apps/maps-app-javascript/?utm_source=github&utm_medium=web&utm_campaign=example_apps_maps_app_javascript) というアプリを提供します。
Maps App をそのまま使うことや、ArcGIS API for JavaScript を使用して拡張することができます。

<!-- MDTOC maxdepth:6 firsth1:0 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [機能](#機能)   
- [詳細なドキュメント](#詳細なドキュメント)   
- [使用方法](#使用方法)   
- [デモ](#デモ)   
- [問題](#問題)   
- [貢献](#貢献)   
- [MDTOC](#mdtoc)   
- [ライセンス](#ライセンス)   

<!-- /MDTOC -->
---

## 機能

 * 動的なベースマップの変更
 * 場所検索
 * ルート案内
 * ArcGIS へのサインイン
 * Service Worker
 * AppCache
 * `manifest.json` - ホーム画面にボタンの追加
 * デフォルトアイコン

このアプリケーションは、開発目的のために多くの技術を活用しています。ソースコードと付随するファイルをコンパイルし、まとめて提供すために [webpack](https://webpack.js.org/) を利用します。 ソースコードは [TypeScript](http://www.typescriptlang.org/) にて記述されており、 [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/) を使用して[custom widgets](https://developers.arcgis.com/javascript/latest/guide/custom-widget/index.html)を作成する方法を紹介しています。

このアプリケーションは、 [Workbox 用 Webpack](https://developers.google.com/web/tools/workbox/get-started/webpack) を使用して、アプリケーションの [service workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) をセットアップし、ソースコードとファイルをキャッシュします。また、Internet Explorer、Edge、Safari の[ appcache fallback  ](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache)を使用します。

[Intern](https://theintern.io/) は、全ての単体テストとコードカバレッジにて使用されます。

このプロジェクトを独自のアプリケーションの出発点としてご自由にご利用ください。

## 詳細なドキュメント

アーキテクチャや ArcGIS の活用方法、アプリをすぐに開始する方法など、アプリケーションの詳細につきましては、 [ドキュメント](./docs/README.md) を参照してください。

## 使用方法

リポジトリを Clone して `npm install` を実行します。

* _Windows ユーザの方_ - このプロジェクトの npm モジュールをコンパイルするには [Windows-Build-Tools](https://github.com/felixrieseberg/windows-build-tools) のインストールが必要になります。 `npm install --global --production windows-build-tools`

* `npm start` - アプリケーションをコンパイルして、ローカルサーバ( `http://localhost:8080/` )にて実行します。
* `npm run build` - デプロイ用にアプリケーションをコンパイルします。
* `npm test` - ローカルの Chrome ドライバにて単体テストを実行します。
* `npm run serve` - アプリケーションの本番環境用にビルドを実行しますが、ローカルで提供して、ビルドされたアプリがどのように動作するかを確認します。

ServiceWorker が `webpack-dev-server` の自己署名証明書で正しく動作しているかどうかを完全にテストするには `npm run serve` を使用します。 開発目的で適切なフラグを有効にしてChromeを実行する方法については、この [記事](https://deanhume.com/testing-service-workers-locally-with-self-signed-certificates/) を参照してください。

* [ArcGIS for Developers](https://developers.arcgis.com/) にログインし、アプリを[登録](https://developers.arcgis.com/applications/#/) してクライアントIDを発行します。

* ArcGIS Platform からプレミアムな[ルート案内やルート検索](https://developers.arcgis.com/features/directions/)、[ジオコーディング](https://developers.arcgis.com/features/geocoding/)を利用するために、クライアントIDとともにアプリを登録する必要があります。

![](images/Register1.png)
* Maps App のバージョンを登録したら、登録情報からクライアントIDのコピーを取得し、`src/app/config.ts` というファイルの `appId` という定数にコピーしたクライアントIDを設定します。 `portalUrl` という定数には、 `"https://<MY-ORGANIZATION>.maps.arcgis.com"` といった、組織のポータル URL を指定する必要があります。 また、独自のWebMapを提供することや、提供されているデフォルトのWebMapを使用することもできます。

```js
// src/app/config.ts
/**
 * Registered application id.
 * This is needed to be able to use premium
 * services such as routing and directions.
 */
export const appId = "<APP-ID>";

/**
 * Users Portal URL.
 */
export const portalUrl = "https://arcgis.com"; // default Portal URL

/**
 * WebMap id to use for this application.
 * You can update this WebMap id with your own.
 */
export const webMapItem = {
  portalItem: {
    // shared WebMap with Vector Tile basemap
    id: "1aab2defd7444b6790f439a186cd4a23"
  }
};
```

* 登録プロセスの一環として、アプリのリダイレクト URI を追加します。 登録ページの下部にあるリダイレクト URI セクションに移動し、開発目的で示されているようにリダイレクト URI を設定します。 また、アプリケーションがデプロイされる場所のリダイレクト URI を追加することもできます。 このリダイレクト URI は、 `https：// www.arcgis.com` のデフォルトのリダイレクトです。 

開発の用途では、登録したアプリに以下のいずれかのリダイレクト URL を追加する必要があります:

* `http://127.0.0.1:8080`
* `http://localhost:8080`

アプリケーションをデプロイするときは、本番環境と同じアプリは開発に使用しないでください。 登録したアプリには、本番Webサイトのみをリダイレクトする必要があります。 

![](images/Register2.png)

## デモ

![application](images/maps-app.gif)

## 問題

バグの発見や、新機能の強化をリクエストしたい場合は、問題を提出してお知らせください。

## 貢献

どなたでも[投稿](CONTRIBUTING.md)を歓迎いたします。 プルリクエストを受け付けています。

1. 参加する
2. 問題点の報告
3. コードの投稿
4. ドキュメントの改善

## MDTOC

このリポジトリ内のドキュメントの目次の生成は、[Atom の MDTOC パッケージ](https://atom.io/packages/atom-mdtoc)を使用して作成されています。 

## ライセンス

Copyright 2018 Esri

Apache License Version 2.0（「本ライセンス」）に基づいてライセンスされます。あなたがこのファイルを使用するためには、本ライセンスに従わなければなりません。本ライセンスのコピーは下記の場所から入手できます。

http://www.apache.org/licenses/LICENSE-2.0

適用される法律または書面での同意によって命じられない限り、本ライセンスに基づいて頒布されるソフトウェアは、明示黙示を問わず、いかなる保証も条件もなしに「現状のまま」頒布されます。本ライセンスでの権利と制限を規定した文言については、本ライセンスを参照してください。

ライセンスのコピーは本リポジトリの[ライセンス](./LICENSE) ファイルで利用可能です。

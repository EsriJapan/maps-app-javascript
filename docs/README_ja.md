# 説明

この ArcGIS API for JavaScript にて構築されたアプリを使用して、組織の権限のあるマップデータを作業者に共有してください。 構築するアプリケーションには、[ArcGIS Online の組織](https://doc.arcgis.com/en/arcgis-online/reference/what-is-agol.htm) で作成したカスタムの Web マップを含めることができます。 例として、Living Atlas の [Web マップ](http://doc.arcgis.com/en/living-atlas/item/?itemId=26888b0c21a44eb1ba2f26d1eb7981fe) をアプリの開始場所として使用できます。 Maps App には ArcGIS Online の強力なサービスまたは独自のサービスを使用した場所の検索やルート機能の例も含まれています。 また、組織で設定したベースマップを利用して、ユーザーが自分にとって意味のあるベースマップに切り替えることもできます。

この例のアプリはオープンソースのため、コードを入手してあなたの組織に合わせてアプリを構成するか、似たような機能を自分のアプリに統合する方法を学びます。

<!-- MDTOC maxdepth:6 firsth1:0 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [機能紹介](#機能紹介)   
- [Web マップの使用方法](#Webマップの使用方法)   
- [組織のベースマップへのアクセス](#組織のベースマップへのアクセス)   
- [Identity](#Identity)   
- [場所検索](#場所検索)   
- [リバース ジオコーディング](#リバースジオコーディング)   
- [ルート](#ルート)   

<!-- /MDTOC -->
---

## 機能紹介

- ベースマップの切り替え
- 組織からの Web マップの読み込み
- 住所または場所の検索 (ジオコーディング)
- マップ上の位置検索 (リバース ジオコーディング)
- ルーティングとターンバイターン形式経路
- OAuth2 による認証

![Application](./images/application.png)

## Web マップの使用方法

ArcGIS Online または ArcGIS Pro にて独自の Web マップを作成し、ArcGIS Online にて所属している組織のアプリで共有できます。これが ArcGIS に組み込まれた Web GIS モデルの中心的な機能です。 Web マップを使用するアプリを構築すると、コードではなく、地図作成やマップの構成を ArcGIS Online で完結できます。 これにより、コードの変更やアプリの更新を行うことなく、時間経過とともに地図を変更することが可能となります。 Web マップを使用して開発する利点について詳しくは、[こちら](https://developers.arcgis.com/web-map-specification/) をご参照ください。 また、 [ArcGIS Online](http://doc.arcgis.com/en/arcgis-online/create-maps/make-your-first-map.htm)  および [ArcGIS Pro](http://pro.arcgis.com/en/pro-app/help/mapping/map-authoring/author-a-basemap.htm) で Web マップを作成する方法についてもご参照ください。

コード内の Web マップを読み込むことは簡単です。Maps App は、下記のコードを使用してポータルから Web マップを読み込みます（ユーザーがサインインする必要がある場合があります。）。

```ts
// config.ts
export const webMapItem = {
  portalItem: {
    // shared WebMap
    id: "3ff64504498c4e9581a7a754412b6a9e"
  }
};

// Application.ts
@subclass("app.widgets.Application")
class Application extends declared(Accessor) {
  @property({ readOnly: true })
  webmap = new WebMap(webMapItem);
  ...
}
```

![Webmap Browser](./images/webmap-browser.png)

## 組織のベースマップへのアクセス

ArcGIS Online の組織またはポータルの管理者は、[グループ](http://doc.arcgis.com/en/arcgis-online/share-maps/share-items.htm)を介してユーザーが切り替えることができるベースマップを構成できます。アプリケーションは、[Portal API](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html) を使用してこの構成を利用できます。Maps App では、 ArcGIS API for JavaScript の [Basemap Gallery](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapGallery.html) ウィジェットを使用します。

必要な操作はインスタンス化するだけで、ユーザーがサイン インすると、ユーザーの組織に合わせて構成されたベースマップに切り替わります。

```ts
const basemapGallery = new BasemapGallery({
  container: element(),
  view
});
```

## ID

Maps App は ArcGIS [Identity](https://developers.arcgis.com/authentication/)  モデルを利用して、[指定されたユーザー](https://developers.arcgis.com/authentication/#named-user-login)のログインパターンを介して、リソースへのアクセスを提供します。

[ルート検索](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Directions.html)ウィジェットにアクセスするには、提供された認証用ウィジェットにてサインインする必要があります。サインインすることで、カスタム WebMapBrowser ウィジェットと同様にルート検索ウィジェットも利用可能になります。

ルート検索ウィジェットは、 ArcGIS Online 及び カスタムのネットワーク解析ルートサービスを利用して、運転案内と歩行案内を構築する方法を提供します。デフォルトでは、[Esri World Route Service](http://www.arcgis.com/home/item.html?id=1feb41652c5c4bd2ba5c60df2b4ea2c4) が使用されます。

WebMapBrowser ウィジェットは Maps App のカスタムウィジェットで、組織のコンテンツの一部として Web マップの閲覧のために使用されます。
WebMapBrowser ウィジェットを使用すると、アプリケーションで使用される Web マップを切り替えることができます。

![ルート検索](./images/identity.png)

設定ファイルに有効なクライアント ID と、デフォルトとは異なるポータル URL を設定するだけで、アプリケーションの認証ワークフローによって使用されます。

```ts
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
```

## 場所検索

[検索](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html) ウィジェットは、住所や地名を特定の地理的な位置に変換するために使用されます。逆に、地理的な位置を使用して、住所や地名などの場所の説明を検索することができます。検索ウィジェットは、[Esri の World Geocoding Service](https://developers.arcgis.com/features/geocoding/) によって提供されるジオコーディング及びリバースジオコーディングの機能を実行します。

検索ウィジェットで使用するソースをカスタマイズして、独自のカスタムジオコードまたはフィーチャーサービスをソースとして使用できます。

[Suggestions](https://developers.arcgis.com/rest/geocode/api-reference/geocoding-suggest.htm) は、[検索ウィジェット](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html#suggestions)にてすぐにサポートされます。提供されるカスタムソースが定義されていない場合、返される Suggestions の [最小候補](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html#minSuggestCharacters)及び[最大候補](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html#maxSuggestions)をカスタマイズできます。

## リバースジオコーディング

Maps App の MapView にて [`"hold"`](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#event:hold) イベントを使用して、選択した場所を検索ウィジェットの[検索](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html#search)メソッドで送信して、選択した場所をリバースコーディングします。

```ts
// src/app/mapactions/reverseGeocode.ts

@subclass("app.mapactions.ReverseGeocode")
export class ReverseGeocode extends declared(MapAction)<esri.SearchResponse> {
  @property() search: Search;

  constructor(params?: ReverseGeocodeOptions) {
    super(params);
    this.reverseGeocode = this.reverseGeocode.bind(this);
    if (this.view) {
      this.addListeners();
    }
    this.watch("view", () => {
      this.addListeners();
    });
  }

  addListeners() {
    const handler = this.view.on("hold", this.reverseGeocode);
    this.handlers.push(handler);
  }

  async reverseGeocode({ mapPoint }: GeocodeOptions) {
    const response = await this.search.search(mapPoint);
    this.value = response;
  }
}

```

## ルート

Maps App でナビゲーションの[方向](https://developers.arcgis.com/features/directions/)を取得することは [ArcGIS Online](http://doc.arcgis.com/en/arcgis-online/use-maps/get-directions.htm) と同様に、 [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/latest/index.html) でも容易です。 ナビゲーションサービスを組織用に[カスタマイズ](http://doc.arcgis.com/en/arcgis-online/administer/configure-services.htm#ESRI_SECTION1_567C344D5DEE444988CA2FE5193F3CAD)したり、組織のワークフローを反映した新しい移動モードを加えたり、組織のワークフローに適さない移動モードを削除することができます。

Directions ウィジェットは、[デフォルトのルートサービス URL](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Directions.html#routeServiceUrl) を組織のURLに更新できます。

![ルート](./images/route.png)

ArcGIS API for JavaScript の使用に関する他のサンプルについては、[ドキュメント](https://developers.arcgis.com/javascript/latest/sample-code/index.html)を参照してください。

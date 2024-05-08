# PLATEAU-3DTiles/MVT 配信チュートリアル

## 1. PLATEAU-3DTiles/MVT の概要

Project PLATEAUでは、CityGML形式で作成された3D都市モデルのデータを3D Tiles 1.0及びMVT（Mapbox Vector Tiles）形式に変換し、それぞれ配信を行っています。

本チュートリアルでは、3D TileおよびMVTの利用方法について解説します。

## 2. CityGMLの3D Tiles/MVTへの変換

CityGMLを3D Tiles形式に変換するためには、以下の方法があります。詳しい情報は、それぞれのタイトルをクリックしてください。

- [PLATEAU GIS Converter](https://github.com/Project-PLATEAU/PLATEAU-GIS-Converter) : FY2023 の Project PLATEAU「都市デジタルツインの実現に向けた研究開発及び実証調査業務」（内閣府/研究開発とSociety5.0との橋渡しプログラム（BRIDGE））において開発されたOSSの変換ツール
- [FME](https://github.com/Project-PLATEAU/FMEscript-CityGML-to-3DTiles)：Project PLATEAU で利用したCityGMLから3D TilesへのFME変換テンプレート
- [Cesium ion](https://cesium.com/learn/3d-tiling/ion-tile-3d-buildings/)：オンラインサービスを利用したデータ変換とホスティング
- [Cesium ion オンプレミス](https://cesium.com/platform/on-premises-products/)：3D Tiles変換用の有償のコマンドラインツール
- [citygml-to-3dtiles](https://github.com/njam/citygml-to-3dtiles)：データ変換用オープンソースツール

## 3. PLATEAU-3DTiles/MVT の利用方法

PLATEAU-3DTiles/MVT の配信サービスを利用することで、独自に立ち上げたCesiumJS等の環境で3D都市モデルをすぐに利用することができます。詳しくは、本ドキュメントの末尾にある「配信データの取得方法」をご覧ください。

なお、本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことをご了承ください。またデータの内容は予告なく更新されることがあります。

## 3.1. CesiumJSでの利用方法

3D Tiles/MVT形式のデータは、PLATEAU VIEW上でデータカタログから選択したり、CesiumJSを利用したコードを作成したりすることで表示できます。ただし、MVTは現在CesiumJSではサポートされていないため、別途外部ライブラリが必要になります（ここでは[cesium-mvt-imagery-provider](https://github.com/reearth/cesium-mvt-imagery-provider)を使用）。

以下は、CesiumJSを利用して3D Tilesを表示させるためのサンプルコードです。この例では、東京都千代田区の建築物モデル（3D Tiles）を利用していますが、下記の「配信データの取得方法」にあるURLに置き換えることで、様々なデータを表示することができます。

```html
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>PLATEAU-3DTiles、PLATEAU-Ortho、PLATEAU-TerrainをCesiumで表示</title>
  <script src="https://cesium.com/downloads/cesiumjs/releases/1.117/Build/Cesium/Cesium.js"></script>
  <link href="https://cesium.com/downloads/cesiumjs/releases/1.117/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
  <script src="https://unpkg.com/cesium-mvt-imagery-provider@1.4.1/dist/cesium-mvt-imagery-provider.umd.js"></script>
  <style>
    #cesiumContainer {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
      margin: 0;
      overflow: hidden;
      padding: 0;
      font-family: sans-serif;
    }
    html {
      height: 100%;
    }
    body {
      padding: 0;
      margin: 0;
      overflow: hidden;
      height: 100%;
    }
  </style>
</head>
<body>
  <div id="cesiumContainer"></div>
  <script>
    // PLATEAU-Terrainで必要
    Cesium.Ion.defaultAccessToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlNjk0MTM4NC1lMWI0LTQxNTgtYjcxZS01ZWJhMGJlMTE1MWQiLCJpZCI6MTQ5ODk3LCJpYXQiOjE3MTUxNTEyODZ9.2aUmEQ2-fDsjf-XeC6-hZpwkgwLse3yXoXF4xTOvPAY";

    const viewer = new Cesium.Viewer("cesiumContainer", {});

    // PLATEAU-Terrain
    viewer.scene.setTerrain(
      new Cesium.Terrain(
        Cesium.CesiumTerrainProvider.fromIonAssetId(2488101),
      ),
    );

    // PLATEAU-Ortho
    const imageProvider = new Cesium.UrlTemplateImageryProvider({
      url: 'https://gic-plateau.s3.ap-northeast-1.amazonaws.com/2020/ortho/tiles/{z}/{x}/{y}.png',
      maximumLevel: 19
    });
    const currentImage = viewer.scene.imageryLayers.addImageryProvider(imageProvider);

    // 東京都千代田区の建築物モデル（3D Tiles）
    Cesium.Cesium3DTileset.fromUrl(
      'https://assets.cms.plateau.reearth.io/assets/0e/e5948a-e95c-4e31-be85-1f8c066ed996/13101_chiyoda-ku_pref_2023_citygml_1_op_bldg_3dtiles_13101_chiyoda-ku_lod1/tileset.json'
    ).then((tileset) => {
      viewer.scene.primitives.add(tileset);
    });

    // 東京都の土地利用モデル（MVT）
    const yourMvt = new CesiumMVTImageryProvider.CesiumMVTImageryProvider({
      urlTemplate: "https://assets.cms.plateau.reearth.io/assets/4c/efcbfe-f523-4a59-92f8-f6af80882333/13_tokyo_pref_2023_citygml_1_op_luse_mvt/{z}/{x}/{y}.mvt",
      layerName: "luse",
      style: feature => {
        return {
          fillStyle: "white" // 白で塗りつぶす
        };
      },
    });
    viewer.scene.imageryLayers.addImageryProvider(yourMvt);

    // カメラの初期位置の指定
    viewer.camera.setView({
      destination: Cesium.Cartesian3.fromDegrees(139.76, 35.68, 5000.0)
    });
  </script>
</body>
</html>
```

## 4. 配信データの取得方法

Project PLATEAUが[G空間情報センター](https://www.geospatial.jp/ckan/dataset/plateau)において公開している都市の3D TilesおよびMVTデータを利用可能です。

配信データを取得するためのAPIとして、以下の2種類の「データカタログAPI」を公開しています。

- 通常のWebAPI
- GraphQL API

### 通常のWebAPI

通常のWebAPIでは、3D Tiles/MVTのデータ一覧がJSONのレスポンスとしてサーバーから返却されます。データを利用する際には、レスポンス内に含まれる、`url`というフィールドに含まれるURLを利用してください。

https://api.plateauview.mlit.go.jp/datacatalog/plateau-datasets

> [!WARNING]
> APIのスキーマやレスポンスは予告なく変更されることがあります。また、提供期間やサービスレベルの保証もしておりません。ご了承の上ご利用ください。

> [!WARNING]
> このAPIはレスポンスのサイズが大きく、約2MB以上あります。APIはgzip圧縮に対応していますが、モバイル回線などで使用する際は十分ご注意ください。

**リクエスト例**

```shell
curl https://api.plateauview.mlit.go.jp/datacatalog/plateau-datasets
```

**レスポンス例**

```shell
{
  "datasets": [
    {
      "id": "01101_bldg_lod1",
      "name": "建築物モデル（中央区）",
      "pref": "北海道",
      "pref_code": "01",
      "city": "札幌市",
      "city_code": "01100",
      "ward": "中央区",
      "ward_code": "01101",
      "type": "建築物モデル",
      "type_en": "bldg",
      "url": "https://assets.cms.plateau.reearth.io/assets/b8/314602-4b39-4d5f-be2d-a0b17a3e3c21/01100_sapporo-shi_city_2020_citygml_6_op_bldg_3dtiles_01101_chuo-ku_lod1/tileset.json",
      "layers": null,
      "year": 2020,
      "registration_year": 2023,
      "spec": "3.3",
      "format": "3D Tiles",
      "lod": "1",
      "texture": true
    },
    {
      ...
    }
  ]
}
```

**APIスキーマ**

| フィールド | 名称 | 説明 |
|:-----------|:-----------|:-----------|
| name | 名称 | データの名称 |
| pref | 都道府県名 | 都道府県名 |
| pref_code | 都道府県コード | 2桁の都道府県コード |
| city | 市区町村名 | 市区町村名 |
| city_code | 市の市区町村コード | 5桁の市の市区町村コード |
| ward | 区名 | 5桁の区の市区町村コード。政令指定都市の場合は区ごとにデータが分割されていることがあり、その場合のみ区の名前が含まれます。 |
| ward_code | 区の市区町村コード | 5桁の区の市区町村コード。政令指定都市の場合は区ごとにデータが分割されていることがあり、その場合のみ区ごとの市区町村コードが含まれます。 |
| type | 種別 | 配信している3D都市モデルの地物型の種別。例えば `"建築物モデル"` など。地物型の種別の詳細は[「3D都市モデル標準製品仕様書」](https://www.mlit.go.jp/plateaudocument/)の「ファイル名称」を参照してください。 |
| type_en | 種別（英名） | 配信している3D都市モデルの地物型の種別の英語名。例えば、`"bldg"` は建築物モデルを意味します。地物型の種別の詳細は[「3D都市モデル標準製品仕様書」](https://www.mlit.go.jp/plateaudocument/)の「ファイル名称」を参照してください。 |
| url | URL | 3D Tiles の場合は `tileset.json` のURL、MVTの場合は `{z}/{x}/{y}.mvt` のようなタイルURL。 |
| layers | レイヤー名 | MVTのみ利用可能。MVTのレイヤー名。複数存在することもあります。 |
| year | 整備年度 | データが整備された年度。例えば、 `2023` の場合は2023年度に整備されたデータであることを意味します。 |
| registration_year | 登録年度 | データがPLATEAU VIEWに登録された年度。例えば、 `2023` の場合は2023年度に登録されたデータであることを意味します。 |
| spec | 仕様書バージョン | 元になったCityGMLデータが準拠するPLATEAU 3D都市モデル標準製品仕様書のバージョン（文字列） |
| format | データフォーマット | データ形式。 `"3D Tiles"` または `"MVT"` のいずれか。 |
| lod | LOD | データのLOD（文字列） |
| texture | テクスチャ | 3D Tilesで、テクスチャありのデータの場合 `true` |

### GraphQL API

[GraphQL](https://graphql.org/) APIでは、通常のWebAPIに比べ、より柔軟なクエリが可能です。

https://api.plateauview.mlit.go.jp/datacatalog/graphql

> [!WARNING]
> GraphQLのスキーマやレスポンスは予告なく変更されることがあります。また、提供期間やサービスレベルの保証もしておりません。ご了承の上ご利用ください。

> [!WARNING]
> このAPIはクエリによってはレスポンスのサイズが数MB以上と大きくなることがあります。APIはgzip圧縮に対応していますが、モバイル回線などで使用する際は十分ご注意ください。また、動作速度低下を防ぐため、一定以上の複雑なクエリは制限されます。

GraphQLをブラウザ上から簡単に試すことができる「GraphiQL」は、上記URLをWebブラウザで開くことで利用可能です。GraphQLの詳細なドキュメントやリクエストはGraphiQLからご利用ください。

以下は千代田区のデータ一覧を千代田区の行政コード（01100）をキーとして取得する例です。この他にも、都道府県一覧や都市一覧を取得するAPIなど、多彩なクエリが利用できます。

**リクエスト例**

```shell
curl -X POST -H 'Content-Type: application/json' -d '{"query":"..."}' https://api.plateauview.mlit.go.jp/datacatalog/graphql
```

```gql
query {
  area(code: "01100") {
    id
    type
    datasets {
      id
      name
      items {
        id
        name
        url
      }
    }
  }
}
```

**レスポンス例**

```json
{
  "data": {
    "area": {
      "id": "c_01100",
      "type": "CITY",
      "datasets": [
        {
          "id": "d_01101_bldg",
          "name": "建築物モデル（中央区）",
          "items": [
            {
              "id": "di_01101_bldg_lod1",
              "name": "LOD1",
              "url": "https://assets.cms.plateau.reearth.io/assets/b8/314602-4b39-4d5f-be2d-a0b17a3e3c21/01100_sapporo-shi_city_2020_citygml_6_op_bldg_3dtiles_01101_chuo-ku_lod1/tileset.json"
            },
            {
              ...
            }
          ]
        },
        {
          ...
        }
      ]
    }
  }
}
```

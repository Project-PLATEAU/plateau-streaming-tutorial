# plateau-3D Tiles-streaming
### PLATEAU-3DTiles/MVT配信チュートリアル
## 1. PLATEAU-3DTilesの概要

Project PLATEAUでは、CityGML形式で作成された3D都市モデルのデータを3D Tiles形式に変換し、PLATEAU-3DTilesとして配信を行っています。本チュートリアルでは、3D Tilesの利用方法について解説します。

## 2. CityGMLの3D Tilesへの変換

CityGMLを3D Tiles形式に変換するためには、以下の方法があります。詳しい情報は、それぞれのタイトルをクリックしてください。
- [FME](https://github.com/Project-PLATEAU/FMEscript-CityGML-to-3DTiles): Project PLATEAU で利用したCityGMLから3D TilesへのFME変換テンプレート
- [Cesium ion](https://cesium.com/learn/3d-tiling/ion-tile-3d-buildings/): オンラインサービスを利用したデータ変換とホスティング
- [Cesium ion オンプレミス](https://cesium.com/platform/on-premises-products/)：3D Tiles変換用の有償のコマンドラインツール
- [citygml-to-3dtiles](https://github.com/njam/citygml-to-3dtiles)：データ変換用オープンソースツール

## 3. PLATEAU-3DTilesの利用方法

PLATEAU-3DTilesの配信サービスを利用することで、独自に立ちあげたPLATEAU VIEW等の環境で3D都市モデルをすぐに利用することができます。

本ドキュメントの末尾にある2022年度時点で公開している3D都市モデルの配信URL一覧をご利用ください。

なお、本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことご了承ください。

## 3.1. CesiumJSでの利用方法

3D Tiles形式のデータは、PLATEAU VIEWのカタログに登録して表示させたり、CesiumJSを利用したコードを作成することで表示できます。

以下は、CesiumJSを利用して3D Tilesを表示させるためのサンプルコードです。この例では、東京都千代田区の建物データ（3D Tiles）を利用していますが、3D Tiles一覧にあるURLに置き換えることで、様々な3D Tilesデータを表示することができます。

```html
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>PLATEAU-3DTiles、PLATEAU-Ortho、PLATEAU-TerrainをCesiumで表示</title>
  <script src="https://cesium.com/downloads/cesiumjs/releases/1.104/Build/Cesium/Cesium.js"></script>
  <link href="https://cesium.com/downloads/cesiumjs/releases/1.104/Build/Cesium/Widgets/widgets.css" rel="stylesheet"></head>
</head>
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
    // Cesium ionの読み込み指定
    Cesium.Ion.defaultAccessToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI5N2UyMjcwOS00MDY1LTQxYjEtYjZjMy00YTU0ZTg5MmViYWQiLCJpZCI6ODAzMDYsImlhdCI6MTY0Mjc0ODI2MX0.dkwAL1CcljUV7NA7fDbhXXnmyZQU_c-G5zRx8PtEcxE";

    // Terrainの指定（EGM96、国土数値情報5m標高から生成した全国の地形モデル、5m標高データが無い場所は10m標高で補完している）
    var viewer = new Cesium.Viewer("cesiumContainer", {
      terrainProvider: new Cesium.CesiumTerrainProvider({
        url: Cesium.IonResource.fromAssetId(770371)
      })
    });

    // G空間情報センターに置かれている、Project PLATEAUで作成したPLATEAUオルソの参照
    var imageProvider = new Cesium.UrlTemplateImageryProvider({
      url: 'https://gic-plateau.s3.ap-northeast-1.amazonaws.com/2020/ortho/tiles/{z}/{x}/{y}.png',
      maximumLevel: 19
    });
    var currentImage = viewer.scene.imageryLayers.addImageryProvider(imageProvider);

   // 東京都千代田区の建物データ（3D Tiles）
    var your_3d_tiles = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({
      url: 'https://assets.cms.plateau.reearth.io/assets/11/6d05db-ed47-4f88-b565-9eb385b1ebb0/13100_tokyo23-ku_2022_3dtiles%20_1_1_op_bldg_13101_chiyoda-ku_lod1/tileset.json'
    }));

    // カメラの初期位置の指定
    viewer.camera.setView({
      destination: Cesium.Cartesian3.fromDegrees(139.76, 35.68, 5000.0)
    });
  </script>
</body>
</html>
```

## 4. 配信データ（3D Tiles）一覧

2023年度末の時点でProject PLATEAUが[G空間情報センター](https://www.geospatial.jp/ckan/dataset/plateau)において公開している都市の3DTilesデータを利用可能です。
配信している3D都市モデルの3D TilesデータのURL一覧は、[3dtiles_url.json](../3dtiles_url.json)から取得することができます。このファイルの仕様は次の通りです。
JSON形式のレスポンスのフィールドについては[こちら](../README.md)を参照してください。
配信URLの取得方法には、以下の2種類あります。
* 通常のWebAPI
* GraphQL形式のAPI
### 通常のWebAPI
通常のWebAPIでは、都市のデータ一覧がJSONのレスポンスとしてサーバーから返却されます。3D Tiles形式等のデータを利用する際には、レスポンス内に含まれる、`url`というフィールドに含まれるURLを利用してください。

**リクエスト例**
```shell
curl https://api.plateau.reearth.io/datacatalog/plateau-datasets
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

### GraphQL形式のAPI
GraphQL形式のAPIでは、より柔軟なクエリが可能です。GraphQLをブラウザ上から簡単に試すことができるGraphiQLは、[https://api.plateau.reearth.io/datacatalog/graphql](https://api.plateau.reearth.io/datacatalog/graphql)からアクセスが可能です。GraphQLのドキュメントやリクエストはこちらのGraphiQLからご利用ください。以下は千代田区のデータ一覧を千代田区の行政コード（01100）をキーとして取得する例です。この他にも、都道府県一覧や都市一覧を取得するAPIも利用できます。
**リクエスト例**
```gql
query FetchData {
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
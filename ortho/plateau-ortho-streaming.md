# PLATEAU-Ortho 配信チュートリアル

## 1. PLATEAU-Orthoの概要

Project PLATEAUでは、航空写真測量によって作成したオルソ画像をタイル化し、PLATEAU-Orthoとして配信を行っています。本チュートリアルでは、タイル化技術及びPLATEAU-Orthoの利用方法について解説します。

### 1.1. オルソ画像タイルについて

オルソ画像タイルとは、大きなオルソ画像をタイル状に分割したものです。

オルソ画像をタイル化する理由は、現状のインターネットやパソコン、スマホの能力では、大きな画像が重たすぎて、スムーズな配信・表示ができないからです。そのほかにも、部分的な差し替えが簡単になるなどの副次的なメリットもありますが、何よりも、表示が軽くなるというのが最大の理由です。

PLATEAU VIEWではオルソ航空写真をあらかじめオルソ画像タイルにしてあるので、どこまでもスクロールしながら綺麗なオルソ画像を楽しむことができるようになっています。この時、PLATEAU VIEWはサーバーから日本全部の画像をダウンロードしているのではなく、そのときの表示範囲に必要な画像タイルを、ほんの何十枚かだけ取ってきています。地図をスクロールしたときは、新しい表示に必要な画像タイルを追加でダウンロードします。

画像タイル1枚のサイズは256x256ピクセルの正方形で、非常に小さく、軽く作られているので、新しいタイルをどんどんダウンロードしても、表示が重たくなることはありません。

### 1.2. PLATEAU-Orthoの構成

PLATEAU VIEWで見ることのできるオルソ画像タイルは2種類あります。ひとつはProject PLATEAUで新たに整備したPLATEAU-Ortho、もうひとつは国土地理院が整備した日本全国をカバーする地理院タイル全国最新写真（シームレス）です。そのほかに、Bingの空中写真も背景図として選択できますが、写真はオルソ化されていません。

PLATEAU-Orthoの特徴は、国土地理院が整備している[地理院タイル](https://maps.gsi.go.jp/development/siyou.html)をベースにしつつ、新たに取得したオルソ航空写真を組み合わせ、精度を高めているところです。これにより、全体のカバー率を維持しつつ、ユースケース等に必要な範囲で高精度の地形テクスチャを提供することが可能になっています。また、地理院タイルと比べて新しいデータを利用可能です。

Project PLATEAUが新たに取得したオルソ航空写真は[G空間情報センター](https://www.geospatial.jp/ckan/dataset/plateau)からGeoTIFF形式で入手可能です。

Project PLATEAUで配布しているデータの利用許諾については、クリエイティブ・コモンズ・ライセンスの表示4.0国際等に準拠していますので、無償かつ商用も含めた利用が可能です。詳しくはProject PLATEAU [サイトポリシー](https://www.mlit.go.jp/plateau/site-policy/)をご確認ください。

## 2. 配信URL

> [!WARNING]
> 本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことをご了承ください。

### 2.1. PLATEAU-Ortho（xyzタイル）

2023年度に作成されたデータが利用可能です。

```
https://api.plateauview.mlit.go.jp/tiles/plateau-ortho-2023/{z}/{x}/{y}.png
```

ズームレベルは10〜19に対応しています。

**整備地域**

| 地域コード | 都道府県 | 市町村 | 撮影年次 |
| --- | --- | --- | --- |
| 01100 | 北海道 | 札幌市 | 2020 |
| 04100 | 宮城県 | 仙台市 | 2022 |
| 07203 | 福島県 | 郡山市 | 2020 |
| 07204 | 福島県 | いわき市 | 2020 |
| 07205 | 福島県 | 白河市 | 2020 |
| 07209 | 福島県 | 相馬市 | 2023 |
| 09201 | 栃木県 | 宇都宮市 | 2020 |
| 10201 | 群馬県 | 前橋市 | 2023 |
| 10207 | 群馬県 | 館林市 | 2020 |
| 11202 | 埼玉県 | 熊谷市 | 2020 |
| 11210 | 埼玉県 | 加須市 | 2023 |
| 11214 | 埼玉県 | 春日部市 | 2023 |
| 11222 | 埼玉県 | 越谷市 | 2023 |
| 11232 | 埼玉県 | 久喜市 | 2023 |
| 11234 | 埼玉県 | 八潮市 | 2023 |
| 11238 | 埼玉県 | 蓮田市 | 2022 |
| 11243 | 埼玉県 | 吉川市 | 2023 |
| 11246 | 埼玉県 | 白岡市 | 2023 |
| 11442 | 埼玉県 | 宮代市 | 2023 |
| 11464 | 埼玉県 | 杉戸市 | 2023 |
| 11465 | 埼玉県 | 松伏町 | 2023 |
| 12210 | 千葉県 | 茂原市 | 2022 |
| 12217 | 千葉県 | 柏市 | 2020 |
| 13100 | 東京都 | 23区 | 2023 |
| 13213 | 東京都 | 東村山市 | 2020 |
| 13219 | 東京都 | 狛江市 | 2023 |
| 13229 | 東京都 | 西東京市 | 2022 |
| 14100 | 神奈川県 | 横浜市 | 2023 |
| 14130 | 神奈川県 | 川崎市 | 2022 |
| 15202 | 新潟県 | 長岡市 | 2023 |
| 15222 | 新潟県 | 上越市 | 2023 |
| 17206 | 石川県 | 加賀市 | 2022 |
| 19201 | 山梨県 | 甲府市 | 2022 |
| 20202 | 長野県 | 松本市 | 2020 |
| 20204 | 長野県 | 岡谷市 | 2020 |
| 20206 | 長野県 | 諏訪市 | 2023 |
| 20209 | 長野県 | 伊那市 | 2020 |
| 20214 | 長野県 | 茅野市 | 2022 |
| 22203 | 静岡県 | 沼津市 | 2020 |
| 22213 | 静岡県 | 掛川市 | 2020 |
| 23100 | 愛知園 | 名古屋市 | 2020 |
| 23201 | 愛知県 | 豊橋市 | 2023 |
| 23206 | 愛知県 | 春日井市 | 2023 |
| 23207 | 愛知園 | 豊川市 | 2022 |
| 23208 | 愛知園 | 津島市 | 2020 |
| 24202 | 三重県 | 四日市市 | 2023 |
| 26100 | 京都府 | 京都市 | 2022 |
| 27100 | 大阪府 | 大阪市 | 2022 |
| 27203 | 大阪府 | 豊中市 | 2020 |
| 27204 | 大阪府 | 池田市 | 2020 |
| 28201 | 兵庫県 | 姫路市 | 2023 |
| 28210 | 兵庫県 | 加古川市 | 2020 |
| 28225 | 兵庫県 | 朝来市 | 2022 |
| 31202 | 鳥取県 | 米子市 | 2023 |
| 31384 | 鳥取県 | 日吉津村 | 2023 |
| 33211 | 岡山県 | 備前市 | 2023 |
| 34100 | 広島県 | 広島市 | 2022 |
| 36201 | 徳島家 | 徳島市 | 2023 |
| 37201 | 香川県 | 高松市 | 2022 |
| 38201 | 愛媛県 | 松山市 | 2020 |
| 40202 | 福岡県 | 大牟田市 | 2023 |
| 40203 | 福岡県 | 久留米市 | 2020 |
| 40225 | 福岡県 | うきは市 | 2023 |
| 40447 | 福岡県 | 筑前町 | 2023 |
| 43100 | 熊本県 | 熊本市 | 2020 |
| 43206 | 熊本県 | 玉名市 | 2020 |
| 43443 | 熊本県 | 益城町 | 2023 |
| 47201 | 沖縄県 | 那覇市 | 2020 |



### 2.2. PLATEAU-Ortho （OGC WMS)

2020年度に作成されたデータがWMSで利用可能です。

**サービスアドレス**

```
https://plateauortho.geospatial.jp/mapproxy/service
```

**WMSバージョン**

WMSバージョン1.1.1または1.3.0に準拠しています。

**WMSリクエスト**

以下のWMSリクエストをサポートしています。

- GetCapabilities
- GetMap

**座標系**

EPSG 4326、EPSG 3857、EPSG 900913、CRS 84をサポートしています。

**レイヤー**

| レイヤー名 | 内容 |
| :---: | :---: |
| plateau | PLATEAU-Ortho |
| photo | 国土地理院の写真（ベースマップ） |
| plateau_photo | plateauとphotoの重ね合わせ |

**リクエスト例**

- GetCapabilities

```
https://plateauortho.geospatial.jp/mapproxy/service?REQUEST=GetCapabilities
```

- GetMap

EPSG:3857でのリクエスト例

```
https://plateauortho.geospatial.jp/mapproxy/service?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&BBOX=15528612.30738627352,4234060.365724110045,15581327.2790426109,4277115.012342031114&CRS=EPSG:3857&WIDTH=1075&HEIGHT=878&LAYERS=plateau_photo&STYLES=&FORMAT=image/png&DPI=96&MAP_RESOLUTION=96&FORMAT_OPTIONS=dpi:96&TRANSPARENT=TRUE
```

EPSG:4326でのリクエスト例

```
https://plateauortho.geospatial.jp/mapproxy/service?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&BBOX=35.62364808927861759,139.853018468884585,35.67783063591231496,139.9193581472800645&CRS=EPSG:4326&WIDTH=1075&HEIGHT=878&LAYERS=plateau_photo&STYLES=&FORMAT=image/png&DPI=96&MAP_RESOLUTION=96&FORMAT_OPTIONS=dpi:96&TRANSPARENT=TRUE
```

**整備地域**

| 地域コード | 都道府県 | 地域名 | 撮影年次 |
| :---: | :---: | :---: | :---: |
| 01100 | 北海道 | 札幌市 | 2019 |
| 07203 | 福島県 | 郡山市 | 2016 |
| 07204 | 福島県 | いわき市 | 2019 |
| 07205 | 福島県 | 白河市 | 2019 |
| 09201 | 栃木県 | 宇都宮市 | 2019 |
| 10207 | 群馬県 | 館林市 | 2019 |
| 11202 | 埼玉県 | 熊谷市 | 2017 |
| 12217 | 千葉県 | 柏市 | 2019 |
| 13100 | 東京都 | 東京23区 | 2018 |
| 13213 | 東京都 | 東村山市 | 2020 |
| 14100 | 神奈川県 | 横浜市 | 2019 |
| 20202 | 長野県 | 松本市 | 2019 |
| 20204 | 長野県 | 岡谷市 | 2019 |
| 20209 | 長野県 | 伊那市 | 2019 |
| 22203 | 静岡県 | 沼津市 | 2017 |
| 22213 | 静岡県 | 掛川市 | 2019 |
| 23100 | 愛知県 | 名古屋市 | 2020 |
| 23208 | 愛知県 | 津島市 | 2019 |
| 27100 | 大阪府 | 大阪市 | 2020 |
| 27203 | 大阪府 | 豊中市 | 2019 |
| 27204 | 大阪府 | 池田市 | 2019 |
| 28210 | 兵庫県 | 加古川市 | 2019 |
| 38201 | 愛媛県 | 松山市 | 2016 |
| 40203 | 福岡県 | 久留米市 | 2017 |
| 43100 | 熊本県 | 熊本市 | 2017 |
| 43206 | 熊本県 | 玉名市 | 2014 |
| 43443 | 熊本県 | 益城町 | 2020 |
| 47201 | 沖縄県 | 那覇市 | 2018 |

**ズームレベル**

ズームレベル19までの拡大が可能です。これは、約30cmの解像度に相当します。インターネット地図の中では比較的高精度な画像です。
なお、ズームレベルと画像の解像度との関係は以下のようになっています。

|ズームレベル |画像解像度 |
| :----: | :----: |
| 10 | 153m |
| 11 | 76m |
| 12 | 38m |
| 13 | 19m |
| 14 | 10m |
| 15 | 5m |
| 16 | 2m |
| 17 | 1.2m |
| 18 | 0.6m |
| 19 | 0.3m |

## 3. PLATEAU-Orthoの利用方法

本節では、PLATEAU-Ortho(xyzタイル）をCesiumJS、TerriaJS、Leaflet、QGISで利用する方法について解説します。

### 3.1. CesiumJSで利用する方法

CesiumJSは一般的に使われている3次元地図を表示するためのJavaScriptライブラリです。

CesiumJSでオルソ画像タイルを利用するには、地形モデルもあわせて使用することをおすすめします。ここでは、地形データとしてPLATEAU-Terrainを利用し、PLATEAU-Orthoを表示するためのサンプルコードを示します。

Cesiumの構築方法についてはCesium GS社の[チュートリアル](https://cesium.com/learn/cesiumjs-learn/)や[ドキュメント](https://cesium.com/learn/cesiumjs/ref-doc/Viewer.html)を参照してください。

PLATEAU-Terrainについては[PLATEAU-Terrain](/terrain/plateau-terrain-streaming.md)を参照してください。

なお、建物モデルは[PLATEAU-3DTiles/MVT](../3d-tiles/plateau-3dtiles-streaming.md)から配信されている千代田区の建物モデルデータを利用しています。

```html
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>PLATEAU-3DTiles/MVT、PLATEAU-Ortho、PLATEAU-TerrainをCesiumで表示</title>
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
      url: 'https://api.plateauview.mlit.go.jp/tiles/plateau-ortho-2023/{z}/{x}/{y}.png',
      maximumLevel: 19
    });
    const currentImage = viewer.scene.imageryLayers.addImageryProvider(imageProvider);

    // 東京都千代田区の建築物モデル（3D Tiles）
    Cesium.Cesium3DTileset.fromUrl(
      'https://assets.cms.plateau.reearth.io/assets/0e/e5948a-e95c-4e31-be85-1f8c066ed996/13101_chiyoda-ku_pref_2023_citygml_1_op_bldg_3dtiles_13101_chiyoda-ku_lod1/tileset.json'
    ).then((tileset) => {
      viewer.scene.primitives.add(tileset);
    });

    // カメラの初期位置の指定
    viewer.camera.setView({
      destination: Cesium.Cartesian3.fromDegrees(139.76, 35.68, 5000.0)
    });
  </script>
</body>
</html>
```

### 3.2. TerriaJSで利用する方法

PLATEAU VIEW 1.1では、TerriaJSというJavaScriptライブラリを使用しています。ここでは、TerriaJSにオルソ画像タイルを組み込む方法をご紹介します。

TerriaJSのカタログファイルに以下のように記述することで、PLATEAU-OrthoをTerriaJSのレイヤとして組み込むことができます。

```json
{
  "type": "open-street-map",
  "id": "/basemap//オルソ画像タイル/imagery",
  "name": "航空写真",
  "url": "https://api.plateauview.mlit.go.jp/tiles/plateau-ortho-2023/",
  "fileExtension": "png",
  "attribution": ""
}
```
Project PLATEAUでは[PLATEAU VIEW 1.1](https://github.com/Project-PLATEAU/PLATEAU-VIEW-1.1)及びそのための[カタログファイルジェネレータ](https://github.com/Project-PLATEAU/plateau-catalog-generator)のソーススクリプトを公開していますので、そちらも参照してください。

### 3.3. Leafletで利用する方法

web地図を作成する際によく利用されるLeafletでの利用方法をサンプルHTMLで紹介します。

LeafletはJavaScriptの地図ライブラリです。無償で利用できることと、Leafletサイトのサンプルコードのコピー＆ペーストで、簡単に地図アプリをホームページに組み込むことができるので、人気の高いソフトウェアです。

以下のHTMLはLeafletでオルソ画像タイルを表示するサンプルです。このHTMLをortho_test.htmlなどの名前でPCに保存して、ブラウザで開いてみてください。東京都心部のオルソ画像タイルがLeafletで表示されます。

```html
<html lang=ja>
<head>
  <title>LeafletでPLATEAU-Orthoを表示する</title>
  <meta charset="utf-8" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
  <script>
    document.addEventListener("DOMContentLoaded", () => {
      const map = L.map('map').setView([35.68, 139.76], 14);
      map.options.minZoom = 10;
      map.options.maxZoom = 18;
      L.tileLayer('https://api.plateauview.mlit.go.jp/tiles/plateau-ortho-2023/{z}/{x}/{y}.png', {
        attribution: '<a href="https://www.mlit.go.jp/plateau/">国土交通省Project PLATEAU</a>'
      }).addTo(map);
    });
  </script>
</head>
<body>
  <div id="map" style="width: 80%; height: 600px; border: solid 1px"></div>
</body>
</html>
```

このサンプル以外にも、背景画像や重ね合わせ画像として、画像タイルを読み込むためのサンプルがインターネット上に多数公開されています。それらを参考にして、Leafletの地図サイトを作成してみてください。

### 3.4. QGISで利用する方法

[QGIS](https://qgis.org/ja/site/)はWindowsやMacで使える、オープンソースのGISソフトです。QGISではGISデータを表示するときの背景図として、PLATEAU-Orthoを使うことができます。

ここでは、QGISでの使い方の一例をご紹介します。画面はバージョン3.22.1のものですが、最近のバージョンであれば同じように使えます。

① QGISのブラウザタブにある「XYZ
Tiles」を右クリックして、「新規接続」を選びます。

<img src="images/ortho_qgis_xyz.png" alt="QGIS drag and drop" style="width:200px;">

② 下図のウインドウが出てきますので、名前、URL、最大ズームレベルを記入してください。

- 名前は自由につけられます。
- URL欄は次の文字を丸ごとコピー＆ペーストしてください。

  ```
  https://api.plateauview.mlit.go.jp/tiles/plateau-ortho-2023/{z}/{x}/{y}.png
  ```

- 最大ズームレベルは、19 にしてください。
- 最後に「OK」を押します。

<img src="images/ortho_qgis_xyz_dialog.png" alt="QGIS xyz dialog" style="width:400px;">

③ ブラウザウインドウに「PLATEAU-Ortho画像タイル」が追加されます。この名前は、ひとつ前の②で、XYZ接続の名前として指定したものです。

④地図にオルソ画像タイルを組み込んで表示

最初に、マウスで「OpenStreetMap」をつかんで、レイヤタブに入れます。

PLATEAUのオルソ画像タイルは全国で存在するわけではないので、背景図として先にOpenStreetMapを入れておきます。

次に、「PLATEAU-Ortho画像タイル」をつかんで、レイヤタブに入れます。

<img src="images/ortho_qgis_drag.png" alt="QGIS drag and drop" style="width:200px;">

上記地域を拡大してみてください。ある程度拡大すると、PLATEAU-Orthoの画像タイルが表示されます。

<img src="images/orth_gqis_map.png" alt="QGIS map" style="width:500px;">

QGISでPLATEAU-Ortho画像タイルを表示（背景はオープンストリートマップ）

## 4. オルソ画像タイルを作成する方法

オルソ画像タイルを作るには専用のソフトウェアを利用します。

Project PLATEAUでは、大量のオルソ画像のタイル化を効率的に行うために、FMEという市販のソフトウェアを利用していますが、オープンソースやフリーソフトでも、オルソ画像タイルを作成することができます。
例えば、以下のウェブサイトではQGISというオープンソースのGISソフトを利用したタイル生成の方法が紹介されています。

- [「GIS実習オープン教材」タイル地図入門（ラスタ）](https://gis-oer.github.io/gitbook/book/materials/web_gis/rastertile/rastertile.html)

オルソ画像タイルを作成する手順は以下の2ステップです。

- オルソ画像の準備

  - オルソ画像の材料は航空写真や衛星画像です。普通は航空測量会社やリモートセンシング技術センターなどから購入することになりますが、小さな範囲であれば、ドローンを使って自前のオルソ航空写真を作成することができます。ここではドローン撮影画像の処理については解説しませんが、オルソ画像を作成するフリーソフトやネット上のサービスなども利用することができます。

  - なおドローン撮影には一般的に関係部署の許可が必要です。撮影に際しては、関係法令を遵守するように充分注意してください。

- オルソ画像のタイル化

  - オルソ画像を専用ソフトに読み込ませて、オルソ画像タイルを保存します。上記のQGISは無償で利用可能です。

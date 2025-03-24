# PLATEAU-Terrain 配信チュートリアル

## 1. PLATEAU-Terrainの概要

Project PLATEAUでは、Cesiumをベースに作られたPLATEAU VIEWで利用する日本全国の地形データ「PLATEAU-Terrain」を作成し、配信を行っています。
またPLATEAUが提供するCityGML形式の地形データを、Mapbox Terrain-RGBに変換する「PLATEAU Mapbox Terrain Converter」を開発し、それを用いて作成した日本全国の地形データの配信も行ってます。

本チュートリアルでは、地形データ作成技術及び利用方法について解説します。


### 1.1. 地形データについて

地形データとは、地形を3次元でモデル化したデータです。通常、地形データはほかの3次元データを重畳するための基盤データとして利用され、地形データ自体を視覚的に表現することはありません。

PLATEAU VIEWでは、地形データの上に、地理院タイルやオルソ写真、そのほか2次元のタイルデータをドレープ（覆いかぶせる処理）して地表面を表示しています。地形データとは、3次元データを表示する際の骨格のようなデータということができます。

<img src="image/../images/terrain_image.png" alt="terrain image" style="width:600px;">

図1　地形データのイメージ（引用：https://cesium.com/blog/2015/12/18/terrain-quantization/ ）

地形データの種類やデータフォーマットには様々なものがあります。これらのうち、PLATEAU
VIEWを構成するCeisumでは、[quantized-mesh形式](https://github.com/CesiumGS/quantized-mesh)のデータを利用しています。

Quantized-mesh形式は、3Dのメッシュデータを量子化（quantization）して圧縮する方式で、通常のメッシュデータよりもデータサイズと使用メモリ量を大幅に縮小することができます。

Quantize-mesh形式に興味のある方は、[このサイト](https://cesium.com/blog/2015/12/18/terrain-quantization/)をご覧ください。

地形データには、quantized-mesh形式以外にも様々な形式が存在します。それらの地形データの詳細については、以下にリストとしてURLをまとめましたので、それぞれのサイトを参照してください。

### 地形データの種類

- 標高タイル
  - <https://maps.gsi.go.jp/development/demtile.html>
- Mapbox Terrain-RGB v1
  - <https://docs.mapbox.com/data/tilesets/reference/mapbox-terrain-dem-v1/>
- Mapbox Terrain-DEM v1
  - <https://docs.mapbox.com/data/tilesets/reference/mapbox-terrain-dem-v1/>
- Quadtree (四分木)
  - <https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.69.7733&rep=rep1&type=pdf>
- QuadTin
  - <https://www.ifi.uzh.ch/dam/jcr:ffffffff-d894-8e94-0000-0000604fd2b6/QuadTIN.pdf>
- Quantized-mesh
  - <https://github.com/CesiumGS/quantized-mesh>
  - https://readthedocs.org/projects/quantized-mesh-tile/downloads/pdf/latest/
  - https://cesium.com/blog/2015/12/18/terrain-quantization/
- TIN (triangulated irregular network)
  - <https://ja.wikipedia.org/wiki/TIN>

### 1.2. PLATEAU-Terrainの概要

PLATEAU VIEWでは、quantized-mesh形式のデータを.terraindb形式のファイルとして作成したPLATEAU-Terrainを利用しています。

PLATEAU-Terrainは、国土地理院が整備した基盤地図情報数値標高モデル5mメッシュを基本とし、5mメッシュが存在しない場所は基盤地図情報数値標高モデル10mメッシュを利用して作成されています。また、ジオイドモデルには「日本のジオイド2011 (Ver.2.2)」を使用しています。

詳細なデータ作成方法については後述「3. 地形データの作成」を参照してください。

## 2. PLATEAU-Terrainの利用方法

### 2.1. アクセストークン及びアセットID

PLATEAU-TerrainをCesiumで利用する際は以下のトークンとアセットIDを利用してください。

> [!WARNING]
> 本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことをご了承ください。

**トークン**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlNjk0MTM4NC1lMWI0LTQxNTgtYjcxZS01ZWJhMGJlMTE1MWQiLCJpZCI6MTQ5ODk3LCJpYXQiOjE3MTUxNTEyODZ9.2aUmEQ2-fDsjf-XeC6-hZpwkgwLse3yXoXF4xTOvPAY
```

**アセットID**

```
2488101
```

**使用例**

```js
Cesium.Ion.defaultAccessToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlNjk0MTM4NC1lMWI0LTQxNTgtYjcxZS01ZWJhMGJlMTE1MWQiLCJpZCI6MTQ5ODk3LCJpYXQiOjE3MTUxNTEyODZ9.2aUmEQ2-fDsjf-XeC6-hZpwkgwLse3yXoXF4xTOvPAY";

// ...

viewer.scene.setTerrain(
  new Cesium.Terrain(
    Cesium.CesiumTerrainProvider.fromIonAssetId(2488101),
  ),
);
```

### 2.2. CeisumJSアプリケーションの作成

CesiumJS上でPLATEAU-Terrainを利用するためのサンプルコードを示します。　
地形データの配信についてご質問がある方は、PacificSpatialSolutions株式会社（info@pacificspatial.com）までご連絡ください。

配信された地形データを利用する場合は、「地形データは、測量法に基づく国土地理院長承認（使用）R3JHs 778を得て使用」とデータの帰属に記載してください。

なお、建物モデルは[PLATEAU-3DTiles](../3d-tiles/plateau-3dtiles-streaming.md)から配信されている千代田区の建物モデルデータを、ドレープするオルソ写真は[PLATEAU-Ortho](/ortho/plateau-ortho-streaming.md)から配信されている東京都23区の航空写真を参照しています。

Cesiumの構築方法についてはCesium GS社の[チュートリアル](https://cesium.com/learn/cesiumjs-learn/)や[ドキュメント](https://cesium.com/learn/cesiumjs/ref-doc/Viewer.html)を参照してください。

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
        Cesium.CesiumTerrainProvider.fromIonAssetId(2487029),
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

> [!NOTE]
> CesiumJSでは、PLATEAU-Terrain以外に、地形データとしてCesiumがデフォルトで配信しているCesium World Terrainを利用することもできます。日本地域における地形データ詳細度は、Cesium World Terrainの方がPLATEAU-Terrainより劣りますが、世界各地において地形表現を有効にすることができます。Cesium World Terrainを利用する場合は、上記コード内の`terrainProvider`を以下のように設定してください。

```js
var viewer = new Cesium.Viewer('cesiumContainer', {
  terrainProvider: Cesium.createWorldTerrain()
});
```

## 3. 地形データの作成

PLATEAU-Terrainなどのオープンデータとして利用できる地形データを利用することに加え、独自に地形データを作成することも可能です。
本節では、独自に地形データを整備するために必要なデジタル標高モデル（DEM）データの作成方法について説明します。

### 3.1. DEMデータの作成

地形データを作成する場合は、地形データのもととなるラスター形式のデジタル標高モデル（DEM）を準備する必要があります。

ここでは、国土地理院が整備する基盤地図情報数値標高モデルを利用し、FMEを用いてDEMデータを作成する方法について説明します。

なお、DEMデータは様々な方法で作成可能であり、必ずしもFMEを利用する必要はありません。例えば、GISソフトウェアを利用して基盤地図情報数値標高モデル(10mメッシュ）を一つの大きなメッシュのDEMデータとして統合することで地形データを作成するためのデータソースとして利用できます。

### 3.1.1. 作業フォルダとFMEワークスペースの格納

基盤地図情報数値標高モデルから地形モデル用GeoTIFF形式DEMラスターへの変換を行うため、以下のワークスペースを[plateau-terrain-fme](./fme/)から入手し、作業フォルダ内に格納してください。作業フォルダはユーザーが任意に決めてください。

- s1_基盤地図情報DEMのGeoTIFF変換.fmw
- s1_基盤地図情報DEMのGeoTIFF変換_runner.fmw
- s2_海面つきDEM10B_GeoTIFF作成.fmw

これらのFMEワークスペースは、以下の条件で稼働することを確認しています。

- FME 2021.2.2またはそれ以降のバージョンのFME Desktop
- FME Hubで公開されているカスタムフォーマット [**Japanese Fundamental
    Geospatial Data (FGD) DEM V2** のダウンロード](https://hub.safe.com/publishers/pacific-spatial-solutions/formats/japanese-fundamental-geospatial-data-fgd-dem-v2)とインストール
- ダウンロードした **JP_FGD_DEM2.fds** をエクスプローラー上で右クリックしてインストール

### 3.1.2 DEMデータのダウンロード

次に、基盤地図情報数値標高モデルをダウンロードします。基盤地図情報ダウンロードサイトから対象とする地域の数値標高モデル（DEM）データ（5A, 5B, 5C, 10A, 10B）をすべてダウンロードし、作業フォルダーに保存してください。

基盤地図情報DEMの各データセット（2次メッシュ区画単位）のzipファイルは解凍しないでください。基盤地図情報ダウンロードサイトの「まとめてダウンロード」機能を使って複数のデータセット（zip）をさらにまとめたzipファイルでダウンロードした場合には、最上位のzipのみ解凍して2次メッシュ区画単位のデータセットに展開しておいてください。

DEMの種類（5A, 5B, 5C, 10A, 10B）ごとにサブフォルダーに分ける必要はありません（分けておいても差し支えありません）。

### 3.1.3. FMEワークスペースの実行

次に順番にFME WorkbenchまたはFME Quick
Translatorによってワークスペースを実行することにより、GeoTIFF形式DEMラスターデータセットに変換します。

(1) 基盤地図情報DEMデータからGeoTIFF形式DEMラスターへの変換

- ワークスペース名：s1_基盤地図情報DEMのGeoTIFF変換_runner.fmw

  - このワークスペースを実行することにより、作業フォルダー以下（サブフォルダーも含む）に保存したすべての基盤地図情報DEMデータをDEMラスターに変換し、出力先として指定したフォルダー内のDEMタイプ別サブフォルダー（dem5a, dem5b, dem5c, dem10a, dem10b）にGeoTIFF形式で保存します。

<img src="images/terrain_fme_runner_dialog.png" alt="fme runner dialog" style="width:500px;" >

図１　s1_基盤地図情報DEMのGeoTIFF変換_runner.fmw実行時のダイアログ

表１　s1_基盤地図情報DEMのGeoTIFF変換_runner.fmwで指定するパラメタの説明
|パラメーター名 | 内容 |
| --- | --- |
|FGD DEM Download Folder | 基盤地図情報DEMデータをダウンロードしたルートフォルダー（downloadsフォルダー）|
|Destination GeoTIFF Root Folder | 変換結果GeoTIFFファイルの出力先ルートフォルダー |
| Log Folder | ログファイル出力先フォルダー |
| Maximum Concurrent FME Processes | 同時に実行するFMEプロセス数（M, 最大7）|
| Workspace Runs per FME Process| ひとつのFMEプロセスによるワークスペースの実行回数（N）|

  - このワークスペースは、基盤地図情報DEMデータ（\*.zip）を1ファイルずつGeoTIFF形式に変換するための子ワークスペース「s1_基盤地図情報DEMのGeoTIFF変換.fmw」(同一フォルダー内に保存されていることを前提とします)をファイル数分、繰り返し実行します。

  - このワークスペースによる1ファイルあたりの変換時間は数秒ですが、多数のファイルを処理する場合にファイルごとに毎回FMEプロセスを起動するのでは、そのオーバーヘッドも無視できないほど大きくなります。そのため、このワークスペースでは、Workspace Runs per FME Process パラメーターでひとつのFMEプロセス（1回の起動）による子ワークスペースの実行回数（N）、Maximum　Concurrent FME Processes によって同時に実行できるFMEプロセス数（M, 最大7）を設定することにより、全体の効率化を図っています。

  - FME Desktopによって同一マシンで同時に実行できるFMEプロセス（データ変換処理）数の上限は8ですが、このワークスペースを実行することで1プロセスを使うため、同時に実行できる子ワークスペース用のプロセス数の上限は7です。

  - マシンのスペックやFMEによって同時に行う他のデータ変換プロセス数も勘案してまずMを決め、Nには、処理の対象とする全ファイル数÷Mをある整数で割った値に近い数を設定するのが適当です。


(2) 海域に標高値を与えたDEMラスター（10B）の作成

- ワークスペース: s2_海面つきDEM10B_GeoTIFF作成.fmw

- s1_基盤地図情報DEMのGeoTIFF変換_runner.fmwによって基盤地図情報DEMからGeoTIFF形式DEMラスターが作成できますが、全国をカバーしている10Bでも海域はNodata（値がない）であるため、このままCesiumの地形モデルに変換すると陸域と海域の境界付近で高さが不連続になることがあり、好ましくありません。このワークスペースを実行することにより、Nodataのセルに標高値0を与えるとともに、若干沖合に範囲を拡大した10BのDEMラスターデータセットを作成できます。

<img src="images/terrain_fme_dem10b.png" alt="fme 10B" style="width:500px;">

図２　ワークスペース: s2_海面つきDEM10B_GeoTIFF作成.fmw実行時のダイアログ

表２　ワークスペース:
s2_海面つきDEM10B_GeoTIFF作成.fmwで使用するパラメタの説明
| パラメーター名 | 内容 |
| --- | --- |
|Source GeoTIFF Folder (dem10b) | 変換済みのGeoTIFF形式10B DEMラスター保存先フォルダー |
| Destination GeoTIFF Root Folder | 変換結果GeoTIFFファイルの出力先フォルダー |

- 基盤地図情報DEMは、10B以外は全国の陸域をカバーしているわけではありません。また、標高値の精度は次の順で高くなります。

  - 低: 10B \< 10A \< 5C \< 5B \< 5A: 高

- そのため、Cesium ionの地形モデル（terrain）を作成する場合は、次の順に一つ前に作成した地形モデルに次の地形モデルを上乗せしていくことにより、地点ごとに存在するDEMタイプうち最も精度が高いものの標高値を使用した地形モデルとなります。

  - 海域に標高値を与えた10B 10A 5C 5B 5A

### 3.2. 地形データへの変換

ここでは、Cesium ionを利用したDEMデータの地形データへの変換方法を説明します。

Cesium ionのサービスを利用することで、データの変換後、地形データの配信も行えるようになります。なお、Cesium ionは有償サービスですが、一定の範囲内であれば無償で利用可能です。

Cesium ionでは、ユーザーがアップロードしたDEMファイルをterraindb形式に変換し、配信に利用可能です。terraindb形式への変換は、Cesium ionのクラウドサービスに加え、Ceisumのオンプレミス（有償）で提供される変換プログラムでも行えます。日本全国の詳細な地形データを作成する場合は、Cesiumのオンプレミスの利用をお勧めします。

なお、その他の地形データの利用方法としては、Pythonのライブラリである、quantized-mesh-encorder (<https://pypi.org/project/quantized-mesh-encoder/>) を利用してQuantized-mesh形式の地形データを準備し、Tile Map Service（TMS：<https://wiki.osgeo.org/wiki/Tile_Map_Service_Specification>）を利用して配信する方法などがあります。

### 3.2.1. Cesium ionアカウント開設

まず、Ceisum ion（https://cesium.com/）のアカウントを用意します。Ceisum ionは、商用の3Dデータ配信サービスです。手持ちのCityGMLや点群データ、地形データなどを配信し、PLATEAU VIEWなどで利用することができます。

データサイズが50 GBを超える場合、有料のCommercialアカウント以上を利用する必要があります。

### 3.2.2. 作成したDEMのアップロードと変換

FME等で作成した地形データ作成用のDEMを開設したCeisum ionのアカウントにアップロードしてterraindb形式の地形データを準備します。

FMEにより複数の水平解像度のDEMを用意した場合は、海域に標高値を与えた 10B 10A 5C 5B 5A の順にアップロードし、解像度の高いDEMが一番最後にくるようにしてください。

以下では例として、5B 5A のDEMをアップロードし、地形データを作成する手順を示しますが、さらに多くのDEMを重ね合わせて地形データを作成する場合も同じ作業を繰り返すだけです。

- My Assetタブを開き、「Add data」と書かれた青いボタンをクリック

<img src="images/terrain_ceisum_ion_dialog_1.png" alt="fme 10B" style="width:500px;">

- Add Dataページで、「Add files...」と書かれた青いボタンをクリックし、対象となるファイルを指定!

<img src="images/terrain_ceisum_ion_dialog_2.png" alt="fme 10B" style="width:500px;">

- 対象ファイルがリストに現れたら、Asset name（ユーザーが変更可能）を入力し、What kind of data is this？を「Raster Terrain」に指定します。すると画面左下にベースとなるTerrainデータがリストされるので、最初は、Mean Sea Levelを指定します。指定するベースTerrain（地形）は、Mean Sea Levelでなくても構いません。そのうえで、画面左下にある「Upload」ボタンをクリックして、最初のベースとなる5Bデータをアップロードします。

<img src="images/terrain_ceisum_ion_dialog_3.png" alt="fme 10B" style="width:500px;">

- アップロードして地形データとして変換された5Bデータを確認します。次にこの地形データをベース地形データとして、次の地形データ、5Aを重ねます。そのために、再び、「Add data」ボタンをクリックして、次の5AのDEMファイルを指定してください。

<img src="images/terrain_ceisum_ion_dialog_4.png" alt="fme 10B" style="width:500px;">

- 前回と同様に、What kind of data is this？で、「Raster Terrain」を指定し、今回は、Base Terrainとして、先ほどアップロードして変換したデータを指定します。

<img src="images/terrain_ceisum_ion_dialog_5.png" alt="fme 10B" style="width:500px;">

- 以上で、重ねあわされた地形データが完成します。必要に応じてこのプロセスを繰り返し、目的とする地形データを作成します。

<img src="images/terrain_ceisum_ion_dialog_6.png" alt="fme 10B" style="width:500px;">


### 3.3. 地形データの配信について

Ceisumで地形データを利用するには、地形データをサーバーから配信する必要があります。XYZタイルデータなどと異なり、データファイルを置いておくだけでは地形データは利用できません。

ここでは、Cesium ionの配信サービスを利用した方法を説明します。

### 3.3.1. TerriaJSでCesium ionから配信される地形データを表示する方法

前節によりCesium ionで作成したTerraindb形式の地形データをTerriaJSで表示するためには、データカタログJSONファイルに登録するか、config.jsonを変更してデフォルトの地形データとして利用する必要があります。

- データカタログに登録する方法
カタログアイテムの一つとして地形データを読み込む場合は、以下を参考に、カタログアイテムを追加してください（your_xxx の部分は自分のCesium ionの環境に合わせてください）。

```
{
  "name":"地形データ",
  "type":"cesium-terrain",
  "ionAssetId": your_asset_id,
  "ionAccessToken":"your_ion_access_token",
  "description":"地形データは、国土地理院長の承認を得て基盤地図情報数値標高モデルを加工後、配信したものです（承認番号　ＸＸＸＸＸＸＸＸＸ）"
}
```

- デフォルトの地形データとする方法
デフォルトの地形データとして利用するには、配信のための設定ファイルが収められているCesiumのwwwrootフォルダにあるconfig.jsonを編集します。
以下の設定を書き込んで保存してください（your_xxxの部分は自分の環境に合わせてください）。

```
"useCesiumIonTerrain": true,
“cesiumTerrainAssetId”: your_asset_id,
"cesiumIonAccessToken": "your_ion_access_token",
```

config.jsonを書き換えた後は、設定を反映させるため、Cesiumを再起動してください。

## 4. 「PLATEAU Mapbox Terrain Converter」の概要

これまで説明したterraindb形式のデータはCesium向けの地形データ形式ですが、Mapbox GL JSやMapLibre GL JSなどの他の地図エンジンは対応していないという課題があります。
そこで、2024年度の事業において、 CityGML形式のPLATEAU地形モデル（TIN） をMapboxやMapLibreで利用可能な地形データであるMapbox Terrain-RGBに変換するライブラリ「PLATEAU Mapbox Terrain Converter」を開発しました。
ライブラリの利用方法および生成した日本全域の地形データの利用方法については、下記のリポジトリを参照してください。

- [PLATEAU Mapbox Terrain Converter](https://github.com/Project-PLATEAU-Admin/plateau-mb-terrain-converter)
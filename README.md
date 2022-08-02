# PLATEAU配信サービス（試験運用）-チュートリアル

### 1. 本リポジトリについて
Project PLATEAUでは、プロジェクトにおいて作成した3D都市モデルをはじめとする各種データセットをウェブ上で提供するPLATEAU配信サービスを試験運用しています。  
本リポジトリは、PLATEAU配信サービスの利用方法及びこれに関連するナレッジ等を提供するチュートリアルです。

### 2. PLATEAU配信サービス（試験運用）について
Project PLATEAUでは、3D都市モデルの可視化環境としてCesium及びTerriaJSを利用した[PLATEAU VIEW](https://github.com/Project-PLATEAU/PLATEAU-VIEW)を構築・運用しており、PLATEAU配信サービスではPLATEAU VIEWで利用されている各種データセットを無償で利用可能です。  
なお、本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことご了承ください。

### 3. 利用可能なデータセット
 PLATEAU配信サービスは下記のデータセットに対応しています。各データセットごとの利用方法は各チュートリアルを参照してください。

| データセット | 説明 | チュートリアル |
|:-----------|:-----------|:-----------|
| PLATEAU-3DTiles | 建築物モデル等の3DTilesデータ | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) |
| PLATEAU-Terrain | 地形モデルのTerraindbデータ | [plateau-terrain-streaming](/terrain/plateau-terrain-streaming.md) |
| PLATEAU-Ortho | 航空写真オルソ画像タイルデータ | [plateau-ortho-streaming](/ortho/plateau-ortho-streaming.md) |

### 4. 3DTilesの配信URLの取得
配信している3D都市モデルの3DTilesデータのURL一覧は、[3dtiles_url.json](/3dtiles_url.json)から取得することができます。このファイルの仕様は次の通りです。
|フィールド|名称|説明|
|:-----------|:-----------|:-----------|
|CITYCODE|市区町村コード|5桁の市区町村コード|
|TYPE|データ種別|配信している3D都市モデルの地物型の種別です。例えば、bldgは建築物モデルを意味します。地物型の種別の詳細は[「3D都市モデル標準製品仕様書」](https://www.mlit.go.jp/plateaudocument/)の「7.2.3 ファイル名称」を参照してください。|
|URL|URL|3D Tilesのtileset.jsonが置かれているURL |
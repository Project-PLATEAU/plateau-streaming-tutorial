# PLATEAU配信サービス（試験運用）-チュートリアル

### 1. 本リポジトリについて

Project PLATEAUでは、プロジェクトにおいて作成した3D都市モデルをはじめとする各種データセットをウェブ上で提供するPLATEAU配信サービスを試験運用しています。

本リポジトリは、PLATEAU配信サービスの利用方法及びこれに関連するナレッジ等を提供するチュートリアルです。

### 2. PLATEAU配信サービス（試験運用）について

Project PLATEAUでは、3D都市モデルの可視化環境としてCesium及びRe:Earthを利用した[PLATEAU VIEW](https://github.com/Project-PLATEAU/PLATEAU-VIEW-3.0)を構築・運用しており、PLATEAU配信サービスではPLATEAU VIEWで利用されている各種データセットを無償で利用可能です。

なお、本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことご了承ください。

### 3. 利用可能なデータセット

 PLATEAU配信サービスは下記のデータセットに対応しています。各データセットごとの利用方法は各チュートリアルを参照してください。

| データセット | 説明 | チュートリアル |
|:-----------|:-----------|:-----------|
| PLATEAU-3DTiles | 建築物モデル等の3D Tilesデータ | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) |
| PLATEAU-Terrain | 地形モデルのTerraindbデータ | [plateau-terrain-streaming](/terrain/plateau-terrain-streaming.md) |
| PLATEAU-Ortho | 航空写真オルソ画像タイルデータ | [plateau-ortho-streaming](/ortho/plateau-ortho-streaming.md) |

### 4. 3D Tilesの配信URLの取得

配信している3D都市モデルの3D TilesデータのURL一覧は、[3dtiles_url.json](/3dtiles_url.json)から取得することができます。このファイルの仕様は次の通りです。

| フィールド | 名称 | 説明 |
|:-----------|:-----------|:-----------|
| name | 名称 | データの名称 |
| pref | 都道府県名 | 都道府県名 |
| pref_code | 都道府県コード | 2桁の都道府県コード |
| city | 市区町村名 | 市区町村名 |
| city_code | 市の市区町村コード | 5桁の市の市区町村コード |
| ward | 区名 | 5桁の区の市区町村コード。政令指定都市の場合は区ごとにデータが分割されていることがあり、その場合のみ区の名前が含まれます。 |
| ward_code | 区の市区町村コード | 5桁の区の市区町村コード。政令指定都市の場合は区ごとにデータが分割されていることがあり、その場合のみ区ごとの市区町村コードが含まれます。 |
| type | 種別 | 配信している3D都市モデルの地物型の種別。例えば `"建築物モデル"` など。地物型の種別の詳細は[「3D都市モデル標準製品仕様書」](https://www.mlit.go.jp/plateaudocument/)の「7.2.3 ファイル名称」を参照してください。 |
| type_en | 種別（英名） | 配信している3D都市モデルの地物型の種別の英語名。例えば、`"bldg"` は建築物モデルを意味します。地物型の種別の詳細は[「3D都市モデル標準製品仕様書」](https://www.mlit.go.jp/plateaudocument/)の「7.2.3 ファイル名称」を参照してください。 |
| url | URL | 3D Tilesのtileset.jsonが置かれているURL |
| year | 整備年度 | データが整備された年度。例えば、 `2023` の場合は2023年度に整備されたデータであることを意味します。 |
| lod | LOD | データのLOD |
| no_texture | テクスチャなし | 建築物モデルでテクスチャなし版の場合 `true` |

### 5. 更新履歴
| Day | Update | 説明 |
|:-----------|:-----------|:-----------|
| 2024/4/24 | 3D Tiles配信URLの更新 | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧を2023年度データにアップデートしました。|
| 2023/10/25 | 3D Tiles配信URLの更新 | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました。|
| 2023/09/25 | 3D Tiles配信URLの更新 | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました。|
| 2023/04/28 | 3D Tiles配信URLの更新 | [plateau-3dtiles-streaming](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesを新バージョンにアップデートしました。建築物モデル以外も、都市設備モデルや植生モデル等の3D Tilesデータが利用可能となっています。また、[3dtiles_url.json](/3dtiles_url.json)の仕様を新仕様に変更しました。これに伴い、旧配信URLは2023年度中に運用を終了しますので、ご注意ください。|

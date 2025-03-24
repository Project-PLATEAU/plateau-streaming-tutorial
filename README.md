# PLATEAU配信サービス（試験運用） チュートリアル

### 1. 本リポジトリについて

Project PLATEAUでは、プロジェクトにおいて作成した3D都市モデルをはじめとする各種データセットをウェブ上で提供するPLATEAU配信サービスを試験運用しています。

本リポジトリは、PLATEAU配信サービスの利用方法及びこれに関連するナレッジ等を提供するチュートリアルです。

### 2. PLATEAU配信サービス（試験運用）について

Project PLATEAUでは、3D都市モデルの可視化環境としてCesium及びRe:Earthを利用した[PLATEAU VIEW](https://github.com/Project-PLATEAU/PLATEAU-VIEW-3.0)を構築・運用しており、PLATEAU配信サービスではPLATEAU VIEWで利用されている各種データセットを無償で利用可能です。

なお、本サービスはあくまで試験的な運用であるため、提供期間やサービスレベルについては保証できないことをご了承ください。

### 3. 利用可能なデータセット

 PLATEAU配信サービスは下記のデータセットに対応しています。各データセットごとの利用方法は各データセットのリンクからチュートリアルを参照してください。

| データセット | 説明 |
|:-----------|:-----------|
| [PLATEAU-CityGML](/citygml/plateau-citygml.md) | CityGMLデータ |
| [PLATEAU-3DTiles/MVT](/3d-tiles/plateau-3dtiles-streaming.md) | 建築物モデル等の3D Tiles/MVTデータ |
| [PLATEAU-Terrain](/terrain/plateau-terrain-streaming.md) | 地形モデルのTerraindbデータ |
| [PLATEAU-Ortho](/ortho/plateau-ortho-streaming.md) | 航空写真オルソ画像タイルデータ |

### 4. 付録

#### 4.1 Cesium ionに依存しないCesium向け地形データ配信手法

PLATEAU-Terrainの作成、PLATEAU VIEWへのPLATEAU-Terrainの配信にはCesium社が提供するサービスCesium ionを用いていますが、データサイズが50GBを超える場合、有料のCommercialアカウント以上を利用する必要があります。

そこで、Cesium ionに依存しないCesium向けの地形データ変換および配信方法として、「Cesium-Terrain-Builder」と「Cesium-Terrain-Server」の活用について整理しました。
詳細については 、PLATEAU VIEW構築マニュアル（第5.0版）の 付録として公開予定です。

#### 4.2 Mapbox、MapLibre向け地形データ配信手法
PLATEAU-TerrainはCesium向けの地形データ形式（terraindb形式）であるため、Mapbox GL JSやMapLibre GL JSなどの他の地図エンジンは対応していないという課題があります。
そこで、CityGML形式のPLATEAU 地形モデル（TIN形式）をMapboxやMapLibreで利用可能な地形データであるMapbox Terrain-RGBに変換するライブラリ「PLATEAU Mapbox Terrain Converter」を開発しました。

ライブラリの利用方法および生成した日本全域の地形データの利用方法については、下記のリポジトリを参照してください。

- [PLATEAU Mapbox Terrain Converter](https://github.com/Project-PLATEAU-Admin/plateau-mb-terrain-converter)

### 5. 更新履歴

- **2025/03/25**: 付録「4.1 Cesium ionに依存しないCesium向け地形データ配信手法」「4.2 Mapbox、MapLibre向け地形データ配信手法」 を記載しました。また、[PLATEAU-Terrain](/terrain/plateau-terrain-streaming.md) に、[PLATEAU Mapbox Terrain Converter](https://github.com/Project-PLATEAU-Admin/plateau-mb-terrain-converter)に関して記載ました。
- **2025/03/12**: [PLATEAU-CityGML](/citygml/plateau-citygml.md)を追加しました。
- **2024/09/02**: 可視化用データについて、変換仕様の詳細を[PLATEAU 2023 可視化用データ変換仕様](/3d-tiles/specification.md)に記載しました
- **2024/06/10**: [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) と [PLATEAU-Terrain](/3d-tiles/plateau-3dtiles-streaming.md) と [PLATEAU-Ortho](/ortho/plateau-ortho-streaming.md) で配信されている3D Tiles・地形データ・オルソを2023年度のデータにアップデートし、MVTのデータを追加しました。静的なJSONファイルとマークダウンファイルでの配信URL提供に代わり、新たにデータカタログAPIを公開しました。サンプルコードを改訂しました。
- **2023/10/25**: [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました
- **2023/09/25**: [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました
- **2023/04/28**: [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesを新バージョンにアップデートしました。建築物モデル以外も、都市設備モデルや植生モデル等の3D Tilesデータが利用可能となっています。また、3dtiles_url.jsonの仕様を新仕様に変更しました。これに伴い、旧配信URLは2023年度中に運用を終了しますので、ご注意ください

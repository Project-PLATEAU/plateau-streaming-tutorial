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
| [PLATEAU-3DTiles/MVT](/3d-tiles/plateau-3dtiles-streaming.md) | 建築物モデル等の3D Tiles/MVTデータ |
| [PLATEAU-Terrain](/terrain/plateau-terrain-streaming.md) | 地形モデルのTerraindbデータ |
| [PLATEAU-Ortho](/ortho/plateau-ortho-streaming.md) | 航空写真オルソ画像タイルデータ |

### 4. 更新履歴

| Day | Update | 説明 |
|:-----------|:-----------|:-----------|
| 2024/08/30 | 3D Tiles変換仕様を追加 | 可視化用データについて、変換仕様の詳細を[PLATEAU 2023 可視化用データ変換仕様](./specification.md)に記載しました。|
| 2024/06/10 | 3D Tiles/MVT配信方法の刷新・TerrainとOrthoの更新 | [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) と [PLATEAU-Terrain](/3d-tiles/plateau-3dtiles-streaming.md) と [PLATEAU-Ortho](/ortho/plateau-ortho-streaming.md) で配信されている3D Tiles・地形データ・オルソを2023年度のデータにアップデートし、MVTのデータを追加しました。静的なJSONファイルとマークダウンファイルでの配信URL提供に代わり、新たにデータカタログAPIを公開しました。サンプルコードを改訂しました。 |
| 2023/10/25 | 3D Tiles配信URLの更新 | [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました。|
| 2023/09/25 | 3D Tiles配信URLの更新 | [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesの一覧をアップデートしました。|
| 2023/04/28 | 3D Tiles配信URLの更新 | [PLATEAU-3DTiles](/3d-tiles/plateau-3dtiles-streaming.md) で配信されている3DTilesを新バージョンにアップデートしました。建築物モデル以外も、都市設備モデルや植生モデル等の3D Tilesデータが利用可能となっています。また、3dtiles_url.jsonの仕様を新仕様に変更しました。これに伴い、旧配信URLは2023年度中に運用を終了しますので、ご注意ください。|


---

# PLATEAU 2023 可視化用データ変換仕様

## 本仕様書の目的

本仕様書は、3D都市モデル標準製品仕様書3.x版に基づいて整備されたCityGML形式の3D都市モデルデータをCesium環境において可視化するためのCesium 3D Tiles（以下「3D Tiles」という）またはMapbox Vector Tile（以下「MVT」という）データセットへの変換仕様（主に主題属性の構成）を説明するものである。

1. 変換対象の地物型と変換結果データセット名

2. Tiles MVTの主題属性の構成

3. 変換に使用するソフトウェア等

## 1. 変換対象の地物型と変換結果データセット名

標準製品仕様書3.x版で定義されている空間属性持つ地物型のうち、地形モデルを除くすべての地物型を可視化のためのデータ変換の対象とする。CityGMLおよびi-URの仕様に基づくフィーチャークラスについては詳細度別に、実質的に2Dの空間属性を持つもの（標準製品仕様書の規定に基づいてz=0で作成されるもの）はMVTに、3Dの空間属性を持つものは3D Tilesに変換する。公共測量標準図式の応用スキーマに基づくuro:DmGeometricAttribute型（空間属性を持つもの）およびuro: DmAnnotation型の属性として記述されたフィーチャー（以下「DMフィーチャー」と呼ぶ）は、すべてMVTに変換する。

### 表 1 可視化用変換結果データセット名
| #  | パッケージ                 | 変換結果データセット名                          | データ形式  | 概要（*補足説明は後述）                         |
|----|--------------------------|---------------------------------------------|-----------|-----------------------------------------------|
| 1  | 建築物モデル                | bldg_lod1                                  | 3D Tiles  | LOD1建築物モデル                                |
|    |                          | bldg_lod2                                  | 3D Tiles  | LOD2以下の最大詳細度の建築物モデル                    |
|    |                          | bldg_lod3                                  | 3D Tiles  | LOD3以下の最大詳細度の建築物モデル                    |
|    |                          | bldg_lod4                                  | 3D Tiles  | LOD4以下の最大詳細度の建築物モデル                    |
|    |                          | bldg_lod1_no_texture                       | 3D Tiles  | LOD1建築物モデル（テクスチャなし）                     |
|    |                          | bldg_lod2_no_texture                       | 3D Tiles  | LOD2以下の最大詳細度の建築物モデル（テクスチャなし）     |
|    |                          | bldg_lod3_no_texture                       | 3D Tiles  | LOD3以下の最大詳細度の建築物モデル（テクスチャなし）     |
|    |                          | bldg_lod4_no_texture                       | 3D Tiles  | LOD4以下の最大詳細度の建築物モデル（テクスチャなし）     |
|    |                          | bldg_dm_geometric_attributes               | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | bldg_dm_annotations                        | MVT       | DmAnnotation に記述されているジオメトリ            |
| 2  | 交通（道路）モデル           | tran_lod0                                  | MVT       | LOD0道路モデル                                   |
|    |                          | tran_lod1                                  | MVT       | LOD1道路モデル                                   |
|    |                          | tran_lod2                                  | MVT       | LOD2道路モデル（交通領域、交通補助領域）                |
|    |                          | tran_lod3                                  | 3D Tiles  | LOD3道路モデル（交通領域、交通補助領域）                |
|    |                          | tran_dm_geometric_attributes               | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | tran_dm_annotations                        | MVT       | DmAnnotation に記述されているジオメトリ            |
| 3  | 交通（鉄道）モデル           | rwy_lod0                                   | MVT       | LOD0鉄道モデル                                    |
|    |                          | rwy_lod1                                   | MVT       | LOD1鉄道モデル                                    |
|    |                          | rwy_lod2                                   | MVT       | LOD2鉄道モデル（交通領域、交通補助領域）               |
|    |                          | rwy_lod3                                   | 3D Tiles  | LOD3鉄道モデル（交通領域、交通補助領域）               |
|    |                          | rwy_dm_geometric_attributes                | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | rwy_dm_annotations                         | MVT       | DmAnnotation に記述されているジオメトリ            |
| 4  | 交通（徒歩道）モデル          | trk_lod0                                   | MVT       | LOD0徒歩道モデル                                    |
|    |                          | trk_lod1                                   | MVT       | LOD1徒歩道モデル                                    |
|    |                          | trk_lod2                                   | MVT       | LOD2徒歩道モデル（交通領域、交通補助領域）               |
|    |                          | trk_lod3                                   | 3D Tiles  | LOD3徒歩道モデル（交通領域、交通補助領域）               |
|    |                          | trk_dm_geometric_attributes                | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | trk_dm_annotations                         | MVT       | DmAnnotation に記述されているジオメトリ            |
| 5  | 交通（広場）モデル           | squr_lod0                                  | MVT       | LOD0広場モデル                                     |
|    |                          | squr_lod1                                  | MVT       | LOD1広場モデル                                     |
|    |                          | squr_lod2                                  | MVT       | LOD2広場モデル（交通領域、交通補助領域）                |
|    |                          | squr_lod3                                  | 3D Tiles  | LOD3広場モデル（交通領域、交通補助領域）                |
|    |                          | squr_dm_geometric_attributes               | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | squr_dm_annotations                        | MVT       | DmAnnotation に記述されているジオメトリ            |
| 6  | 交通（航路）モデル           | wwy_lod0                                   | MVT       | LOD0航路モデル                                     |
|    |                          | wwy_lod1                                   | MVT       | LOD1航路モデル                                     |
|    |                          | wwy_lod2                                   | MVT       | LOD2航路モデル（交通領域）                             |
| 7  | 土地利用モデル               | luse                                       | MVT       | LOD1土地利用モデル                                   |
| 8  | 災害リスク（浸水）モデル - 洪水 | fld_{admin}_{river}_{scale}                | 3D Tiles  | LOD1洪水浸水想定区域モデル *補足説明1                 |
| 9  | 災害リスク（浸水）モデル - 津波 | tnm_{name}                                 | 3D Tiles  | LOD1津波浸水想定区域モデル *補足説明2                 |
| 10 | 災害リスク（浸水）モデル - 高潮 | htd_{name}                                 | 3D Tiles  | LOD1高潮浸水想定区域モデル *補足説明2                 |
| 11 | 災害リスク（浸水）モデル - 内水 | ifld_{name}                                | 3D Tiles  | LOD1内水浸水想定区域モデル *補足説明2                 |
| 12 | 災害リスク（土砂災害）モデル    | lsld                                       | MVT       | LOD1土砂災害警戒区域モデル                              |
| 13 | 都市計画決定情報モデル          | urf_{class}                                | MVT       | LOD1都市計画決定情報モデル *補足説明3                 |
| 14 | 橋梁モデル                   | brid_lod1                                  | 3D Tiles  | LOD1橋梁モデル                                       |
|    |                          | brid_lod2                                  | 3D Tiles  | LOD2橋梁モデル                                       |
|    |                          | brid_lod3                                  | 3D Tiles  | LOD3橋梁モデル                                       |
|    |                          | brid_lod4                                  | 3D Tiles  | LOD4橋梁モデル                                       |
|    |                          | brid_dm_geometric_attributes               | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | brid_dm_annotations                        | MVT       | DmAnnotation に記述されているジオメトリ            |
| 15 | トンネルモデル               | tun_lod1                                   | 3D Tiles  | LOD1トンネルモデル                                    |
|    |                          | tun_lod2                                   | 3D Tiles  | LOD2トンネルモデル                                    |
|    |                          | tun_lod3                                   | 3D Tiles  | LOD3トンネルモデル                                    |
|    |                          | tun_lod4                                   | 3D Tiles  | LOD4トンネルモデル                                    |
|    |                          | tun_dm_geometric_attributes                | MVT       | DmGeometricAttribute に記述されているジオメトリ       |
|    |                          | tun_dm_annotations                         | MVT       | DmAnnotation に記述されているジオメトリ            |
| 16 | その他の構造物モデル           | cons_lod0                                  | MVT       | LOD0その他の構造物モデル                               |
|    |                          | cons_lod1                                  | 3D Tiles  | LOD1その他の構造物モデル                               |
|    |                          | cons_lod2                                  | 3D Tiles  | LOD2その他の構造物モデル                               |
|    |                          | cons

#### 補足説明1: 災害リスクモデル（浸水想定区域）洪水の変換結果データセット名

- `fld_{admin}_{river}_{scale}`
  - `{admin}`: 河川管理者区分に応じて"natl"（国）または"pref"（都道府県）のいずれか
  - `{river}`: 入力データセットにおけるサブフォルダー名（水系名・河川名を表す）
  - `{scale}`: 洪水規模に応じて"l1"（計画規模）または"l2"（想定最大規模）のいずれか

#### 補足説明2: 災害リスクモデル（浸水想定区域）津波、高潮、内水の変換結果データセット名

- `(tnm|htd|ifld)_{name}`
  - `{name}`: 入力データセットにおけるサブフォルダー名（浸水想定区域の名称を表す）

#### 補足説明3: 都市計画決定情報モデルの変換結果データセット名

- `urf_{class}`
  - `{class}`: 当該データセットに格納される都市計画決定情報モデルのフィーチャークラス名（UrbanPlanningArea, UseDistrict, FirePreventionDistrict など。名前空間修飾子を除く。）

#### 補足説明4: 地下埋設物モデルの変換結果データセット名

- `unf_{class}_(lod[1-4]|dm_geometric_attributes|dm_annotations)`
  - `{class}`: 当該データセットに格納される地下埋設物モデルのフィーチャークラス名（WaterPipe, Duct, Manhole など。名前空間修飾子を除く。）

#### 補足説明5: 植生モデルの変換結果データセット名

- `veg_{class}_(lod[1-3]|dm_geometric_attributes|dm_annotations)`
  - `{class}`: 当該データセットに格納される植生モデルのフィーチャークラス名（SolitaryVegetationObject（単独木）または PlantCover（植被）。名前空間修飾子を除く。）

#### 補足説明6: 汎用都市オブジェクトモデルの変換結果データセット名

- `gen_{code}_lod[0-4]`
  - `{code}`: 当該データセットに格納される汎用都市オブジェクトモデルフィーチャーの `gml:name`の値（汎用都市オブジェクトを識別する名称を示すコード）

## 2. 3D Tiles MVTの主題属性の構成

### 2.1 共通属性

下表に掲げる属性は原則としてすべての3D Tilesフィーチャー、MVTフィーチャーに付加する。ただし、災害リスク（浸水）モデルについては、可視化にあたって軽量化（浸水ランクごとにTINサーフェスを集約した後、外周上の頂点はそのまま残し、内部の頂点数を減らす）を行う都合上、入力データセットの図郭の範囲を超えて浸水ランク別にフィーチャー間の統合を行うので、`meshcode`, `gml_id` 属性は付加しない。

#### 表 2 共通属性

| # | 属性名       | 内容                                                                                          |
|---|--------------|-----------------------------------------------------------------------------------------------|
| 1 | meshcode     | メッシュコード。入力gmlファイル名を_区切りで分割した場合の最初の要素を与える。                                                    |
| 2 | city_code    | 市区町村コード（5桁数字）                                                                             |
| 3 | city_name    | 市区町村名（標準のコードリスト"Common_localPublicAuthorities.xml"にに基づいて取得した市区町村コードに対応する文字列）                |
| 4 | feature_type | CityGML i-URで定義されているフィーチャータイプ（地物型）名                                                    |
| 5 | gml_id       | 各フィーチャーの `gml:id` 属性                                                                           |
| 6 | attributes   | 全ての主題属性を元のXMLの階層構造に準じて格納したJSON文書                                                         |

### 2.2. 地物型別のフラットにする属性

上記共通属性のほか、標準製品仕様書3.2版付属 ”template_objectlist.xlsx” [A.3.1_取得項目一覧] シートにおいて「拡張製品仕様書の対象とすべき主題属性」として「●：データ作成上必須」または「○：原則として入力」と指定されている主題属性（●、○がひとつもないフィーチャータイプについては主要と思われるもの）、および、標準製品仕様書3.4版で追加された不動産ID属性（建築物モデル）、標準製品仕様書3.5版で追加された公園管理属性（建築物、徒歩道、都市設備、橋梁、水部モデル）を抽出し、該当するフィーチャータイプのフィーチャーにそれぞれ単独の属性として付加する。具体的には「PLATEAU2023データ変換仕様【別添】地物型別のフラットにする属性.xslx」参照。

### 2.3. DMフィーチャーの属性

DMフィーチャーには、上記の共通属性および地物型別の固有の属性に加え、さらに表3, 表4に掲げる属性を追加する。

#### 表 3 uro:DmGeometricAttributeフィーチャーに追加する属性

| #  | 属性名                  | 内容                                       |
|----|------------------------|--------------------------------------------|
| 1  | dm_attributes          | 全属性をXMLの階層構造に準じた構造で格納したJSON文書   |
| 2  | dm_dmCode              | DMコードの意味文字列                         |
| 3  | dm_dmCode_code         | DMコード                                    |
| 4  | dm_geometryType        | レコードタイプ                               |
| 5  | dm_geometryType_code   | レコードタイプコード                          |
| 6  | dm_mapLevel            | 地図情報レベル                               |
| 7  | dm_mapLevel_code       | 地図情報レベルコード                          |
| 8  | dm_shapeType           | 図形区分                                    |
| 9  | dm_shapeType_code      | 図形区分コード                               |

#### 表 4 uro:DmAnnotation フィーチャーに追加する属性

| #  | 属性名                  | 内容                                       |
|----|------------------------|--------------------------------------------|
| 1  | dm_attributes          | 全属性をXMLの階層構造に準じた構造で格納したJSON文書   |
| 2  | dm_dmCode              | ＤＭコードの意味文字列                        |
| 3  | dm_dmCode_code         | ＤＭコード                                    |
| 4  | dm_geometryType        | レコードタイプ                               |
| 5  | dm_geometryType_code   | レコードタイプコード                          |
| 6  | dm_shapeType           | 図形区分                                    |
| 7  | dm_shapeType_code      | 図形区分コード                               |
| 8  | dm_label               | 注記文字列                                    |
| 9  | dm_isVertical          | 文字方向                                     |
| 10 | dm_size                | 字大                                        |
| 11 | dm_orientation         | 角度                                        |
| 12 | dm_linewidth           | 線号                                        |
| 13 | dm_spacing             | 字隔                                        |

## 3. 変換に使用するソフトウェア等

可視化用のデータ変換は、FME（カナダSafe Software Inc.製 2022.2.6以降のバージョン）によって表に掲げるワークスペースを実行することにより行うことができる。

#### 表 5 可視化用データ変換ワークスペース一覧
| #   | ワークスペース名                                                 | 備考                                               |
|-----|------------------------------------------------------------------|----------------------------------------------------|
| 1-1 | PLATEAU3 可視化用データ変換01 建築物.fmw                        |                                                    |
| 1-2 | PLATEAU3 可視化用データ変換01-2 政令市の建築物.fmw               | 注1                                                |
| 2   | PLATEAU3 可視化用データ変換02 道路・鉄道・徒歩道・広場・航路.fmw |                                                    |
| 3   | PLATEAU3 可視化用データ変換03 都市設備・植生.fmw                 |                                                    |
| 4   | PLATEAU3 可視化用データ変換04 土地利用・土砂災害警戒区域.fmw     |                                                    |
| 5   | PLATEAU3 可視化用データ変換05 浸水想定区域.fmw                   |                                                    |
| 6   | PLATEAU3 可視化用データ変換06 都市計画決定情報・区域.fmw          |                                                    |
| 7   | PLATEAU3 可視化用データ変換07 橋梁・トンネル・その他の構造物.fmw |                                                    |
| 8   | PLATEAU3 可視化用データ変換08 地下街fmw                          |                                                    |
| 9   | PLATEAU3 可視化用データ変換09 地下埋設物.fmw                     |                                                    |
| 10  | PLATEAU3 可視化用データ変換10 水部.fmw                           |                                                    |
| 11  | PLATEAU3 可視化用データ変換11 汎用都市オブジェクト               |                                                    |

注1: 政令市の建築物モデルの変換にあたっては、#1-2ワークスペースを実行することによって区ごとに#1-1を実行して区単位の3D Tilesを作成することができる。

これらのワークスペース（テンプレート）はFME Hub（下記URL）で公開しており、ウエブブラウザによって同サイトからダウンロードできるほか、インターネットに接続している環境であれば、FME Workbenchメニュー: File > Workspace from Template によって直接ダウンロード、展開することもできる。

[https://hub.safe.com](https://hub.safe.com)

なお、3D Tilesデータセットへの変換を行うワークスペースを実行するには「日本のジオイド2011」ジオイドモデルに基づく楕円体高への変換を行うため、同じくFME Hubで公開されている次のテンプレートに同梱されているグリッドファイルが必要になる。使用方法については関係ワークスペース（下記URL）の説明を参照すること。

[Vertical Transformation with GSIGEO2011](https://hub.safe.com/publishers/pacific-spatial-solutions/templates/vertical-transformation-with-gsigeo2011)

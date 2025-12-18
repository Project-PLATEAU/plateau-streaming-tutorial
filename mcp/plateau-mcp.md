# PLATEAU MCP Server

このパッケージは、PLATEAU（プラトー）の3D都市モデルに関する情報をAIクライアントから簡単にアクセスできるようにするMCP（Model Context Protocol）サーバーを提供します。

> [!WARNING]
> **試験的な提供 - 利用上の注意**
>
> このMCPサーバーは試験的な提供です。以下の点にご注意ください：
>
> - **動作保証・SLA（サービスレベル保証）はありません** - 可用性やパフォーマンスの保証は一切行いません
> - **予告なく変更されることがあります** - ツールの追加・削除・変更、レスポンス形式の変更などが事前告知なく行われることがあります
> - **本番環境での利用は推奨しません** - 検証・評価目的での利用を想定しています
> - **サポートは限定的です** - 問い合わせへの対応は保証されません
>
> 本サービスの利用により生じたいかなる損害についても、提供者は一切の責任を負いません。

## 概要

PLATEAU MCP Serverは、PLATEAUの都市モデルデータおよび仕様書へのアクセスを提供するHTTPベースのMCPサーバーです。以下の情報にアクセスできます：

### データカタログ
- **地域情報**: 都道府県、市区町村などの地域データ
- **データセット情報**: 建物、道路、土地利用などの3D都市モデルデータセット
- **データセット種類**: 利用可能なデータセットの種類と分類
- **メタデータ**: PLATEAU全体の統計情報や仕様バージョン
- **CityGMLデータ**: 空間ID・メッシュコードによるCityGMLファイルの検索と属性取得

### 仕様書
- **3D都市モデル標準製品仕様書**: PLATEAUの3D都市モデルに関する標準仕様
- **3D都市モデル標準作業手順書**: 3D都市モデル作成の標準手順

## AIクライアントでの設定方法

> [!NOTE]
> 以下のセットアップ手順は各AIクライアントの仕様変更により変わる可能性があります。最新の情報は各公式ドキュメントをご確認ください。（2025/12/18 時点の情報）

### Claude Desktop での設定

#### 有料プラン (Pro/Business/Enterprise) の場合

1. Claude Desktop を開く
2. Settings → Integrations を選択
3. 「+ Add Custom Integration」をクリック
4. 以下の情報を入力：
   - **名前**: `PLATEAU MCP`
   - **URL**: `https://api.plateauview.mlit.go.jp/mcp`
   - **詳細設定 → 認証（OAuth）**: 空欄のまま
5. 設定を保存

#### 組織プラン（Enterprise/Business）で管理者が設定する場合

組織の管理者がメンバー全員に MCP サーバーを提供する場合は、以下の手順で設定できます：

1. Claude の管理コンソールにログイン
2. 管理者設定 → コネクタ を選択
3. 「カスタムコネクタを追加」をクリック
4. 以下の情報を入力：
   - **名前**: `PLATEAU MCP`
   - **URL**: `https://api.plateauview.mlit.go.jp/mcp`
   - **詳細設定 → 認証（OAuth）**: 空欄のまま
5. 設定を保存

#### 無料プランの場合

Claude Desktop 無料版はHTTP MCPに対応していないため、HTTP-to-StdioアダプタCLIを使用する必要があります。

1. Claude Desktop を開く
2. Settings → Developer タブを選択
3. 「Edit Config」をクリック
4. 設定ファイルに以下を追加：

```json
{
  "mcpServers": {
    "plateau": {
      "command": "npx",
      "args": [
        "-y",
        "@pyroprompts/mcp-stdio-to-streamable-http-adapter"
      ],
      "env": {
        "URI": "https://api.plateauview.mlit.go.jp/mcp",
        "MCP_NAME": "plateau"
      }
    }
  }
}
```

5. ファイルを保存して Claude Desktop を再起動

> [!IMPORTANT]
> **Node.js のセットアップが必要です**
>
> この方法では `npx` コマンドを使用するため、事前に Node.js をインストールする必要があります。
> [Node.js 公式サイト](https://nodejs.org/) から LTS 版をダウンロードしてインストールしてください。

**参考**:
- 設定手順の詳細は [Claude Desktop ヘルプセンター](https://support.claude.com/en/articles/10949351-getting-started-with-local-mcp-servers-on-claude-desktop) をご確認ください
- HTTP-to-Stdioアダプタの詳細は [@pyroprompts/mcp-stdio-to-streamable-http-adapter](https://www.npmjs.com/package/@pyroprompts/mcp-stdio-to-streamable-http-adapter) を参照

### ChatGPT での設定

ChatGPT (Pro/Team/Enterprise/Edu) では、以下の手順で MCP サーバーを追加できます：

1. ChatGPT の Settings で Developer Mode（開発者モード）を有効化
2. Settings → Apps を選択
3. 「アプリを作成する」をクリック
4. 以下の情報を入力：
   - **名前**: `PLATEAU MCP`
   - **説明**: 任意（例: `日本の3D都市モデル（PLATEAU）のデータカタログと仕様書にアクセスできます`）
   - **アイコン**: 任意
   - **URL**: `https://api.plateauview.mlit.go.jp/mcp`
   - **認証**: `認証なし` を選択
5. 「カスタムMCPサーバーのリスク警告」で「理解したうえで続行」にチェック
6. 設定を保存
7. チャットで「+」ボタン → 「More」→ PLATEAU MCP を選択して利用

**参考**: 設定手順の詳細は [ChatGPT MCP 公式ドキュメント](https://platform.openai.com/docs/mcp) をご確認ください。

### Claude Code での設定

Claude Code では、以下のコマンドで MCP サーバーを追加できます：

```bash
claude mcp add --transport http plateau https://api.plateauview.mlit.go.jp/mcp
```

**参考**: 設定手順の詳細は [Claude Code MCP 公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code/mcp) をご確認ください。

### その他のMCP対応AIクライアント

HTTP MCPをサポートする他のAIクライアントでも、同様にサーバーURLを設定することで利用できます：

```
https://api.plateauview.mlit.go.jp/mcp
```

# 各ツールの説明

## 仕様書ツール

PLATEAU 3D都市モデルの公式仕様書にアクセスするためのツールです。

### 1. `plateau_spec_outline`
仕様書の目次（アウトライン）を取得します。

**パラメータ**:
- `document_type` (optional): ドキュメント種類 - 'standard'（標準製品仕様書、デフォルト）または 'procedure'（標準作業手順書）
- `depth` (optional): 取得する階層の深さ（1=章のみ、2=章+節、3=より深く）。デフォルト: 2。1を指定するとより高速
- `chapter` (optional): 特定の章のみ取得（例: 'toc4' でデータ構造の章）。全体を取得するより高速
- `format` (optional): 出力形式 - 'markdown'（デフォルト）または 'json'

**レスポンス例**:
```markdown
# 3D都市モデル標準製品仕様書 目次

以下のパスを `plateau_spec_read` ツールで指定すると、その節の内容を読むことができます。

- **1 概覧** `/plateaudocument/toc1`
  - **1.1 製品仕様の作成情報** `/plateaudocument/toc1/toc1_01`
  - **1.2 目的** `/plateaudocument/toc1/toc1_02`
  ...
- **4 データ構造** `/plateaudocument/toc4`
  - **4.1 概要** `/plateaudocument/toc4/toc4_01`
  - **4.2 建築物** `/plateaudocument/toc4/toc4_02`
  ...
```

### 2. `plateau_spec_read`
特定の節の内容を取得します。デフォルトで子ページの内容も含めて取得します。

**パラメータ**:
- `path` (required): 読み込むパス（例: '/plateaudocument/toc1' で第1章全体、'/plateaudocument/toc4' で第4章全体）。`plateau_spec_outline` で取得可能なパスを確認できます
- `document_type` (optional): ドキュメント種類 - 'standard'（デフォルト）または 'procedure'
- `single_page` (optional): trueの場合、子ページを含めず指定したページのみ取得。デフォルト: false（子ページ含む）
- `include_images` (optional): trueの場合、base64エンコードされた画像をマークダウンに含める。デフォルト: false（プレースホルダーのみ表示）

**特徴**:
- 子ページを含む全内容を**並列で高速取得**（セマフォで同時リクエスト数を制限）
- 出力が長すぎる場合は**自動的にトランケート**され、より細かい節ごとに取得するためのヒントが表示されます

**レスポンス例**:
```markdown
# 1 概覧

## 1.1 製品仕様の作成情報

| 製品仕様の題名 | 3D都市モデル標準製品仕様書 第5.0版 |
| --- | --- |
| 日付 | 2025/03/21 |
| 作成者 | 国土交通省 |
...

## 1.5 用語と定義

**1.5.1 3D都市モデル**
都市空間の地物及び属性を都市スケールで3次元的に再現したCityGML形式のデータ。

**1.5.2 BIM（Building Information Modeling）**
コンピュータ上に作成した主に三次元の形状情報に加え、...
```

**トランケート時のレスポンス例**:
```markdown
（内容が長い場合、途中でカットされ以下のヒントが表示されます）

---
⚠️ **Output truncated due to length.**

To get the full content, please request smaller sections individually.

Available sub-sections:
- `/plateaudocument/toc4/toc4_01`
- `/plateaudocument/toc4/toc4_02`
- `/plateaudocument/toc4/toc4_03`
...

Use `plateau_spec_read` with these paths to get each section's content.
```

## データカタログツール

### 3. `plateau_get_metadata`
PLATEAU全体のメタデータを取得します。

**パラメータ**: なし

**レスポンス例**:
```json
{
  "available_years": [2020, 2021, 2022, 2023],
  "plateau_specs": [
    {
      "id": "...",
      "major_version": 3,
      "year": 2023,
      "minor_versions": [...]
    }
  ],
  "total_areas": 150,
  "total_datasets": 1200
}
```

### 4. `plateau_search_areas`
地域（都道府県、市区町村）を検索します。

**パラメータ**:
- `parent_code` (optional): 親地域コード（例: "13" で東京都）
- `dataset_types` (optional): データセット種類コードのリスト
- `categories` (optional): カテゴリ（PLATEAU, RELATED, GENERIC）
- `area_types` (optional): 地域タイプ（PREFECTURE, CITY, WARD）
- `search_text` (optional): 検索文字列
- `include_parents` (optional): 親地域を含めるか
- `include_empty` (optional): データセットがない地域も含めるか
- `deep` (optional): 深い階層まで検索するか

**レスポンス例**:
```json
{
  "areas": [
    {
      "id": "...",
      "type": "CITY",
      "code": "13101",
      "name": "千代田区",
      "parent_id": "...",
      "dataset_count": 15
    }
  ],
  "metadata": {
    "total_count": 50,
    "returned_count": 50,
    "has_more": false,
    "refinement_suggestions": []
  }
}
```

### 5. `plateau_get_area`
特定の地域の詳細情報を取得します。

**パラメータ**:
- `code` (required): 地域コード（例: "13101"）

**レスポンス例**:
```json
{
  "id": "...",
  "type": "CITY",
  "code": "13101",
  "name": "千代田区",
  "parent": {
    "id": "...",
    "code": "13",
    "name": "東京都"
  },
  "children": [],
  "planar_crs_epsg_code": "6677"
}
```

### 6. `plateau_search_datasets`
データセットを検索します。

**パラメータ**:
- `area_codes` (optional): 地域コードのリスト
- `dataset_types` (optional): データセット種類コードのリスト
- `categories` (optional): カテゴリ（PLATEAU, RELATED, GENERIC）
- `plateau_spec` (optional): PLATEAU仕様バージョン
- `year` (optional): 整備年度
- `registration_year` (optional): 登録年度
- `search_text` (optional): 検索文字列
- `shallow` (optional): 詳細情報を省略するか

**レスポンス例**:
```json
{
  "datasets": [
    {
      "id": "...",
      "name": "千代田区_建築物モデル（LOD2、テクスチャなし）",
      "description": "...",
      "type": {
        "code": "bldg",
        "name": "建築物モデル",
        "category": "PLATEAU"
      },
      "area": {
        "prefecture": "東京都",
        "city": "千代田区",
        "ward": null
      },
      "year": 2023,
      "registration_year": 2023,
      "plateau_spec": "第3.0版",
      "item_count": 3
    }
  ],
  "metadata": {
    "total_count": 120,
    "returned_count": 100,
    "has_more": true,
    "refinement_suggestions": [
      "地域コードで絞り込む (area_codes パラメータ)",
      "データセット種類で絞り込む (dataset_types パラメータ)"
    ]
  }
}
```

### 7. `plateau_get_dataset`
特定のデータセットの詳細情報を取得します。

**パラメータ**:
- `id` (required): データセットID

**レスポンス例**:
```json
{
  "id": "...",
  "name": "千代田区_建築物モデル（LOD2、テクスチャなし）",
  "description": "...",
  "type": {
    "code": "bldg",
    "name": "建築物モデル",
    "category": "PLATEAU"
  },
  "area": {
    "prefecture": {
      "id": "...",
      "code": "13",
      "name": "東京都"
    },
    "city": {
      "id": "...",
      "code": "13101",
      "name": "千代田区"
    },
    "ward": null
  },
  "year": 2023,
  "registration_year": 2023,
  "plateau_spec": {
    "name": "第3.0版",
    "version": "3.0"
  },
  "groups": ["建築物"],
  "open_data_url": "https://...",
  "items": [
    {
      "id": "...",
      "name": "LOD2（OP2）",
      "format": "CITYGML",
      "url": "https://...",
      "lod": 2,
      "texture": "NONE",
      "layers": []
    }
  ]
}
```

### 8. `plateau_list_dataset_types`
データセット種類の一覧を取得します。

**パラメータ**:
- `category` (optional): カテゴリ（PLATEAU, RELATED, GENERIC）
- `plateau_spec` (optional): PLATEAU仕様バージョン
- `year` (optional): 対象年度

**レスポンス例**:
```json
{
  "dataset_types": [
    {
      "id": "...",
      "code": "bldg",
      "name": "建築物モデル",
      "category": "PLATEAU",
      "year": 2023,
      "dataset_count": 150
    },
    {
      "id": "...",
      "code": "tran",
      "name": "道路モデル",
      "category": "PLATEAU",
      "year": 2023,
      "dataset_count": 120
    }
  ]
}
```

## CityGML ツール

データカタログツールに加えて、CityGMLファイルから直接属性情報を取得するツールも提供しています。

### 9. `plateau_citygml_get_attributes`
CityGMLファイルから指定した建物IDの属性情報を取得します。

**使い方の流れ:**
1. `plateau_get_citygml_files`でメッシュコードや空間IDを指定してCityGML URLを取得
2. レスポンスの`cities[].files[type][].url`からCityGML URLを取得（typeは'bldg', 'tran'など）
3. そのURLと建物IDをこのツールに渡す

**パラメータ**:
- `url` (required): CityGMLファイルのURL
- `building_ids` (required): 取得する建物IDのリスト
- `skip_code_list` (optional): コードリストの取得をスキップするか（デフォルト: false）

**レスポンス例**:
```json
{
  "attributes": [
    {
      "gml:id": "BLD_12345",
      "bldg:measuredHeight": 30.5,
      "bldg:storeysAboveGround": 10,
      "bldg:usage": "住宅",
      "_bbox": {
        "min": {"lng": 139.7, "lat": 35.6, "alt": 0},
        "max": {"lng": 139.71, "lat": 35.61, "alt": 30.5},
        "center": {"lng": 139.705, "lat": 35.605, "alt": 15.25}
      }
    }
  ]
}
```

### 10. `plateau_citygml_get_features`
CityGMLファイルから指定した空間ID（SpatialID）に交差する地物のIDリストを取得します。

**使い方の流れ:**
1. `plateau_get_citygml_files`でメッシュコードや空間IDを指定してCityGML URLを取得
2. レスポンスの`cities[].files[type][].url`からCityGML URLを取得（typeは'bldg', 'tran'など）
3. そのURLと空間IDをこのツールに渡す
4. 返ってきた地物IDを`plateau_citygml_get_attributes`で使用できる

**パラメータ**:
- `url` (required): CityGMLファイルのURL
- `spatial_ids` (required): 検索する空間IDのリスト（例: ["25/52235/23212/25/0"]）

**レスポンス例**:
```json
{
  "feature_ids": [
    "BLD_12345",
    "BLD_12346",
    "BLD_12347"
  ]
}
```

### 11. `plateau_citygml_get_geoid_height`
指定した緯度経度のジオイド高を取得します。日本のジオイド2011に基づいています。標高の楕円体高と正標高の変換に使用できます。

**パラメータ**:
- `latitude` (required): 緯度（度）
- `longitude` (required): 経度（度）

**レスポンス例**:
```json
{
  "latitude": 35.681236,
  "longitude": 139.767125,
  "geoid_height": 39.456,
  "geoid": "39.456"
}
```

### 12. `plateau_get_citygml_files`
指定した条件でCityGMLファイルを検索します。メッシュコード、空間ID、または矩形範囲で検索できます。

**条件フォーマット**:
- メッシュコード: `m:53393580,53393581` (カンマ区切りで複数指定可)
- 空間ID: `s:15/0/29134/12950,15/0/29134/12951` (カンマ区切りで複数指定可)
- 矩形範囲: `r:139.7,35.6,139.8,35.7` (西経度,南緯度,東経度,北緯度)

**地物型フィルタ**:
`feature_types`パラメータで地物型を絞り込むことができます。指定しない場合は全ての地物型を取得します。

主な地物型:
- `bldg`: 建築物モデル
- `tran`: 交通（道路）モデル
- `luse`: 土地利用モデル
- `dem`: 地形モデル
- `fld`: 洪水浸水想定区域モデル
- `lsld`: 土砂災害警戒区域モデル
- `urf`: 都市計画決定情報モデル

利用可能な全ての地物型は`plateau_list_dataset_types`ツールで取得できます。

**使い方の流れ**:
1. このツールでCityGMLファイルURLを取得
2. 取得したURLを`plateau_citygml_get_features`または`plateau_citygml_get_attributes`で使用

**パラメータ**:
- `condition` (required): 検索条件（例: "m:53393580", "s:15/0/29134/12950", "r:139.7,35.6,139.8,35.7"）
- `feature_types` (optional): 取得する地物型のリスト（例: ["bldg", "tran"]）。指定しない場合は全ての地物型を取得

**レスポンス例**:
```json
{
  "cities": [
    {
      "cityCode": "13112",
      "cityName": "世田谷区",
      "year": 2023,
      "registrationYear": 2024,
      "spec": "4.1",
      "url": "https://assets.cms.plateau.reearth.io/assets/.../13112_setagaya-ku_pref_2023_citygml_2_op.zip",
      "files": {
        "bldg": [
          {
            "code": "53393580",
            "maxLod": 2,
            "url": "https://assets.cms.plateau.reearth.io/assets/.../bldg/53393580_bldg_6697_op.gml"
          }
        ],
        "tran": [
          {
            "code": "53393580",
            "maxLod": 2,
            "url": "https://assets.cms.plateau.reearth.io/assets/.../tran/53393580_tran_6697_op.gml"
          }
        ]
      }
    }
  ],
  "featureTypes": {
    "bldg": {
      "name": "建築物モデル"
    },
    "tran": {
      "name": "交通（道路）モデル"
    }
  }
}
```

## ヘルパーツール

### 13. `plateau_explain_spatial_id`
空間ID（Spatial ID）の仕様と使い方を解説するマークダウンを返します。

空間IDは3次元空間を一意に識別するための規格で、`{z}/{f}/{x}/{y}` 形式で表現されます。このツールを呼び出すと、以下の情報を含む解説が返されます：

- 空間IDのフォーマットと各要素の意味
- ズームレベルごとの解像度一覧
- 座標計算式
- PLATEAUツールでの使い方
- 参考リンク

**パラメータ**: なし

**使用例**:
空間IDを使った検索を行う前に、このツールを呼び出して空間IDの仕様を理解してから操作することを推奨します。

## 制限事項

- 検索結果は最大100件までに制限されています
- 100件を超える場合、`metadata.has_more` が `true` となり、`refinement_suggestions` に絞り込み方法が提示されます
- より詳細な検索を行う場合は、パラメータを追加して絞り込んでください

## 技術仕様

このサーバーは [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) の公式仕様に準拠しています。

- プロトコル: [MCP (Model Context Protocol) 2025-06-18](https://modelcontextprotocol.io/specification/2025-06-18)
- トランスポート: HTTP（単一JSONレスポンス形式、SSE非使用）
- メッセージ形式: JSON-RPC 2.0
- 認証: なし（公開データのため）
- 実装ライブラリ: [mark3labs/mcp-go](https://github.com/mark3labs/mcp-go) v0.43.2

## 関連リンク

- [PLATEAU 公式サイト](https://www.mlit.go.jp/plateau/)
- [3D都市モデル標準製品仕様書](https://www.mlit.go.jp/plateaudocument/)
- [3D都市モデル標準作業手順書](https://www.mlit.go.jp/plateaudocument02/)
- [Model Context Protocol (MCP) 仕様](https://modelcontextprotocol.io/specification)

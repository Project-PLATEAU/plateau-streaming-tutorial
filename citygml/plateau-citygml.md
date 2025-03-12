# PLATEAU-CityGML 配信チュートリアル

Project PLATEAUでは、CityGML形式で作成された3D都市モデルのデータの配信を行っており、Project PLATEAUが[G空間情報センター](https://www.geospatial.jp/ckan/dataset/plateau)において公開している都市のCityGMLデータを利用可能です。

ここでは、そのCityGMLデータをより柔軟に取得するためのAPIとして、7種類の「CityGML API」の使用方法を説明します。

- API共通エンドポイント： `https://api.plateauview.mlit.go.jp`
- OpenAPI定義： https://api.plateauview.mlit.go.jp/openapi.json
- APIドキュメント： https://api.plateauview.mlit.go.jp/docs/

> [!WARNING]
> このAPIは試験運用中であり、APIのスキーマやレスポンスは予告なく変更されることがあります。また、提供期間やサービスレベルの保証もしておりません。ご了承の上ご利用ください。
>
> このAPIはクエリによってはレスポンスのサイズが大きくなることがあります。APIはgzip圧縮に対応していますが、モバイル回線などで使用する際は十分ご注意ください。

# 1. CityGML Files API

## 1.1. GET /datacatalog/citygml/{conditions}

- **目的**: 自治体コードやメッシュコードなど、様々な方法で指定した範囲に含まれる各種CityGMLファイルのURLのリストを取得するためのAPI。
- **パラメータ**:
    - `conditions` : 地理的なエリアや都市の情報をフィルタリングするための条件を指定する。詳細は下記「検索条件の種類」を参照。
- **レスポンス**:
    - **200 OK**
    - **400 Bad Request**: 無効な条件が指定された場合（例: メッシュコードや座標範囲の形式が誤っている場合）

        ```json
        {"error":"some error message"}
        ```

    - **404 Not Found**: 指定条件に該当するCityGMLデータが見つからない場合

        ```json
        {"error":"not found"}
        ```

    - **503 Service Unavailable**: ジオコーディングサービスにアクセスできない場合

## 1.2. 検索条件の種類

このAPIでは、 `:conditions` で地理的なエリアや都市の情報をフィルタリングするための条件を指定する。

使用例: `m:533945`, `s:spatialID`, `r:135.5,35.0`, `g:locationName`

`:conditions`パラメータは以下の条件タイプに対応している。

1. **メッシュコード (`m`)**
    - **概要**: カンマ区切りで複数のメッシュコードに基づいてエリアを指定。2次メッシュ（6桁）・3次メッシュ（8桁）・1/2メッシュ（9桁）・1/4メッシュ（10桁）に対応する。メッシュコードはハイフンで区切らず数値だけで表すこと。
    - **形式**: `m:<メッシュコード>`
    - **例**: `m:533944,533945`

        https://api.plateauview.mlit.go.jp/datacatalog/citygml/m:533935

2. **メッシュコード厳密検索 (`mm`)**
    - **概要**: 1とほぼ同じだが、1と比べ、メッシュコードの桁数が完全に一致するCityGMLのみを取得する。2次メッシュの場合は2次メッシュのデータセットのみが、3次メッシュの場合は3次メッシュのCityGMLのみがヒットする。
    - **形式**: `mm:<メッシュコード>`
    - **例**: `mm:533944,533945`

        https://api.plateauview.mlit.go.jp/datacatalog/citygml/mm:533935

3. **空間ID (`s`)**
    - **概要**: カンマ区切りで複数の空間IDを使用してエリアを指定。
    - **形式**: `s:<空間ID>,<空間ID>,...`
        - 空間IDは、 `/z/x/y` 、 `/z/f/x/y` 、およびタイルハッシュの表記に対応。
    - **例**
        - https://api.plateauview.mlit.go.jp/datacatalog/citygml/s:/18/1/232853/103220

        （ `/z/f/x/y` ）

4. **座標範囲 (`r`)**
    - **概要**: 座標範囲を指定してエリアを指定。
    - **形式**:
        - 2要素（`lng`, `lat`）で中心点を指定する場合: `r:lng,lat`
        - 4要素（`lng1`, `lat1`, `lng2`, `lat2`）で範囲を指定する場合: `r:lng1,lat1,lng2,lat2`

            注意：`lng1` < `lng2`, `lat1` < `lat2` となるように指定する必要がある。

    - **例**:
        - 中心点の指定: `r:139.7375,35.65833333333333`

            https://api.plateauview.mlit.go.jp/datacatalog/citygml/r:139.7385,35.65933333333333

        - 範囲の指定: `r:139.7375,35.65833333333333,139.74,35.66`

            https://api.plateauview.mlit.go.jp/datacatalog/citygml/r:139.7375,35.65833333333333,139.74,35.66

5. **ジオコーディング (`g`)**
    - **概要**: ジオコーディングAPIを用いてエリアを指定する。
    - **形式**: `g:<ロケーション名>`
    - **例**: `g:千代田区`

        [https://api.plateauview.mlit.go.jp/datacatalog/citygml/g:千代田区](https://api.plateauview.mlit.go.jp/datacatalog/citygml/g:%E5%8D%83%E4%BB%A3%E7%94%B0%E5%8C%BA)

6. **自治体コード**
    - **概要**: カンマ区切りで複数の自治体コードを指定する。
    - **形式**: `cityId1,cityId2,...`
    - **例**: `13999`

        https://api.plateauview.mlit.go.jp/datacatalog/citygml/13999


# 2. CityGML Pack API

CityGMLのURLのリストを入力すると、関連するCityGMLファイルを含むzipファイルを生成（パック）するAPI。

サーバーの負荷を抑制するためタイムアウトなどの制限が設けられており、また一度作成されたzipファイルは一定時間経過後、削除されることがあることに注意。

## 2.1. POST /citygml/pack

- **目的**: CityGMLファイルを含んだzipファイルの非同期作成をリクエストする。
- **リクエストボディ（JSON）**:
    - `urls` : ファイルのURLのリストを指定。PLATEAU CMSから配信されるファイル（assets.cms.plateau.reearth.io）以外のURLはエラーになる。
- **レスポンス**:
    - **200 OK**: リクエストが受理され、ステータス確認のためのIDが返却される。この `id` フィールドの値は、後で `GET /pack/{id}/status` を使ってパッキングの状態を確認する際に使用する（パックID）。

        ```json
        { "id": "2df41c35a57f7e63a04f422a6843faf047dac428c386bfbc7e2aad393a76472c" }
        ```

    - **400 Bad Request**: 無効なリクエストやURLエラー。
        - リクエストボディが無効な場合

            ```json
            {
              "error": "invalid request body",
              "reason": "invalid JSON"
            }
            ```

        - 無効なURLが含まれている場合

            ```json
            {
              "error": "invalid url",
              "url": "<http://example.com/invalid_file.gml>"
            }
            ```

        - ドメインが無効な場合

            ```json
            {
              "error": "invalid domain",
              "url": "http://another-domain.com/file.gml"
            }
            ```

    - **500 Internal Server Error**: サーバーエラー。
        - サーバーエラーでパックリクエストが処理できなかった場合

            ```json
            { "error": "failed to enqueue pack job" }
            ```

- 呼び出し例

    ```
    curl https://api.plateauview.mlit.go.jp/citygml/pack \
      --json '{
        "urls": [
          "https://assets.cms.plateau.reearth.io/assets/59/984b81-8516-4e13-a8f5-2ca24e272c3e/13101_chiyoda-ku_city_2023_citygml_1_op/udx/bldg/53394509_bldg_6697_op.gml",
          "https://assets.cms.plateau.reearth.io/assets/59/984b81-8516-4e13-a8f5-2ca24e272c3e/13101_chiyoda-ku_city_2023_citygml_1_op/udx/bldg/53394518_bldg_6697_op.gml"
        ]
      }'
    ```


## 2.2. GET /citygml/pack/{id}/status

- **目的**: 指定したIDのCityGMLパックプロセスの状態を確認する。
- **パラメータ**:
    - `id` ：パックID
- **レスポンス**:
    - **200 OK**: 状態を返却（`accepted`, `processing`, `succeeded`, `failed`）。

        ```json
        {
          "status": "succeeded",
          "startedAt": "2025-02-19T05:10:20.907Z",
          "progress": 0.1
        }
        ```

        - status：パック処理のステータス
            - `accepted` - リクエストが受理された。
            - `processing` - パック処理が進行中。
            - `succeeded` - パック処理が完了。
            - `failed` - パック処理が失敗。タイムアウトの場合も含む。
        - startedAt：パック処理の開始日時
        - progress：パック処理の進捗率。0〜1の範囲で示す。完了時は1。
    - **404 Not Found**: ファイルが存在しない場合のエラー。指定したIDのファイルが存在しないか、パック処理が開始されていない場合に返される。

        ```json
        { "error": "not found" }
        ```

- 呼び出し例

    ```
    curl https://api.plateauview.mlit.go.jp/citygml/pack/2df41c35a57f7e63a04f422a6843faf047dac428c386bfbc7e2aad393a76472c/status
    ```


## 2.3. GET /citygml/pack/{id}.zip

- **目的**: 指定したIDのCityGMLパックファイルをダウンロードする。
- **パラメータ**:
    - `id` ：パックID
- **レスポンス**:
    - **302 Found**: リダイレクトしてファイルのダウンロードが開始。
    - **404 Not Found**: ファイルが存在しない場合のエラー。

        ```json
        { "error": "not found" }
        ```

    - **400 Bad Request**: ファイルの状態が不正の場合、またはパック処理が完了していない場合。
- 呼び出し例:

    ```bash
    curl -L -X GET https://api.plateauview.mlit.go.jp/citygml/pack/2df41c35a57f7e63a04f422a6843faf047dac428c386bfbc7e2aad393a76472c.zip -o citygml_pack.zip
    ```


# 3. CityGML 属性API

## 3.1. GET /citygml/attributes

- **目的**: CityGMLのURLとgml_idのリストを入力すると、属性情報をJSONで返却するAPI。
- **クエリパラメータ**:
    - `url` : CityGMLファイルのURLを指定。1つのみ。PLATEAU CMSから配信されるファイル（assets.cms.plateau.reearth.io）以外のURLはエラーになる。
    - `id` : `gml:id` をカンマ区切りで複数指定。
    - `skip_code_list_fetch` : 何らかの値が指定されていれば、コードリスト取得をスキップし生のコードを返す。
- **レスポンス**:
    - **200 OK**: リクエストが受理され、属性情報がJSON形式で返却される。レスポンス内容は以下の呼び出し例を参照。なお、3D Tilesの属性情報に入っているmeshcodeはレスポンス内容に含まない。
    - **400 Bad Request**: 無効なリクエストやURLエラー。
        - 無効なURLが含まれている場合

            ```json
            {
              "error": "invalid url",
              "url": "<http://example.com/invalid_file.gml>"
            }
            ```

        - CityGMLを取得できなかった場合

            ```json
            {
              "error": "cannot fetch",
              "url": "http://example.com/invalid_file.gml"
            }
            ```

        - ドメインが無効な場合

            ```json
            {
              "error": "invalid domain",
              "url": "http://another-domain.com/file.gml"
            }
            ```

    - **500 Internal Server Error**: サーバーエラー。
        - XMLのパースに失敗した場合。

            ```json
            {
              "error": "internal",
              "url": "http://example.com/invalid_file.gml"
            }
            ```

- 呼び出し例

    https://api.plateauview.mlit.go.jp/citygml/attributes?url=https://assets.cms.plateau.reearth.io/assets/b0/cf0c83-7df3-40c4-a95d-7cc66a801cc6/22100_shizuoka-shi_city_2023_citygml_1_op/udx/bldg/52382287_bldg_6697_psc_op.gml&id=bldg_c77c3e2b-ffdc-4b1a-91bf-185a1b46a4d1,bldg_2eb12f7a-c5d9-4145-9609-a6a0f5824368


# 4. CityGML 空間ID検索API

## 4.1. GET /citygml/features

- **目的:** CityGMLのURLと空間IDのリストを入力すると、その空間ID内に含まれる地物のgml_idのリストを返す。
- **クエリパラメータ**:
    - `sid`：空間IDでカンマ区切りで指定可能。/z/x/y・/z/f/x/y・ハッシュタイルが指定可能。
- **レスポンス**:
    - **200 OK**: CityObjectMemberのgml:idが複数返却される。

        ```json
        {
        	"featureIds": [
        		"bldg_09f2c415-e668-4de4-880a-40cfd91a57b1",
        		"bldg_1d1c7c3d-65b0-4228-a4a7-041a5a960bc4",
        		"bldg_55958432-0b29-484b-a4f5-7b366e94f77a",
        		"bldg_a917f23b-8c8e-4491-b184-4ceeab3e8f77"
        	]
        }
        ```

- **呼び出し例**

    https://api.plateauview.mlit.go.jp/citygml/features?url=https://assets.cms.plateau.reearth.io/assets/59/984b81-8516-4e13-a8f5-2ca24e272c3e/13101_chiyoda-ku_city_2023_citygml_1_op/udx/bldg/53394600_bldg_6697_op.gml&sid=/18/0/232840/103234,/18/0/232840/103235


# 5. CityGML 空間ID属性API

## 5.1. GET /citygml/spatialid_attributes

- **目的**: 空間IDと地物型のリストを入力すると、その範囲に含まれる地物の属性情報をJSON形式で返却するAPI。1・4・3のAPIを順番に呼び出す手間を省くことができる。
- **クエリパラメータ**:
    - `sid` : 空間IDをカンマ区切りで複数指定可能。最低1つ以上必要。/z/x/y・/z/f/x/y・ハッシュタイルが指定可能。
    - `type` : 地物型をカンマ区切りで複数指定可能。最低1つ以上必要。
- **レスポンス**:
    - **200 OK**: リクエストが受理され、属性情報がJSON形式で返却される。レスポンスのスキーマはCityGML属性APIと同じ。
    - **400 Bad Request**: 無効なリクエストやURLエラー。
        - sidパラメータが不足の場合（最低1つは必要）

            ```
            { "error": "sid parameter is required" }
            ```

        - typeパラメータ不足の場合（最低1つは必要）

            ```
            { "error": "type parameter is required" }
            ```

        - CityGMLを取得できなかった場合

            ```json
            {
              "error": "cannot fetch",
              "url": "http://example.com/invalid_file.gml"
            }
            ```

    - **404 Not Found**: 検索したが見つからなかった場合。
        - CityGMLが見つからなかった場合

            ```json
            { "error": "not found" }
            ```

        - CityGMLに指定された地物型のち物が存在しなかった場合

            ```json
            { "error": "no citygml files for the given types" }
            ```

        - 指定した空間ID内に地物が見つからなかった場合

            ```json
            { "error": "no features found" }
            ```

    - **500 Internal Server Error**: サーバーエラー。
        - XMLのパースに失敗した場合

            ```json
            {
              "error": "internal",
              "url": "http://example.com/invalid_file.gml"
            }
            ```

        - CityGMLから属性の抽出に失敗した場合

            ```json
            { "error": "failed to extract attributes
            ```

        - その他内部エラー

            ```json
            { "error": "internal" }
            ```

- 呼び出し例

    https://api.plateauview.mlit.go.jp/citygml/spatialid_attributes?sid=/18/0/232840/103234,/18/0/232840/103235&type=bldg,veg,tran

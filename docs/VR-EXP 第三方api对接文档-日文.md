## 目次
```plain
1. APIポート概要およびテストデータ
2. ポート命名規則 
3. ポート説明
	3.1 第三者サービスプロバイダー接続説明
		3.1.1 第三者ポート認証取得
		3.1.2 第三者のQRコード取得ポート
		3.1.3 Visbody プッシュメッセージ統合通知メッセージ
		3.1.4 第三者状態ステータスコード
	3.2 Visbodyポート認証の取得
		3.2.1 Visbodyポート認証の取得
		3.2.2 ユーザー情報の関連付け
	3.3 身体測定ファイルおよびデータの取得
		3.3.1 体組成データの取得
 			3.3.1-1 vr-explorer 体組成データの取得
    	3.3.1-2 S30 体組成データの取得
		3.3.2 ユーザー脂肪率ランクの取得
		3.3.3 身体評価点数の取得
		3.3.4 体組成調節データの取得
	3.4 姿勢ファイルおよびデータの取得
		3.4.1 姿勢ファイルの取得
		3.4.2 姿勢データの取得
		3.4.3 各部位ごとの周囲長ファイルの取得
		3.4.4 各部位ごとの周囲長データの取得
	3.5 肩部測定データの取得
	3.6 プリントレポートの取得
	3.7 Visbodyリターンステータスコードの説明
	3.8 プッシュメッセージタイプとポート関連の説明
```

## 1. ファイル紹介
### 1.1 APIポート概要
| ポート | ポート機能 |
| :--- | :--- |
| 第三者サービスプロバイダー認証ポートの取得 | アクセス可能な第三者サービスプロバイダーのポート認証アドレスを取得します。第三者サービスプロバイダーから提供されるもので、ポートタイプおよび受信側リターンパラメータ（戻り値）は3.1.1を参照してください。 |
| 第三者QRコード取得ポートのリクエスト | ユーザー側がデバイスにて測定完了後、第三者サービスプロバイダーから提供される結果QRコードで、ポートタイプおよび受信側リターンパラメータ（戻り値）は3.1.2を参照してください。 |
| Visbody プッシュメッセージ統合通知メッセージ | 身体測定/姿勢/局部分布結果および読み取った関連情報で、このポートから第三者サービスプロバイダーにプッシュメッセージに転用されるものです。第三者サービスプロバイダーから提供されるもので、ポートタイプおよび受信側リターンパラメーター（戻り値）は3.1.3を参照してください。 |
| Visbodyポート認証の取得 | 第三者サービスプロバイダーがこのポートで取得したVisbodyに関連するポート認証を転用し、vfidおよびvfsecretをVisbody管理プラットフォームから取得します。 |
| ユーザー情報の関連付け | 第三者サービスプロバイダーのAPPがデバイスのQRコードを読み取った後、第三者サービスプロバイダーのバックグラウンドでユーザー身分の認証を行い、このポートのVisbodyバックグラウンドサービスを転用して統合リクエストを開始します。 |
| 身体測定ファイルデータポート | 身体測定模型の統合が成功し、第三者サービスプロバイダーがこのポートで取得した身体測定模型とデータを転用します。 |
| 姿勢ファイルデータポート | 姿勢模型の統合が成功し、第三者サービスプロバイダーがこのポートで取得した姿勢模型とデータを転用します。 |
| リターンステータスコードの説明 | ステータスコードの説明 |


**1.2　テストデータ**

| パラメータ名 | パラメータ値 |
| :--- | :--- |
| key | <font style="color:rgb(23, 26, 29);">vfbuqo5Tmnvrqe25</font> |
| secret | <font style="color:rgb(23, 26, 29);">GpBIk52k83PGvyzkeFDtfCUs3kaX8vUg</font> |
| scanid | <font style="color:rgb(23, 26, 29);">35042104080001-7254507f-50da-11ec-b365-300ed55248eb</font> |


## 2. ポート規則
### 2.1 命名規則
https://[ドメイン名]/[バージョン]/[ポート名]

例：<u><font style="color:#0000FF;">http://api.explorer.visbody.com/v1/token</font></u>

| 実例： | 説明 |
| :--- | :--- |
| api.explorer.visbody.com | ドメイン名 |
| v1 | バージョン |
| token | ポート名 |


### 2.2 バージョンコントロール
ポートバージョンはパスを通じて管理

```plain
HTTP GET:
// v1バージョン
http://api.explorer.visbody.com/v1/token
// v2バージョン
http://api.explorer.visbody.com/v2/token
```

### 2.3 POST提出方法
```plain
Content-Type: application/json
```

## 3. ポート説明
### 3.1 第三者サービスプロバイダー接続説明
現在API接続/QRコード接続の2種類の方法をサポートします。

#### １. API接続
取扱説明:** **

Visbodyが提供するAPI接続を通じてユーザーの測定データを取得を可能にします。

接続成功後Visbodyは客先が割り当てる3.1.3ポート∂を介して読み取りIDなどの関連情報をプッシュし、客先は測定項目結果に基づき対応するポートにアクセスしデータを取得します。統合プッシュタイプおよびポートの関係は3.8の説明を参照ください。

<!-- 这是一张图片，ocr 内容为：第三者妙-匕久 B端末管理75以 visbodyAPI 八y夕夕力沙 颜客 分八竹夕一 寸一久 卜7才一A API接统杀定 VFSECRETO夕- 统合成功通知 secret介Ltvisbody一卜解证取得 术一卜韶证小夕-> 疏孙取宁一夕取得 宁一夕小夕一> -->
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21651137/1631523493590-45959421-85e0-42a0-ac85-df18dccf5566.jpeg)

**接続説明：**

+ API接続権限のリクエスト
+ Visbody管理プラットフォームを介するAPIの接続設定は3.1.1/3.1.3ポートを割当

#### 2. QRコード接続
**取扱説明: **

デバイス初期設定のシリアル番号を変換し、コードを読み取った後客先自身のAPPもしくはミニアプリなどその他プラットフォームにジャンプします。

<!-- 这是一张图片，ocr 内容为：第三者寸-匕久 VisbodyAPI 癫客 了八仁久 八夕分分 分八1夕一 寸一久 宁八仁QR了一下要求 QRD一下术-卜1夕工卜 QRD一FoU夕一2 QRD一下oU夕> 疏子取 工寸-情辍关连付( 纽付(OK 统合通知开始 QR7一1o夕口一文 统合通知成功 疏办取宁一夕取得 画面表示 说孙取0示一夕好夕 -->
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21651137/1631523509769-569a5c59-7298-4e20-be77-b08d6107c018.jpeg)

**接続説明：**

+ API接続権限のリクエスト
+ Visbody管理プラットフォームを介するAPIの接続設定は3.1.1/3.1.2/3.1.3ポートを割当
+ ユーザーのQRコードを読み取った後、Visbody3.2.2ポートがユーザー情報の紐付けおよび統合をリクエストします。

#### 3.1.1 第三者ポート認証取得 :id=third-token
**ポート説明：**

客先から取得でき、3.1.2、3.1.3などポート認証情報アドレスなどで第三者サービスプロバイダーのポートとアクセス可能にします。

**リクエストURLフォーマット要件：**

+ `<http|https>://<ドメイン名>/<path>`
+ `http://<host>:<port>/<path>`

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| key | YES | string | ユーザー独自の認証で、第三者サービスプロバイダーから提供 |
| secret | YES | string | ユーザー独自の認証暗号化キーで、第三者サービスプロバイダーから提供 |


**正常時のリターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "token": "TOKEN",
      "expires_in": 7200
    }
  }
```

**エラー時のリターン例**

```plain
  {
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| code | int | ステータスコード、リターンされるステータスコードは3.1.5の説明を参照ください。 |
| error_msg | string | エラー情報 |
| token | string | ポート認証 |
| expires_in | int | 認証有効時間（単位：秒） |


#### 3.1.2 第三者のQRコード取得ポート :id=get-grcode
**ポート説明：**

ユーザー側がデバイスにて測定完了後の結果QRコードで、第三者サービスプロバイダーから提供されます。

**リクエストURLフォーマット要件：**

+ `<http|https>://<ドメイン名>/<path>`
+ `http://<host>:<port>/<path>`

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| scan_id | YES | string | 読み取りID |
| device_id | YES | string | デバイスID |
| token | YES | string | 第三者サービスプロバイダーポート認証 |


**正常時のリターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "url": "qrCodeUrl",
    }
  }
```

**エラー時のリターン例**

```plain
  {
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| code | int | ステータスコード、リターンされるステータスコードは3.1.4の説明を参照ください。 |
| error_msg | string | エラー情報 |
| url | string | QRコードアドレス |


#### 3.1.3 Visbody プッシュメッセージ統合通知メッセージ :id=notify
**ポート説明：**

ユーザーがデバイス側でQRコードを読み取った後、身体測定/姿勢/局部分布結果および読み取った関連情報で、このポートから第三者サービスプロバイダーにプッシュメッセージに転用されます。

**リクエストURLフォーマット要件：**

+ `<http|https>://<ドメイン名>/<path>`
+ `http://<host>:<port>/<path>`

**リクエスト方法：**

+ `POST`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| scan_id | YES | string | 読み取りID |
| device_id | YES | string | デバイスID |
| time | YES | date | 完了時間　時間表示フォーマット：　2018/06/25　10:10:10 |
| user_info | NO | object | ユーザー情報 |
| age | NO | int | 年齢 |
| birthday | NO | string | 生年月日 |
| phone | NO | string | 携帯電話 |
| sex | NO | string | F=女性，M=男性 |
| height | NO | int | 身長 |
| action_status | YES | object | ステータス情報 |
| girth_status | NO | int | 身体全体の周囲長統合ステータス　0＝エラー/1＝成功/2＝タイムアウト |
| eval_status | NO | int | 姿勢の統合ステータス　0＝エラー/1＝成功/2＝タイムアウト |
| bia_status | NO | int | 体組成の統合ステータス　0＝エラー/1＝成功/2＝タイムアウト |
| eval_shoulder_status | NO | int | 頸肩部の統合ステータス　0＝エラー/1＝成功/2＝タイムアウト |
| pdf_status | NO | int | pdfのプリントアウト可否　0＝no/1＝Yes |
| token | YES | string | 第三者サービスプロバイダーポート認証 |


**フォーマットは下記の通り。**

```plain
{
  "user_info": {
    "age": 26,
    "birthday": "1992-12-12",
    "height": 178,
    "phone": "13812345678",
    "sex": "f"
  },
  "action_status":{
    "measure_status":0,
    "eval_status":0,
    "bia_status":0,
    "bodypredict_status":0,
    "tchar_status":0,
    "eval_dynamic_status":0
  },
  "device_id": "20041910080001",
  "scan_id": "20041910080001-a210136e-1bfb-11ea-b711-00d861a9ecd9",
  "time": "2019-12-11 17:56:15",
  "pdf_status":1,
  "token": "TOKEN"
}
```

**正常時のリターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0
  }
```

通知リクエストを正しく受信した場合には必ず応答しなければならず、応答しない場合Visbodyサーバーから再送を3回行います。

**エラー時のリターン例**

<font style="color:#595959;">  </font>

```plain
{
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| code | int | ステータスコード、リターンされるステータスコードは3.1.4の説明を参照ください。 |
| error_msg | string | エラー情報 |


#### 3.1.4 第三者状態ステータスコード
詳細：第三者サービスプロバイダーの上記のポートは下記のステータスコードによって関連するステータス情報を返すものとします。

| ステータスコード | 備考 |
| :--- | :--- |
| 0 | リクエスト成功 |
| 30001 | 無効なtoken |


### 3.2 Visbodyポート認証の取得 :id=get-token
####パラメータを含むリクエストヘッダー

+ リクエストヘッダー中のリターンパラメータに対応する言語と単位のレポートデータに基づき  
リクエストヘッダーの中に言語および単位を付加します。

```plain
$headers[] = “Unit: $vfUnit; // vfUnit 転送可能なパラメータ単位はmetric(メトリック法) / imperial (ヤードポンド法） 
$headers[] = “Language: $vfLanguage; // vfLanguage 転送可能な言語は en-US(アメリカ英語) / ja-JP(日本語) / zh-CN(中国語)
```

#### 3.2.1 Visbodyポート認証の取得
**ポート説明：**

+ 取得したVisbodyポートを用いて認証を転用します。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/token](http://api.vr-explorer.visbody.com/v1/token)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| key | YES | string | 第三者サービスプロバイダーの唯一の認証、すなわちvfid |
| secret | YES | string | 第三者サービスプロバイダーの唯一の認証暗号化キー、すなわちvfsecret |


**正常時のリターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "token": "TOKEN",
      "expires_in": 7200
    }
  }
```

**エラー時のリターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 40001,
    "error_msg": 'ERROR_MSG'
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| code | int | ステータスコード、リターンされるステータスコードは3.8の説明を参照ください。 |
| error_msg | string | エラー情報 |
| token | string | ポート認証 |
| expires_in | int | 認証有効時間（単位：秒） |


**vfid、vfsecret取得方法：**

+ 1. Visbody管理システムアカウントの新規作成。
+ 2. API接続権限の開通リクエスト。
+ 3. Visbody管理システムのデバイスの接続ページで画面に表示されるAPI接続を完了させた後、vfid、vfsecretを取得。

Tokenの使用方法1.

1. POSTもしくはGETリクエストの中で通常のパラメータを転送

2. リクエストヘッダーの中にAuthorization Bearer Tokenを付加（phpを例とする）

```plain
$headers[]  =  "Content-Type: application/json";
$headers[]  =  "Authorization: Bearer ". $vfToken;
```

#### 3.2.2 ユーザー情報の関連付け
**ポート説明：**

+ 第三者サービスプロバイダーのAPPがデバイスのQRコードを読み取った後、第三者サービスプロバイダーのバックグラウンドでデバイスとユーザー身分の認証を行い、このポートのVisbodyバックグラウンドサービスを転用して統合リクエストを開始します。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/dataBind](http://api.vr-explorer.visbody.com/v1/dataBind)

**リクエスト方法：**

+ `POST`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| --- | --- | :--- | --- |
| scan_id | YES | string | 読み取りID |
| device_id | YES | string | デバイスID |
| third_uid | YES | string | 第三者サービスプロバイダーの唯一のIdはアルファベットと数字で、長さは8～40文字とします。 |
| mobile | YES | string | 携帯電話 |
| sex | YES | int | 性別　1＝男性/2＝女性 |
| height | YES | int | 身長　110～205（cm） |
| age | YES | int | 年齢範囲は満10～99歳の間になります。 |
| token | YES | string | ポート認証 |
| name | YES | string | ユーザーは |


**正常時のリターン例**

```plain
  {
    "code": 0,
    "data": {
      "result":TRUE
    }
  }
```

**エラー時のリターン例**

```plain
  {
    "code": 40001,
    "error_msg": 'ERROR_MSG'
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| code | int | ステータスコード、リターンされるステータスコードは3.6の説明を参照ください。 |
| error_msg | string | エラー情報 |
| result | boolean | 統合結果の開始 |


Tokenの使用方法1.

1. POSTもしくはGETリクエストの中で通常のパラメータを転送

2. リクエストヘッダーの中にAuthorization Bearer Tokenを付加（phpを例とする）

```plain
$headers[]  =  "Content-Type: application/json";
$headers[]  =  "Authorization: Bearer ". $vfToken;
```

### 3.3 身体測定ファイルデータの取得
#### 3.3.1 体組成データの取得
#### 3.3.1-1 vr-explorer 体組成データの取得
**ポート説明：**

+ 身体測定体組成データを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/measure/mass](http://api.vr-explorer.visbody.com/v1/measure/mass)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
    	"WT": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "FFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "LM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "TBW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BMI": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "PBF": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BMR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "WHR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "SM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "TM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "PROTEIN": {"l":10,"m":15,"h":20,"v":30.3,"status":3}，
                "ECWRATIO": {
            "l": 0.2,
            "m": 0.2,
            "h": 0.2,
            "v": 0.2,
            "status": 2
        }
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | --- |
| WT | object | 体重（kg） |
| FFM | object | 除脂肪体重（kg） |
| BFM | object | 体脂肪量(kg) |
| LM | object | 筋肉量(kgt) |
| TBW | object | 体内水分(kg) |
| BMI | object | ボディマス指数(BMI) |
| PBF | object | 体脂肪率（%） |
| BMR | object | 基礎代謝量(kcal/d) |
| WHR | object | ウエストヒップ比 |
| SM | object | 骨格筋量(kg) |
| TM | object | 無機塩(kg) |
| PROTEIN | object | タンパク質(kg) |
| ECWRATIO | object | 細胞外液比率 |


**体組成範囲の説明**

```plain
{
	"l":10,        // 下限値
	"m":15,        // 標準値
	"h":20,        // 上限値
	"v":30.3,      // 測定値
	"status":3     // 状態 1＝低，２＝正常，３＝高
}
```



#### 3.3.1-2 vr-explorer 体組成データの取得
**ポート説明：**

+ 身体測定体組成データを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/measure/mass](http://api.vr-explorer.visbody.com/v1/measure/mass)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| --- | --- | :--- | --- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |
| type | NO | string | デバイスタイプ  1 はS 30データタイプ |


**リターン例**

```plain
  {
    "code": 0,
    "data": {
    	"WT": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"FFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"TBW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BMI": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"PBF": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BMR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"WHR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"SM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"TM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"PROTEIN": {"l":10,"m":15,"h":20,"v":30.3,"status":3}，
		"ICW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"ECW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
    "METABOLICAGE": {
            "l": 0,
            "m": 0,
            "h": 0,
            "v": 22,
            "status": 0
        },
                  "ECWRATIO": {
            "l": 0.2,
            "m": 0.2,
            "h": 0.2,
            "v": 0.2,
            "status": 2
        }
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | --- |
| WT | object | 体重（kg） |
| FFM | object | 除脂肪体重（kg） |
| BFM | object | 体脂肪量(kg) |
| LM | object | 筋肉量(kgt) |
| TBW | object | 体内水分(kg) |
| BMI | object | ボディマス指数(BMI) |
| PBF | object | 体脂肪率（%） |
| BMR | object | 基礎代謝量(kcal/d) |
| WHR | object | ウエストヒップ比 |
| SM | object | 骨格筋量(kg) |
| TM | object | 無機塩(kg) |
| PROTEIN | object | タンパク質(kg) |
| ICW | object | 細胞内液（kg） |
| ECW | object | 細胞外液（kg） |
| METABOLICAGE | object | 代謝年齢 |
| ECWRATIO | object | 細胞外液比率 |


**体組成範囲の説明**

```plain
{
	"l":10,        // 下限値
	"m":15,        // 標準値
	"h":20,        // 上限値
	"v":30.3,      // 測定値
	"status":3     // 状態 1＝低，２＝正常，３＝高
}
```

#### 3.3.2 ユーザー脂肪率ランクの取得
**ポート説明：**

+ ユーザーの脂肪レベルに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/body/state](http://api.vr-explorer.visbody.com/v1/body/state)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
			"va_grade":9,
			"body_age":18,
			"body_share":3
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| va_grade | int | 内臓脂肪レベル（1～10：正常/10～14：やや高め/14～17：高め/17以上：高すぎ） |
| body_age | int | 身体年齢 |
| body_share | int | 体型判断（1：細身体型/2：筋肉体型/3：肥満体型/4：健康体型） |


#### 3.3.3 ユーザー身体評価点数の取得
**ポート説明：**

+ ユーザーの身体評価点数に用いられます。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/body/score](http://api.vr-explorer.visbody.com/v1/body/score)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |
| scan_type | YES | int | 読み取りタイプ　1：身体測定/2：姿勢 |


**リターン例**

```plain
  {
    "code": 0,
    "data": {
      "score": 98
    }
  }
```

**リターンパラメータの説明:**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| score | int | 項目評価点 |


#### 3.3.4 身体成分調節データの取得
**ポート説明：**

+ 体組成調節データを取得するのに用います。

**リクエストURL：**

+ [http://api.vrpro3.visbody.com/v1/forecast/adjust](http://api.vrpro3.visbody.com/v1/forecast/adjust)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "weight":1.8,
	  	"body_fat":-2.6,
	  	"muscle":-2.6,
	  	"gr_weight":75,
	  	"gr_body_fat":12.3,
	  	"gr_muscle":15.1,
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| weight | double | 体重調節量 |
| body_fat | double | 体脂肪調節量 |
| muscle | double | 筋肉調節量 |
| gr_weight | double | 体重黄金比率 |
| gr_body_fat | double | 体脂肪黄金比率 |
| gr_muscle | double | 筋肉黄金比率 |


#### 3.3.5 セグメントデータの取得
**API概要：**

+ **セグメントデータを取得するためのAPIです。**

**リクエストURL：**

+ `**http://api.explorer.visbody.com**/v1/seg/spread/data`

**リクエストメソッド：**

+ GET

**パラメータ：**

| パラメータ名 | 必須 | 型 | 説明 |
| --- | --- | --- | --- |
| token | はい | string | アクセストークン |
| scan_id | はい | string | スキャンID |


**レスポンス例：**

```plain
  {
    "code": 0,
    "data": {
    	"BFMLA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMRA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMTR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMLL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMRL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMLA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMRA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMTR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMLL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMRL": {"l":10,"m":15,"h":20,"v":30.3,"status":3}
    }
  }
```

**レスポンスパラメータの説明：**

| パラメータ名 | 型 | 説明 |
| --- | :--- | --- |
| BFMLA | object | 左上肢のセグメント脂肪量 (kg) |
| BFMRA | object | 右上肢のセグメント脂肪量 (kg) |
| BFMTR | object | 体幹のセグメント脂肪量 (kg) |
| BFMLL | object | 左下肢のセグメント脂肪量 (kg) |
| BFMRL | object | 右下肢のセグメント脂肪量 (kg) |
| LMLA | object | 左上肢のセグメント筋肉量 (kg) |
| LMRA | object | 右上肢のセグメント筋肉量 (kg) |
| LMTR | object | 体幹のセグメント筋肉量 (kg) |
| LMLL | object | 左下肢のセグメント筋肉量 (kg) |
| LMRL | object | 右下肢のセグメント筋肉量 (kg) |


_**セグメント範囲オブジェクトの説明**_

```plain
  {
	"l":10,        // 下限値
	"m":15,        // 標準値
	"h":20,        // 上限値
	"v":30.3,      // 測定値
	"status":3     // ステータス：1 低い, 2 正常, 3 高い
  }
```

**範囲パラメータの説明：**

| パラメータ名 | 型 | 説明 |
| :--- | :--- | :--- |
| l | double | 下限値 |
| m | double | 標準値 |
| h | double | 上限値 |
| v | double | 測定値 |
| status | int | ステータス (1: 低い, 2: 正常, 3: 高い) |


### 3.4 姿勢ファイルおよびデータの取得
#### 3.4.1 姿勢ファイルの取得
**ポート説明：**

+ 姿勢模型/キーポイントjsonファイルおよび姿勢撮影画像を取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/shape/file](http://api.vr-explorer.visbody.com/v1/shape/file)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

```plain
  {
    "code": 0,
    "data": {
      "model_url": "MODEL_URL",
	  	"json_url": "JSON_URL",
	  	"pic_front_url": "PIC_FRONT_URL",
	  	"pic_left_url": "PIC_LEFT_URL",
	  	"pic_right_url": "PIC_RIGHT_URL",
	  	"pic_top_url": "PIC_TOP_URL",
      "expires_in": 7200
    }
  }
```

**リターンパラメータの説明:**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | --- |
| model_url | string | 姿勢模型ファイルパス、ファイル形式＝.obj |
| json_url | string | 姿勢キーポイントjsonファイルパス、ファイル形式＝.txt（姿勢模型高価のキーポイントを表示するのに用います） |
| pic_front_url | string | 姿勢の正面画像ファイルパス、ファイル形式＝.jpg |
| pic_left_url | string | 姿勢左側面画像ファイルパス、ファイル形式＝.jpg |
| pic_right_url | string | 姿勢右側面画像ファイルパス、ファイル形式＝.jpg |
| pic_back_url | string | 体勢背面ビュー画像ファイルパス、ファイル形式.jpg |
| pic_top_url | string | 姿勢上面画像ファイルパス、ファイル形式＝.jpg |
| expires_in | int | ファイルパス有効時間 |


#### 3.4.2 姿勢データの取得
**ポート説明：**

+ 姿勢評価身体データを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/shape/points](http://api.vr-explorer.visbody.com/v1/shape/points)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
		"high_low_shoudler": {
			"val": 15.96,
			"conclusion": "怒り肩/なで肩(左高)",
			"risk": :怒り肩/なで肩は頚椎部の慢性的な痛みを引き起こす可能性があり、多くの場合、脊柱管狭窄症、骨盤変位、下肢の長さ不揃いを伴うことがあります。"
		},
		"head_slant": {
			"val": 15.96,
			"conclusion": "正常",
			"risk": "--"
		},
		"head_forward": {
			"val": 15.96,
			"conclusion": "正常",
			"risk": "--"
		},
		"leg_xo": {
			"left_val": 184.3,
			"right_val": 187.2,
			"conclusion": "正常",
			"risk": "--"
		},
		"pelvis_forward": {
			"val": 15.96,
			"conclusion": "正常",
			"risk": "--"
		},
		"left_knee_check": {
			"val": 175.4,
			"conclusion": "正常",
			"risk": "--"
		},
		"right_knee_check": {
			"val": 187.7,
			"conclusion": "正常",
			"risk": "--"
		},
		"round_shoulder_left": {
			"val": 15.6,
			"conclusion": "正常",
			"risk": "--"
		},
		"round_shoulder_right": {
			"val": 15.6,
			"conclusion": "正常",
			"risk": "--"
		},
		"body_slope": {
			"val": 0,
			"conclusion": "正常",
			"risk": "--"
		}
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| high_low_shoudler | object | 怒り肩/なで肩（cm/in) |
| head_slant | object | 頭部の後傾 (°)  |
| head_forward | objectze | 頭部の前傾(°) |
| leg_xo | object | 脚タイプ (°)  |
| pelvis_forward | object | 骨盤変位　前/後　（°） |
| left_knee_check | object | 左側膝蓋分析　（°） |
| right_knee_check | object | 右側膝蓋分析　（°） |
| round_shoulder_left | object | 左側巻き肩（°） |
| round_shoulder_right | object | 右側巻き肩（°） |
| body_slope | object | 身体傾き（°） |
| val | double | 測定値 |
| left_val | double | 左足測定値 |
| right_val | double | 右足測定値 |
| conclusion | string | 評価結論 |
| risk | string | リスクに関して　正常であれば"--"が表示されます。 |


#### 3.4.3 各部位ごとの周囲長ファイルの取得
**ポート説明：**

+ 身体全体の周囲長模型/各部位ごとの周囲長json/各部位ごとの周囲長模型画像ファイルを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/measure/file](http://api.vr-explorer.visbody.com/v1/measure/file)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

```plain
  {
    "code": 0,
    "data": {
      "model_url": "MODEL_URL",
      "json_url": "JSON_URL",
	    "pic_measure_url": "PIC_MEASURE_URL",
      "expires_in": 7200
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| model_url | string | 身体測定模型ファイルパス、ファイル形式＝.obj |
| json_url | string | 各部位ごとの周囲長、ファイルパス、ファイル形式＝.json　模型ファイルが表示する各部位ごとの周囲長の位置及びデータと結合可能 |
| pic_measure_url | string | 各部位ごとの周囲長模型画像のファイルパス、ファイル形式＝.jpg |
| expires_in | int | ファイルパス有効時間 |


#### 3.4.4 各部位ごとの周囲長データの取得
**ポート説明：**

+ 測定した各部位ごとの周囲長データを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/measure/girth](http://api.vr-explorer.visbody.com/v1/measure/girth)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
     "bust_girth": 0,
      "waist_girth": 0,
      "hip_girth": 0,
      "left_upperArm_girth": 0,
      "right_upperArm_girth": 0,
      "left_thigh_girth": 0,
      "right_thigh_girth": 0,
      "left_calf_girth": 0,
      "right_calf_girth": 0,
      "height": 0,
      "neck_girth": 0,
      "left_min_thigh_girth": 0,
      "right_min_thigh_girth": 0,
      "mid_waist_girth": 0
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| --- | :--- | --- |
| neck_girth | double | <font style="color:rgb(38, 38, 38);">頸部周囲長</font>(cm/in) |
| left_upper_arm_girth | double | <font style="color:rgb(38, 38, 38);">左上腕周囲</font>(cm/in) |
| right_upper_arm_girth | double | <font style="color:rgb(38, 38, 38);">右上腕部周囲</font>(cm/in) |
| bust_girth | double | <font style="color:rgb(38, 38, 38);">バスト</font>(cm/in) |
| waist_girth | double | <font style="color:rgb(38, 38, 38);">ハイウエスト</font>(cm/in) |
| mid_waist_girth | double | <font style="color:rgb(38, 38, 38);">ミッドウエスト</font>(cm/in) |
| hip_girth | double | <font style="color:rgb(38, 38, 38);">ヒップ</font>(cm/in) |
| left_thigh_girth | double | <font style="color:rgb(38, 38, 38);">左大腿部</font>(cm/in) |
| left_min_thigh_girth | double | <font style="color:rgb(38, 38, 38);">左大腿部最小外周</font>(cm/in) |
| right_thigh_girth | double | <font style="color:rgb(38, 38, 38);">右大腿部周囲</font>(cm/in) |
| right_min_thigh_girth | double | <font style="color:rgb(38, 38, 38);">右大腿部最小外周</font>(cm/in) |
| left_calf_girth | double | <font style="color:rgb(38, 38, 38);">左足ふくらはぎ周長</font>(cm/in) |
| right_calf_girth | double | <font style="color:rgb(38, 38, 38);">右足ふくらはぎ周囲</font>(cm/in) |
| height | double | 入力身長(cm/in) |


## 3.5 肩部測定データおよび結論の取得
**ポート説明：**

+ 肩部測定データおよび結論を取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/shoulder/data](http://api.vr-explorer.visbody.com/v1/shoulder/data)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


**リターン例**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
		"left_abuction": {
			"val": 25.5,
			"conclusion": "制限",
			"limit": "[150.0°~180.0°]"
		},
		"right_abuction": {
			"val": 25.5,
			"conclusion": "制限",
			"limit": "[150.0°~180.0°]"
		},
		"left_antexion": {
			"val": 45.5,
			"conclusion": "過大",
			"limit": "[120.0°~180.0°]"
		},
		"right_antexion": {
			"val": "--",
			"conclusion": "--",
			"limit": "--"
		},
		"conclusions": [
			{
				"title": "肩部関節活動範囲制限",
				"analysis": "肩関節の活動制限を受けています。多くは筋肉の緊張、鎖骨肩甲骨の活動度合いの不足で、頭/首/肩甲骨が中立状態にないことなどが原因です。正常な運動スタイル（スポーツ傷害につながる）だけでなく、五十肩、猫背、頚椎の痛みなど関連する病理学的問題を引き起こす可能性があります。また、長期間放置すると様々な肩関節の疾病を発生させてしまう恐れもあります。",
				"advice": "具体的な原因を専門家に見てもらい、さらなるスクリーニング検査と治療を受けてください。"
			},
			{
				"title": "肩部関節活動範囲が大きすぎます",
				"analysis": 肩関節の活動範囲が大きすぎます。多くは靭帯弛緩によるものが多いのですが（女性に多い）、肩の柔軟性を高めるトレーニングを定期的に行うことでも発生します。",
				"advice": "具体的な原因を専門家に見てもらい、さらなるスクリーニング検査と治療を受けてください。"
			}
		]
		
    }
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| left_abuction | object | 外転上げ－左手 |
| right_abuction | object | 外転上げ－右手 |
| left_antexion | object | 前屈上げ－左手 |
| right_antexion | object | 前屈上げ－右手 |
| val | double | 測定値（°）　本項目に失敗した場合は"--"で表示されます。 |
| limit | string | 正常範囲　本項目に失敗した場合は"--"で表示されます。 |
| conclusion | string | 評価結論　本項目に失敗した場合は"--"で表示されます。 |
| conclusions | array | 今回の測定の全ての結論は、測定状況に基づき一つのみもしくは複数の結論が表示される状況があります。 |
| title | string | 結論表題 |
| analysis | string | 結論分析　すべての項目が正常範囲であればスペースの文字ストリングが返されます。 |
| advice | string | 結論アドバイス　すべての項目が正常範囲であればスペースの文字ストリングが返されます。 |


## 3.6 プリントレポートの取得
**ポート説明：**

+ プリントレポートファイルのポートを取得するのに用います。

**リクエストURL：**

+ [http://api.explorer.visbody.com/v1/report](http://api.vr-explorer.visbody.com/v1/reprot)

**リクエスト方法：**

+ `GET`

**パラメータ：**

| パラメータ名 | 選択必須 | 類型 | 説明 |
| :--- | :--- | :--- | :--- |
| token | YES | string | ポート認証 |
| scan_id | YES | string | 読み取りID |


パラメータを含むリクエストヘッダーの

レポートの言語は

$headers[]  =  "Language: $vfLanguage; // vfLanguage 転送可能な言語は en-US(アメリカ英語) / ja-JP(日本語) / zh-CN(中国語)

リターン例

```plain
  {
    "code": 0,
    "data": "http://rexp-dev.visbody.com/reptfile/report/vr-exp/pdf/en-US/35042104086001-d0e23a52-0095-11ec-9e7e-300ed55249b1.pdf?_upt=9baa05fa1629365732.003"
  }
```

**リターンパラメータの説明**

| パラメータ名 | 類型 | 説明 |
| :--- | :--- | :--- |
| data | string | ファイルのダウンロードリンク |


## 3.7 Visbodyリターンステータスコードの説明
Visbodyのポート応答はHTTPのステータスコードおよびトラフィックステータスコードを介します。トラフィックステータスコードはresponse body内のタグになります。

| HTTPステータスコード | トラフィックステータスコード | 備考 |
| :--- | :--- | :--- |
| 500 | -1 | システムビジー |
| 200 | 0 | リクエスト完了 |
| 400 | 40001 | 無効なリクエストパス |
| 400 | 40002 | 無効なvfid/secret |
| 400 | 40003 | 無効なtoken |
| 400 | 40004 | パラメータエラー |
| 400 | 40005 | デバイス情報の未紐付け |
| 400 | 40006 | 無効な読み取りID（読み取りIDは当該アカウントもしくは今回の読み取り失敗、読み取りデータの期限切れに属しているわけではありません） |
| 400 | 40008 | ポート権限なし |
| 400 | 40009 | 読み取りID紐付け済み |


## 3.8 プッシュメッセージタイプとポート関連の説明
**説明：**

+ 第三者サービスプロバイダーがAPIポート用の3.1.3ポートを設定した後、、Visbodyユーザーの各測定結果は、当該ポートを介して関連した測定情報を第三者サービスプロバイダーにプッシュし、第三者サービスプロバイダーは測定項目の結合結果statusに基づき対応するVisbodyデータ結果ポートにアクセスします。

**測定項目と結合プッシュタイプの関係は下記のとおりです。**

| ユーザー測定項目 | 結合プッシュ項目 |
| :--- | :--- |
| 身体全体の周囲長 | girth_status |
| 姿勢 | eval_status |
| 体組成 | bia_status |
| 頸肩部 | eval_shoulder_status |
| レポートプリントアウト | pdf_status |


**結合プッシュタイプとポートの関係は下記のとおりです。**

| 結合プッシュ項目 | 説明 | アクセス可能ポート |
| :--- | :--- | :--- |
| girth_status | 身体全体の周囲長 | 3.4.3、3.4.4 |
| eval_status | 姿勢 | 3.3.3、3.4.1、3.4.2 |
| bia_status | 体組成 | 3.3.1、3.3.2、3.3.3、3.3.4 |
| eval_shoulder_status | 頸肩部測定 | 3.5 |
| pdf_status | レポートプリントアウト | 3.6 |


### 3.8.1 ポートに関する追加説明：
プリントアウトレポートのポートはレポートを取得する際測定項目の中に少なくとも1つの項目が結合に成功していなければなりません。

**その他：**

+ 結合の失敗もしくは項目測定の未実施の際、アクセスポートはステータスコード 40006を返してきます。

## 3.9 <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">レポートの履歴を取得</font>


**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">インターフェースの説明</font>****：**



+ <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">レポート履歴の取得</font>



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">リクエストURL</font>****：**



+ `http://api.explorer.visbody.com/v1/history/data`



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">リクエストメソッド：</font>**



+ GET



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">パラメータ</font>****：**

| <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">パラメータ名</font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">必須</font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">タイプ</font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">説明</font> |
| :--- | :--- | :--- | --- |
| token | 是 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">API 資格情報</font> |
| page | 是 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">ページ番号</font> |
| size | 是 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">ページ番号</font> |
| device_id | 是 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">デバイスID</font> |
| phone_number | 否 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">でんわばんごう</font> |
| start_data | 否 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">開始時間</font> |
| end_data | 否 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">終了時間</font> |




**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">レスポンスの例</font>**



```plain
  {
  "code": 0,
  "data": {
    "scanIdArray": [
      "34041912080013-fe14541f-bd22-11eb-a249-300ed5517747",
      "34041912080013-034af912-bd22-11eb-a249-300ed5517747",
      "34041912080013-312ed677-bd1e-11eb-beeb-300ed5517747",
      "34041912080013-cadff1c4-bd1d-11eb-beeb-300ed5517747",
      "34041912080013-7e22e106-bd1d-11eb-beeb-300ed5517747",
      "34041912080013-804c181e-bd0e-11eb-beeb-300ed5517747",
      "34041912080013-338d2b02-bd0e-11eb-beeb-300ed5517747",
      "34041912080013-cea3e9a6-bd0c-11eb-beeb-300ed5517747",
      "34041912080013-8a634611-bd0c-11eb-beeb-300ed5517747",
      "34041912080013-737b5c04-bd09-11eb-beeb-300ed5517747"
    ],
    "count": 40
  }
}
```



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">レスポンスパラメータの説明</font>**

| <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">パラメータ名</font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">タイプ</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">説明</font><font style="color:rgb(38, 38, 38);">   </font> |
| --- | --- | --- |
| <font style="color:rgb(38, 38, 38);">scanIdArray</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(38, 38, 38);">arrat</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">ユーザー履歴データを返す</font><font style="color:rgb(38, 38, 38);">   </font> |
| <font style="color:rgb(38, 38, 38);">count</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(38, 38, 38);">number</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">データの総数</font><font style="color:rgb(38, 38, 38);">   </font> |


<font style="color:rgb(38, 38, 38);">  
</font>

<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">  
</font>


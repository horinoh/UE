# Cesium

## 準備
### プラグイン
- 以下のいずれか
    - プロジェクト以下に Plugins フォルダを作成し、[Cesium for Unreal](https://github.com/CesiumGS/cesium-unreal/releases) 最新(zip)をダウンロード、解凍して配置
    - ~~プロジェクト以下に Plugins フォルダを作成し`、[cesium-unreal](https://github.com/CesiumGS/cesium-unreal.git) をサブモジュールとして追加~~
- Visual Studio ソリューションを生成してビルド
#### プラグイン内のコンテンツを使用する場合
- コンテンツドロワー - 右上の設定 - プラグインコンテンツを表示にチェックを入れておく
#### プラグイン内のソースコードを使用する場合
- XXXBuild.cs へ "CesiumRuntime" を追加する
    ~~~
    PublicDependencyModuleNames.AddRange(new string[] { "CesiumRuntime" });
    ~~~

### Cesium ion
- [Cesium ion](https://cesium.com/ion/signup) アカウントを作っておく

### 接続
- Cesium パネル - Connect to Cesium ion ボタン - 上記で作成したアカウントでログインして接続
- Cesium ion アカウント用のトークンを生成
    - Cesium パネル - Token ボタン - Create New Project Default Token ボタンでトークンを生成

## Cesium パネル
### スカイ
- Quick Add Basic Actors - Cesium Sun Sky を追加
    - (白飛びするので) 編集 - プロジェクト設定 - エンジン - レンダリング - Default Settings - 自動露出設定のデフォルトの輝度範囲を拡張 にチェックが入っているのを確認
    - アウトライナーに [CesiumSunSky](#) が追加される

### 地形
- Quick Add Cesium ion Assets - Cesium World Terrain + Bing Maps Aerial imagery を追加
    - アウトライナーに以下が追加される
        - [Cesium World Terrain](#Cesium-World-Terrain)            
        - [CesiumCameraManager](#)
        - [CesiumCreditSystemBP](#)
        - [CesiumGeoreference](#Cesium-Georeference)

### 建物
- Quick Add Cesium ion Assets - Cesium OSM Bildings を追加

### フォトグラメトリー
- Cesium ion Assets パネル左上の Add(+) アイコン から追加
- フォトグラメトリーを選択 - Add to Level
    - 選択肢に出てこない場合は以下のようにして予めマイアセットへ追加しておく
    - [アセットデポ](https://cesium.com/ion/assetdepot) で検索して - Add to my aseet
        - [メルボルン](https://ion.cesium.com/assetdepot/69380)、[ボストン](https://ion.cesium.com/assetdepot/354759)、[デンバー](https://ion.cesium.com/assetdepot/354307)、 等
- アウトライナーに Photogrammetry が追加される
- フォトグラメトリー領域が、地形や建物とオーバーラップするのは、Zをずらすか、[くり抜く](https://cesium.com/learn/unreal/unreal-clipping/)必要がある

## ワールドコンポジション
- 空のオープンワールド でマップを作成
- コンテンツ以下へフォルダ (ここでは World) を作成し以下へマップ (ここでは WorldMap) を保存
- 右上の設定 - ワールドセッティング
    - ワールド - ワールドコンポジションの有効化 にチェック
    - ワールド - 詳細設定 - ワールドの境界チェックを有効化 のチェックを外す
- レイヤー
    - ウインドウ - レベル - ワールドコンポジションを呼び出す ボタンを押す
    - 「+」 - Streaming距離のチェックを外し - レイヤー名を付けて (ここでは CesiumLayer) - 作成
- パーシスタントレベル
    - Cesium Sun Sky, Cesium World Terrain + Bing Maps Aerial imagery を追加
    - Photogrammetry をサブレベルで使用する分だけ追加 (ここでは以下を追加)
        - [メルボルン](https://ion.cesium.com/assetdepot/69380)、[ボストン](https://ion.cesium.com/assetdepot/354759)、[デンバー](https://ion.cesium.com/assetdepot/354307)
### サブレベル
- レベルパネル - レベル - 新規作成
    - ダブルクリックして選択された状態にする
    - 右クリック - レイヤに割り当てる - (上記で作成した) CesiumLayer
- サブレベルが選択された状態にする
    - ワールドアウトライナー - 対象の Photogrammetry を選択 (その地点がビューポートに表示される)
    - ワールドアウトライナー - CesiumGeoreference - Place Georeference Origin Here
- 以上を追加するサブレベルの分だけ繰り返す

## TIPS
- カメラ速度
    - ビューポート右上で速度を増やす
- UE ワールドを中心へ持ってくる
    - アウトライナー - CesiumGeoreference - Place Georeference Origin Here ボタン
- 暗い場合
    - アウトライナー - CesiumSunSky - Solar Time を調整する

## <a name="Cesium-World-Terrain">Cesium World Terrain</a>
- Cesium3DTileset アクタ
- UE へ 3D タイルデータをストリーミングする

## <a name="Cesium-Georeference">CesiumGeoreference</a>
- シーンを世界のどこにするか指定する 
    ||イリノイ|メルボルン|デンバー|ボストン|
    |-|-|-|-|-|
    |Origin Latitude|41.878101|-105.25737|39.743454|42.363565|
    |Origin Longitude|-87.59201|39.736401|-104.988761|-71.072255|
    |Origin Height|1000.0|2250.0|1798.733479|-23.775415|



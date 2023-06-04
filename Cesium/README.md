# Cesium

## 準備
#### プラグイン
- [Cesium for Unreal](https://github.com/CesiumGS/cesium-unreal/releases) 最新をダウンロードしておく
- プロジェクト以下に Plugins フォルダを作成し、 解凍したものを配置する
- Visual Studio ソリューションを生成してビルド

### Cesium ion
- [Cesium ion](https://cesium.com/ion/signup) アカウントを作っておく

### 接続
- Cesium パネル - Connect to Cesium ion ボタン - 上記で作成したアカウントでログインして接続
- Cesium ion アカウント用のトークンを生成
    - Cesium パネル - Token ボタン - Create New Project Default Token ボタンでトークンを生成

## Cesium パネル
### スカイ
- Quick Add Basic Actors - Cesium Sun Sky を追加
    - (白飛びするので) 編集 - プロジェクト設定 - Extend default luminance range in Auto Exposure settings にチェックにチェックを入れる
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

## <a name="Cesium-World-Terrain">Cesium World Terrain</a>
- Cesium3DTileset アクタ
- UE へ 3D タイルデータをストリーミングする

## <a name="Cesium-Georeference">CesiumGeoreference</a>
- シーンを世界のどこにするか指定する
- シカゴ、イリノイの場合は以下のようになる
    |||
    |-|-|
    |Origin Latitude|41.878101|
    |Origin Longitude|-87.59201|
    |Origin Height|1000.0|

## Cesium ion Asset の追加
- 左上の Add(+) アイコン から追加
- 






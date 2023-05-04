# UE

## .uproject 右クリックメニューが出ない場合
* UnrealVersionSelector を起動し、エンジンのインストール先を登録する
    * <エンジンインストール先>\Engine\Binaries\Win64 UnrealVersionSelector-Win64-Shipping.exe を実行
    * Register this directory as an Unreal Engine installation? と聞かれるので「はい」

## Visual Studio拡張
* UEをソースからビルドしている場合はマーケットプレイスからインストールできないので以下のいずれかをする
    * プロジェクトの Plugins/ 以下へ [Visual Studio拡張](https://github.com/microsoft/vc-ue-extensions#readme) をクローンする
    * エンジンソースの Plugins/ 以下へ [Visual Studio拡張](https://github.com/microsoft/vc-ue-extensions#readme) をクローンする

## ゲームモード
- AGameModeBase 等を継承したクラスを作成
- 編集 - プロジェクト設定 - マップ&モード - デフォルトのゲームモード に上記で作成したクラスを指定する

## [インプット](https://github.com/horinoh/UE/tree/master/Input)

## [キャラクター](https://github.com/horinoh/UE/tree/master/Character)
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

## スターターコンテンツを自動追加
- Config/DefaultGame.ini に以下のように記述する
    ~~~
    [StartupActions]
    bAddPacks=True
    InsertPack=(PackSource="StarterContent.upack",PackName="StarterContent")
    ~~~

## C++ から参照
### BP クラス
- リファレンスをコピーで得られる文字列から '' 内だけ抜き出し、さらに "_C" を付け加えたものを指定する
    ~~~
    "AAA'BBB.CCC'" -> "BBB.CCC_C"
    ~~~
    - 例
    ~~~
    static ConstructorHelpers::FClassFinder<UObject> PawnClass(TEXT("BBB.CCC_C"));
	if (PawnClass.Succeeded())
	{
		DefaultPawnClass = PawnClass.Class;
	}
    ~~~
### アニメーションBP
- リファレンスをコピーで得られる文字列から '' 内だけ抜き出し、さらに . 以降を削除したものを指定する
    ~~~
    "AAA'BBB.CCC'" -> "BBB"
    ~~~
    - 例
    ~~~
    static ConstructorHelpers::FClassFinder<UObject> AnimBPClass(TEXT("BBB"));
	if (AnimBPClass.Succeeded())
	{
		SkelMeshComp->SetAnimInstanceClass(AnimBPClass.Class);
	}
    ~~~

## [インプット](https://github.com/horinoh/UE/tree/master/Input)

## [キャラクター](https://github.com/horinoh/UE/tree/master/Character)

## [Scesium](https://github.com/horinoh/UE/tree/master/Scecium)
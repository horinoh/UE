# インプット

## モジュール追加
* XXX.Build.cs に以下のように "EnhancedImput" を追加する必要がある
    ~~~
    PublicDependencyModuleNames.AddRange(new string[] { ..., "EnhancedInput" });
    ~~~

## 種類
* アクション
    * 例) IA_Move, IA_Jump, IA_Crouch, ....
* マッピングコンテキスト
    * アクションのコレクション
    * 例) プレイヤーが車に乗り込んだら専用のコンテキストを追加、降りたら取り除く
* モディファイア
    * UInputModifier を継承し、ModifyRaw_Implementation をオーバライドする
    * 例) Y軸反転、デッドゾーン、...
* トリガー




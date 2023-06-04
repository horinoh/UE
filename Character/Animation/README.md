# アニメーション

## アニメーションBP

### AnimInstance
- C++
    - UAnimInstance を継承して作成
    - アニメーションBP 内で条件式を書かなくて済むように C++ に書いておく
- スケルトンを指定、C++ で‘‘作成した AnimInstance 継承クラスを使用して作成する

### ステートマシン
- 「出力ポーズ」からノードをドラッグ - アニメーション - ステートマシンを作成
    ![画像](StateMachine0.png)
    ![画像](StateMachine1.png)
    - ここでは Locomotion という名前で作成

    ![画像](StateMachine2.png)
    - Stand
        - 下記「ブレンドスペース」で作成する BS_Stand を指定し、Speed, Direction 変数を繋ぐ
    - Crouch
        - 下記「ブレンドスペース」で作成する BS_Crouch を指定し、Speed, Direction 変数を繋ぐ
    - JumpStart, JumpLoop, JumpEnd
        - 下記「アニメーションシーケンス」で作成する Jump_From_Stand_Start, Jump_From_Stand_Loop, Jump_From_Stand_End を指定する
    - Sprint
        - Sprint_Fwd_Rifle を指定する
        - 詳細 - Loop Animation にチェック
    - ここでは 条件文は C++ で書いてあるので、ステートを繋ぐだけで良い
- TODO エイリアス、コンジットを試す

### 上半身ブレンド
- AnimGraph 内
    - 「新規保存のキャッシュされたポーズ」を作成
        - ここでは SavedPoseLocomotion という名前にした、入力に Locomotion の出力を接続
    - 「ボーン毎のレイヤードブレンド」を作成
        - インスペクタ - コンフィグ - Layer Setup - インデックス[0] - Branch Filters を増やす
        - インデックス[0] - Bone Name に "spine_01" を指定
        - 「キャッシュされたポーズを使用」(SavedPoseLocomotion)を作成、「ボーン毎のレイヤードブレンド」の Base Pose へ接続
        - 「スロット 'Default Slot'」を作成
            - 出力を 「ボーン毎のレイヤードブレンド」の Blend Poses0 へ接続
            - 「キャッシュされたポーズを使用」(SavedPoseLocomotion)を作成、「スロット 'Default Slot'」の入力へ接続
            - Slot Name を UpperBody へ変更 (選択肢 UpperBody スロットは下記「アニメーションモンタージュで」追加)
        ![画像](UpperBody.png)
### エイムオフセット
- AnimGraph 内
    - 「新規保存のキャッシュされたポーズ」を作成
        - ここでは SavedPoseUpperBody という名前にした、入力に(↑で作成した)「ボーン毎のレイヤードブレンド」の出力を接続
    - 「ブレンドPoses by bool」を作成
        - 下記「アニメーションオフセット」で作成する AO_Ironsights, AO_Hip をドラッグ & ドロップ
            - それぞれ Yaw, Pich 変数を接続
            - 「キャッシュされたポーズを使用」(SavedPoseUpperBody) を2つ作成、それぞれの BasePose へ接続
        - 入力 True Pose に AO_Ironsights の出力を接続
        - 入力 False Pose に AO_Hip の出力を接続
        - 入力 Active Value に IsTargeting を接続
## ブレンドスペース
- BS_Stand を作成
    - 軸 (X, Y) = (Direction [-180, 180], Speed [0, 600]) とする
    - 以下のようにアサインする
        |Sp\Dir|-180|90|0|90|180|
        |-|-|-|-|-|-|
        |600|Jog_Bwd_Rifle|Jog_Lt_Rifle|Jog_Fwd_Rifle|Jog_Rt_Rifle|Jog_Bwd_Rifle|
        |0|Idle_Rifle_Hip|Idle_Rifle_Hip|Idle_Rifle_Hip|Idle_Rifle_Hip|Idle_Rifle_Hip|

        ![画像](BlendSpace.png)

- BS_Crouch を作成
   - 軸 (X, Y) = (Direction [-180, 180], Speed [0, 300]) とする
    - 以下のようにアサインする
        |Sp\Dir|-180|90|0|90|180|
        |-|-|-|-|-|-|
        |300|Crouch_Walk_Bwd_Rifle_Hip|Crouch_Walk_Lt_Rifle_Hip|Crouch_Walk_Fwd_Rifle_Hip|Crouch_Walk_Rt_Rifle_Hip|Crouch_Walk_Bwd_Rifle_Hip|
        |0|Crouch_Idle_Rifle_Hip|Crouch_Idle_Rifle_Hip|Crouch_Idle_Rifle_Hip|Crouch_Idle_Rifle_Hip|Idle_Rifle_Hip|

## アニメーションシーケンス
- ジャンプ時の Start, Loop, End を作成する
    - Jump_From_Stand、Jump_From_Stand_Ironsights, Jump_From_Jog は用意されているが、一連のアニメーションになっているので切り出す
    - アニメーションシーケンスを切り出して、Start、Loop、End を作成
        - Jump_From_Stand ... Start(～17), Loop(18), End(18～)
            ||||
            |-|-|-|
            |Jump_From_Stand_Start(-17)|Jump_From_Stand_Loop(18)|Jump_From_Stand_End(18-)|
        - Jump_From_Stand_Ironsights ... Start(～17), Loop(18), End(18～)
            ||||
            |-|-|-|
            |Jump_From_Stand_Ironsights_Start|Jump_From_Stand_Ironsights_Loop|Jump_From_Stand_Ironsights_End|
        - Jump_From_Jog ... Start(～10), Loop(11), End(11～)
            ||||
            |-|-|-|
            |Jump_From_Jog_Start|Jump_From_Jog_Loop|Jump_From_Jog_End|
## アニメーションオフセット
- 構え時のエイム9方向を作成する
    - Aim_Space_Hip, Aim_Space_Ironsights は用意されているが、一連のアニメーションになっているので以下のように切り出す(Aim_Space_Ironsights も同様)
        ||||
        |-|-|-|
        |Aim_Space_Hip_LT(40)|Aim_Space_Hip_CT(10)|Aim_Space_Hip_RT(71)|
        |Aim_Space_Hip_LC(35)|Aim_Space_Hip_CC(00)|Aim_Space_Hip_RC(65)|
        |Aim_Space_Hip_LB(50)|Aim_Space_Hip_CB(20)|Aim_Space_Hip_RB(81)|
    - 加算セッティング
        - Additive Anim Type へ Mesh Space を選択
        - Base Pose Type へ Selected animation frame を選択
        - Base Pose Animation へ Idle_Rifle_Hip(Idle_Rifle_Ironsights) を選択
        
        ![画像](AdditiveSetting.png)
        - 複数アセットを選択し、右クリック - アセットアクション - プロパティマトリックスで一斉編集 - で一括編集すると良い 
- アニメーションオフセット AO_Hip, AO_Ironsights を作成
- 軸 (X, Y) = (Yaw [-90, 90], Pitch [-90, 90]) とする

## アニメーションモンタージュ
- UpperBody スロット追加
    - 虫眼鏡 - Anim Slot Manger - Add Slot(スロット追加) - 「UpperBody」と名前を付け - 保存
    - DefaultGroup.DefaultSlot を DefaultGroup.UpperBody へ変更
- 発砲 (ライフル、ショットガン)
    - AM_Fire_Rifle を作成
        - DefaultGroup.UpperBody スロットを選択
        - Fire_Rifle_Hip, Fire_Rifle_Ironsights をドラッグ&ドロップ
        - Fire_Rifle_Ironsights の先頭に、セクション名 「Ironsights」を追加し、Clear を押す
    - AM_Fire_Shotgun も同様
- リロード (ライフル、ショットガン、ピストル)
    - AM_Reload_Rifle を作成
    - AM_Reload_Shotgun, AM_Reload_Pistol も同様
- 装備 (ライフル、ピストル)
    - AM_Equip_Rifle を作成
    - AM_Equip_Pistol も同様
- 死亡
    - AM_Death を作成




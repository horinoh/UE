# インプット

## モジュール追加
* XXX.Build.cs に以下のように "EnhancedImput" を追加する必要がある
    ~~~
    PublicDependencyModuleNames.AddRange(new string[] { ..., "EnhancedInput" });
    ~~~

## 種類
* 入力アクション
    * 例) IA_Move, IA_Look, IA_Jump, IA_Crouch, ....
        * アクション毎の Value Type を適切に設定しておく (Digita, Axis2D,... 等)
    * アクションのバインド
        ~~~
        class UInputAction;

        virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;
        UPROPERTY(EditAnywhere, Category = "Input")
	    UInputAction* IA_Move;
        ~~~
        ~~~
        #include "EnhancedInputComponent.h"
        #include "InputAction.h"

        AMyPawn::AMyPawn()
        {
	        static ConstructorHelpers::FObjectFinder<UInputAction> Move(TEXT("/Script/EnhancedInput.InputAction'/Game/Input/IA_Move.IA_Move'"));
	        if (Move.Succeeded())
	        {
		        IA_Move = Move.Object;
	        }
        }
        void AMyPawn::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
        {
            if (auto Input = Cast<UEnhancedInputComponent>(PlayerInputComponent))
	        {
		        Input->BindAction(IA_Move, ETriggerEvent::Triggered, this, &AMyPawn::OnMove);
	        }
        }
        void AMyPawn::OnMove(const FInputActionInstance& Instance)
        {
	        auto AxisValue = Instance.GetValue().Get<FVector2D>();
	        GEngine->AddOnScreenDebugMessage(0, 0.0f, FColor::White, FString::Printf(TEXT("OnMove = (%2.1f, %2.1f)"), AxisValue.X, AxisValue.Y));
        }
        ~~~
* 入力マッピングコンテキスト
    * アクションのコレクション
        * Mappings にアクションを追加していく
        * 必要に応じてモディファイアを追加する
            * 例) キーボードの場合のモディファイア
                ||||
                | - | - | - |
                | W  | Swizzle(YXZ) | (1, 0) -> (0, 1) のように変換してY軸として扱う |
                | A  | Nagate(X) | X反転 |
                | S  | Nagate(X) + Swizzle(YXZ)  | X反転 + Y軸変換 |
                | D  | | デフォルトが (1, 0) なのでそのままで良い |
            * 例) マウスの場合のモディファイア
                ||||
                | - | - | - |
                | Mouse XY 2D-Axis | Nagate(Y) | Y反転 |
            * 自前のモディファイア作成は UInputModifier を継承し、ModifyRaw_Implementation をオーバライドする
    * 例) プレイヤーが車に乗り込んだら専用のマッピングコンテキストを追加、降りたら取り除く等
    * マッピングコンテキストの追加
        ~~~
        class UInputMappingContext;

        void BeginPlay() override;
	    UPROPERTY(EditAnywhere, Category = "Input")
	    UInputMappingContext* IMC_Normal;
        ~~~
        ~~~
        AMyPlayerController::AMyPlayerController()
        {
	        static ConstructorHelpers::FObjectFinder<UInputMappingContext> IMC(TEXT("/Script/EnhancedInput.InputMappingContext'/Game/Input/IMC_Normal.IMC_Normal'"));
	        if (IMC.Succeeded())
	        {
		        IMC_Normal = IMC.Object;
	        }
        }
        void AMyPlayerController::BeginPlay()
        {
	        if (nullptr != IMC_Normal) 
            {
		        if (const auto LP = GetLocalPlayer())
		        {
			        if (auto InputSystem = LP->GetSubsystem<UEnhancedInputLocalPlayerSubsystem>())
			        {
				        InputSystem->AddMappingContext(IMC_Normal, 0);
			        }
		        }
	        }
        }
        ~~~
* 入力トリガー
    * TODO

## デバッグ
* アクションと軸マッピング表示 (再生中)
    ~~~
    $showdebug enhancedinput
    ~~~
* インジェクション入力
    ~~~
    $Input.+key A
    $Input.+key Gamepad_Left2D X=0 Y=1
    $Input.-key Gamepad_Left2D
    ~~~
    * 指定するキーの名前は InputCoreTypes.cpp に定義がある
        ~~~
        const FKey EKeys::A("W");        
        const FKey EKeys::A("A");
        const FKey EKeys::A("S");
        const FKey EKeys::A("D");
        ...
        const FKey EKeys::Gamepad_Left2D("Gamepad_Left2D");
        ...
        ~~~
        




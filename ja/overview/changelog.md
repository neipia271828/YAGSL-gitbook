# 変更履歴

## プルリクエストは常にレビューされます！

YAGSL をより良くしたい方はぜひプルリクエストを作成してください。生活の質を向上させる変更を歓迎しています。

{% embed url="https://github.com/BroncBotz3481/YAGSL-Example/pulls?q=is%3Apr+is%3Aclosed" %}

{% embed url="https://github.com/Yet-Another-Software-Suite/YAGSL/pulls?q=is%3Apr+is%3Aclosed" %}

## コントリビューション

YAGSL の開発は YAGSL リポジトリの `dev` ブランチ（`yagsl/java/swervelib`）で行われています。

{% embed url="https://github.com/BroncBotz3481/YAGSL-Example/tree/dev" %}
YAGSL-Example dev ブランチ
{% endembed %}

すべての PR はここをベースにし、ここへマージしてください。YAGSL は定期的に他のリポジトリへ反映されます。

## 2025.8.0

* ブラシ付き SparkMAX に接続されたアブソリュートエンコーダーの修正

## 2025.7.2

* [@MEisSCAMMER](https://github.com/MEisSCAMMER) による `setAbsoluteEncoderOffset()` の修正 ([#338](https://github.com/BroncBotz3481/YAGSL-Example/pull/338))
* [@Sate04](https://github.com/Sate04) による omegaPIDController の連続入力の有効化 ([#337](https://github.com/BroncBotz3481/YAGSL-Example/pull/337))
* 有効状態での起動時のクラッシュを修正

## 2025.7.1

* `SwerveInputStream.allianceRelativeControl` の修正
* 試合中にロボットコードが再起動してもクラッシュしないよう修正
* `SwerveInputStream.translationHeadingOffset` を追加
* YAGSL 起動時のジャイロゼロイングを削除。`autonomousInit` で手動でゼロイングするか `RobotModeTriggers` を使用する必要があります
* サンプルコードに `RobotModeTriggers.autonomous().onTrue(Commands.runOnce(this::zeroGyroWithAlliance));` を追加

## 2025.7.0

* [ ] 旧コンバージョンファクターメソッドが警告の代わりに例外をスローするよう変更
* [ ] TalonFXS サポートを追加
* [ ] `SwerveMotor.isAttachedAbsoluteEncoder` を `SwerveMotor.usingExternalFeedbackSensor` にリネーム
* [ ] `SwerveDrive.pushOffsetsToEncoders` を `SwerveDrive.useExternalFeedbackSensor` に非推奨化
* [ ] `SwerveDrive.restoreInternalOffset` を `SwerveDrive.useInternalFeedbackSensor` に非推奨化
* [ ] [@DanPeled](https://github.com/DanPeled) による Shuffleboard から任意のダッシュボードへの参照更新
* [ ] SparkFlex に接続されたアブソリュートエンコーダーの修正
* [ ] 接続されたアブソリュートエンコーダーはフィードバックデバイスとして自動設定されなくなりました。明示的に呼び出す必要があります
* [ ] `SwerveInputStream.driveToPose` を追加
* [ ] [@clrozeboom](https://github.com/clrozeboom) によるフィードフォワードを使うドライブ関数でのモジュール状態オプション最適化 ([#314](https://github.com/BroncBotz3481/YAGSL-Example/pull/314))
* [ ] [@kytpbs](https://github.com/kytpbs) によるすべてのクローザブルアイテムへの `AutoClosable` 実装 ([#317](https://github.com/BroncBotz3481/YAGSL-Example/pull/317))
* [ ] [@kytpbs](https://github.com/kytpbs) による不要なインポートと変数の削除 ([#318](https://github.com/BroncBotz3481/YAGSL-Example/pull/318))
* [ ] [@CoffeeCoder1](https://github.com/CoffeeCoder1) による PIDFConfigs から生成された PIDController への IZone 設定 ([#319](https://github.com/BroncBotz3481/YAGSL-Example/pull/319))
* [ ] [@fletch3555](https://github.com/fletch3555) によるタイポ修正 ([#323](https://github.com/BroncBotz3481/YAGSL-Example/pull/323))
* [ ] [@wackyvert](https://github.com/wackyvert) による DIO 経由の REV Through Bore 対応 ([#325](https://github.com/BroncBotz3481/YAGSL-Example/pull/325))
* [ ] [@rakosi2](https://github.com/rakosi2) による setMotorBrake の繰り返し実行の停止 ([#327](https://github.com/BroncBotz3481/YAGSL-Example/pull/327))
* [ ] [@MrFast-js](https://github.com/MrFast-js) による axisDeadband を超えないコントローラー入力時の回転防止 ([#328](https://github.com/BroncBotz3481/YAGSL-Example/pull/328))

## 2025.3.0

* [ ] デバッグを容易にするための `SwerveDrive.setModuleStateOptimization` を追加
* [ ] `sparkmax_analog` の修正
* [ ] [@jwt388](https://github.com/jwt388) によるビジョン結果がない場合のヌルポインタクラッシュ修正 ([#307](https://github.com/BroncBotz3481/YAGSL-Example/pull/307))
* [ ] [@konnorreynolds](https://github.com/konnorreynolds) による PigeonViaTalonSRX と接続 SparkFlex の追加 ([#292](https://github.com/BroncBotz3481/YAGSL-Example/pull/292))
* [ ] [@wackyvert](https://github.com/wackyvert) によるモジュール設定が 0 に誤設定された場合のエラースロー ([#308](https://github.com/BroncBotz3481/YAGSL-Example/pull/308))
* [ ] [@konnorreynolds](https://github.com/konnorreynolds) による rawAbsoluteEncoder 位置の修正 ([#309](https://github.com/BroncBotz3481/YAGSL-Example/pull/309))

## 2025.2.2

* [ ] `swerve/measuredChassisSpeeds` が vx と vy の両方を報告するよう修正
* [ ] Team 151 による `SwerveInputStream.robotRelative` の修正
* [ ] WispySparks (Team 2508) が発見した `SwerveDrive.drive` の `fieldOriented` パラメーター適用バグを修正

## 2025.2.1

* [ ] 相対エンコーダーを使った最適化の修正
* [ ] SparkMax の `SparkMaxSwerve.isAbsoluteEncoderAttached()` でのアブソリュートエンコーダー検出修正（Team 217 発見）
* [ ] SparkMax の `absoluteEncoder` を `Optional` に変更（Team 217 による）
* [ ] 自転しながら SysId を行う方法を追加、`SwerveDriveTest.setDriveSysIdRoutine(new Config(),this, swerveDrive, 12, true)` にパラメーターを変更

## 2025.2.0

* [ ] デフォルトでドライブモーターフィードフォワードの kA を無効化
* [ ] [@jwt388](https://github.com/jwt388) によるフィードフォワードで使用される getMaxVelocity の修正 ([#286](https://github.com/BroncBotz3481/YAGSL-Example/pull/286))
* [ ] [@clrozeboom](https://github.com/clrozeboom) による最大速度設定でフル速度未満に制限できない問題の修正 ([#277](https://github.com/BroncBotz3481/YAGSL-Example/pull/277))
* [ ] [@catr1xLiu](https://github.com/catr1xLiu) によるシムモジュール SysId ルーティン & 新しい maple-sim バージョン ([#288](https://github.com/BroncBotz3481/YAGSL-Example/pull/288))

## 2025.1.3

* [ ] `Adjusted IMU Yaw` の修正と `SmartDashboard` への公開
* [ ] シミュレーション速度と読み取り値をモジュール出力に追加
* [ ] スタンドアロン TalonSRX に接続された SRX Mag エンコーダー用に `srxmag_standalone` アブソリュートエンコーダータイプを追加

## 2025.1.2

* [ ] WPILib 2025.1.1 に更新
* [ ] CTRE/REV/Studica/Redux の vendordeps を更新
* [ ] javadocs の一部の問題を修正
* [ ] [@yapplejack](https://github.com/yapplejack) によるコメントの整理 ([#278](https://github.com/BroncBotz3481/YAGSL-Example/pull/278))

## 2025.1.1

* [ ] テレメトリ公開の問題を修正。テレメトリは `SmartDashboard` ネットワークテーブル下に公開されるようになりました
* [ ] ロボットが有効中に SparkMAX または SparkFlex の設定変更が行われた場合にエラーをスロー
* [ ] ドライブモーターのアイドルモードを設定しないようサンプルを変更
* [ ] ThriftyNova サポートを追加
* [ ] `SwerveInputStream.robotRelative` と `SwerveInputStream.allianceRelativeControl` を追加

## 2025.1.0.1

* [ ] SparkMAX および SparkFlex の設定冗長性を追加
* [ ] [@jwt388](https://github.com/jwt388) による driveWithSetpointGenerator のループ時間修正とフィールド指向制御の使用 ([#271](https://github.com/BroncBotz3481/YAGSL-Example/pull/271))
* [ ] テレメトリにサイクルタイムを追加
* [ ] `SwerveDriveTelemetry.updateSettings` が `true` の場合のみ設定を送信するようテレメトリを最適化
* [ ] SparkMAX および SparkFlex のリトライ遅延を 10ms から 5ms に短縮
* [ ] オンザフライ設定遅延を 100ms から 10ms に短縮
* [ ] init 後のオンザフライ設定遅延に対する警告を追加
* [ ] `SimpleMotorFeedForward.calculate` を速度のみ使用するよう修正（残念ながら加速度は無視）
* [ ] `SmartDashboard.put` を NT4 Publishers に変更
* [ ] `IMUVelocity` を削除し、ライブラリの速度フェッチを使用
* [ ] `SwerveIMU.getRate()` を `SwerveIMU.getYawAngularVelocity()` にリネームし、`AngularVelocity` オブジェクトを返すよう変更
* [ ] 簡単なコントローラー変換のために `SwerveInputStream` オブジェクトを追加

## 2025.1.0

* [ ] ビジョンファイルのヌルポインタ例外を修正
* [ ] [@catr1xLiu](https://github.com/catr1xLiu) による Maple-Sim 0.2.4 への更新と `SwerveDrive.getMapleSimDrive()` の追加 ([#262](https://github.com/BroncBotz3481/YAGSL-Example/pull/262))
* [ ] Spark Max ブラシモーターコントローラーエンコーダーのヌルポインタ例外を修正
* [ ] `SwerveMath.scaleTranslation` の問題を修正
* [ ] @jwt388 (FRC Team 151) による `resultLists` チェックを使った `Vision` 更新の修正
* [ ] `SwerveDrive.getMaximumVelocity()` を `SwerveDrive.getMaximumChassisVelocity()` に変更
* [ ] `SwerveDrive.getMaximumAngularVelocity()` を `SwerveDrive.getMaximumChassisAngularVelocity()` に変更
* [ ] 既知のモータータイプを使ってドライブモーターフィードフォワードを計算
* [ ] モーターコントローラーに接続されたモータータイプを認識するようパーサーを拡張（ブラシモーター除く）
* [ ] シャシー最大速度とモジュール最大速度を分離
* [ ] `navx_mxp_serial` を再追加
* [ ] モーター指定子 `krakenx60foc`、`krakenx60`、`falcon500foc`、`falcon500`、`sparkmax_neo550`、`sparkmax_neo`、`sparkflex_neo`、`sparkflex_vortex`、`sparkflex_neo550` を追加
* [ ] `SwerveModule` コンストラクターでドライブモーターフィードフォワードを作成
* [ ] `SwerveModule.maxDriveVelocity` と `SwerveModule.maxAngularVelocity` にモジュール最大速度を追加
* [ ] Web 設定を更新

## 2025.0.0

* [ ] [@clrozeboom](https://github.com/clrozeboom) によるコンストラクタースコープ終了後に AHRS imu 変数が失われる問題の修正 ([#254](https://github.com/BroncBotz3481/YAGSL-Example/pull/254))
* [ ] [@catr1xLiu](https://github.com/catr1xLiu) による Maple-Sim の追加 ([#251](https://github.com/BroncBotz3481/YAGSL-Example/pull/251))
* [ ] [@Etaash-mathamsetty](https://github.com/Etaash-mathamsetty) による kA サポートの実装 ([#258](https://github.com/BroncBotz3481/YAGSL-Example/pull/258))
* [ ] [@thenetworkgrinch](https://github.com/thenetworkgrinch) による WPILib 2025 Beta-2 への更新 ([#257](https://github.com/BroncBotz3481/YAGSL-Example/pull/257))
* [ ] [@WispySparks](https://github.com/WispySparks) による Talon SRX での統合アブソリュートエンコーダーサポート追加 ([#208](https://github.com/BroncBotz3481/YAGSL-Example/pull/208))
* [ ] [@catr1xLiu](https://github.com/catr1xLiu) による DCMotor モデリングを使った PathPlanner DriveFeedForward の実装 ([#260](https://github.com/BroncBotz3481/YAGSL-Example/pull/260))
* [ ] NavX 反転の修正
* [ ] Team 457 Grease Monkeys の協力による MAXSwerve のアブソリュートエンコーダー問題の修正

## 2024.7.0 - 2024 WPILib 最終バージョン

* [ ] SparkMAX および SparkFlex の反転冗長チェックを追加
* [ ] [TechnologyMan00](https://github.com/Technologyman00) によるパスファインディングのウォームアップ追加 ([#241](https://github.com/BroncBotz3481/YAGSL-Example/pull/241))

## 2024.6.1.0

* [ ] `Canandgyro` サポートを追加
* [ ] [catr1xLiu](https://github.com/catr1xLiu) によるサンプルスワーブサブシステムのスピーカー照準コマンドの微修正 ([#239](https://github.com/BroncBotz3481/YAGSL-Example/pull/239))

## 2024.6.0.0

* [ ] [clrozeboom](https://github.com/clrozeboom) によるスワーブ設定テスト変更のマージ ([#228](https://github.com/BroncBotz3481/YAGSL-Example/pull/228))
* [ ] [yapplejack](https://github.com/yapplejack) による角速度補正（YAGSL を大幅に改善する重要なアップデート！）([#231](https://github.com/BroncBotz3481/YAGSL-Example/pull/231))
* [ ] [yapplejack](https://github.com/yapplejack) による SparkMax 最適化、平均フィルターの変更など ([#233](https://github.com/BroncBotz3481/YAGSL-Example/pull/233))
* [ ] `sparkmax_analog5v` を有効なアブソリュートエンコーダータイプとして追加
* [ ] [yapplejack](https://github.com/yapplejack) による desiredChassisSpeeds を使った desaturateWheelSpeeds() の提案 ([#232](https://github.com/BroncBotz3481/YAGSL-Example/pull/232))
* [ ] `SwerveDrive.setModuleEncoderAutoSynchronize` による自動同期のオプション設定化
* [ ] `TalonFXSwerve` のコンフィギュレーター問題を修正

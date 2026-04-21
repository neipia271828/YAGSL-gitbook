# 主な機能

## ドキュメント

{% embed url="https://yet-another-software-suite.github.io/YAGSL/javadocs/" %}
YAGSL Javadocs
{% endembed %}

{% embed url="https://yet-another-software-suite.github.io/YAGSL/config_generator/" %}
設定ファイル生成ツール
{% endembed %}

{% embed url="https://github.com/Yet-Another-Software-Suite/YAGSL" %}
サンプルコードリポジトリ
{% endembed %}

## インストール

WPILib オンラインから以下のリストに記載されているすべての vendordep をダウンロードするだけです: [#vendor-urls](../../../configuring-yagsl/dependency-installation.md#vendor-urls "mention")。手順はこちらを参照してください：

{% embed url="https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#rd-party-libraries" %}

## ハードウェアサポート

### モーター

* [Neo](https://www.revrobotics.com/rev-21-1650/)
* [Falcon 500](https://store.ctr-electronics.com/falcon-500-powered-by-talon-fx/) / [Kraken X60](https://wcproducts.com/products/kraken)
* [SparkMAX で制御されるブラシモーター](https://www.andymark.com/products/rs775-5-motor-with-encoder-for-pg71-pg188-gearbox)（[エンコーダー](https://www.mouser.com/ProductDetail/Grayhill/63R256)[付き](https://store.ctr-electronics.com/srx-mag-encoder/)）。角度モーターはクアドラチャエンコーダーを必要とせず、SparkMAX のデータポートに接続されたデューティサイクルエンコーダーを使用する必要があります。

### 絶対エンコーダー

* [Thrifty Absolute Encoders](https://www.thethriftybot.com/bearings/Thrifty-Absolute-Magnetic-Encoder-p421607500)
* [CANCoder](https://store.ctr-electronics.com/cancoder/)
* [REV Through Bore](https://www.revrobotics.com/rev-11-1271/)
* [Canandcoder（SparkMAX 経由）](https://docs.reduxrobotics.com/canandcoder/spark-max#using-the-pwm-output-with-spark-max)
* [Canandcoder（CAN 経由）](https://docs.reduxrobotics.com/canandcoder/getting-started)
* [Throughbore](https://www.revrobotics.com/rev-11-1271/)（PWM 経由）
* [Thrifty Absolute Magnetic Encoder](https://www.thethriftybot.com/products/thrifty-absolute-magnetic-encoder)（AnalogInput 経由）
* [MA3](https://www.andymark.com/products/ma3-absolute-encoder-with-cable)
* [SRX Mag](https://store.ctr-electronics.com/srx-mag-encoder/)
* [AM Mag](https://www.andymark.com/products/am-mag-encoder)
* PWM 対応の絶対エンコーダー全般
* アナログ対応の絶対エンコーダー全般

### IMU（ジャイロスコープ）

#### **すべてのジャイロスコープは反時計回りを正方向とすること！**

* [Pigeon IMU](https://store.ctr-electronics.com/pigeon-2/)
* [Pigeon 2 IMU](https://store.ctr-electronics.com/pigeon-2/)
* [NavX](https://www.studica.com/nav2-mxp-robotics-navigation-sensor)
* [NavX 2](https://www.studica.com/nav2-mxp-robotics-navigation-sensor)
* [NavX Micro](https://www.studica.com/navx-2-micro-9-axis-inertialmagnetic-sensor)
* [NavX 2 Micro](https://www.studica.com/navx-2-micro-9-axis-inertialmagnetic-sensor)
* [ADIS16448](https://wiki.analog.com/first/adis16448\_imu\_frc)
* [ADIS16470](https://wiki.analog.com/first/adis16470\_imu\_frc)
* アナログジャイロスコープ全般

## シミュレーション

* `YAGSL/examples` に含まれるサンプルで、フルシミュレーションをすぐに利用できます。

## 制御

* PathPlanner がサポートされており、`YAGSL/examples` にサンプルがあります。
* 制御は、目標角度と現在角度の差分を用いる方式（`SwerveController.getTargetSpeeds`）と、正しい単位で直接 `SwerveDrive` に渡す方式の両方に対応しています。
* `SwerveDrive.lockPose` 関数はすべてのホイールを内向きに向け、ロボットをほぼ動かせない状態にします。
* ドライブモーターのアイドルモード（コーストまたはブレーキ）は `SwerveDrive.setMotorIdleMode` で設定できます。
* スワーブモジュールのドライブモーターフィードフォワードは `SwerveDrive.replaceSwerveModuleFeedforward` で上書きできます。
* `SwerveController.addSlewRateLimiters` を使ってスルーレートリミッターを追加し、操作性を向上させることができます。
* `Matter` クラスを使ったモーメント計算器により、ロボットの最大速度を制限して転倒を防ぎます。
* CAN フレームは前回と異なる角度・速度の更新にのみ送信されるよう制限されています。
* `SwerveDrive.setMaximumSpeed` と `SwerveController.setMaximumAngularVelocity`、またはユーティリティ関数 `SwerveDrive.setMaximumSpeeds` で最大速度を上書きできます。
* `SwerveDrive.chassisVelocityCorrection` を使い、`SwerveDrive.drive` 関数のみにシャシー速度補正を適用できます。
* `SwerveDrive.drive` で異なる回転中心を使った制御が可能です。
* `SwerveDrive.pushOffsetsToControllers` でオフセットをモーターコントローラーに書き込めます。
* `SwerveDrive.setChassisSpeedsDisctretization` で `ChassisSpeeds.discretize` を制御できます。
* `SwerveDrive.setCosineCompensator` でコサイン補正を設定できます。
* `SwerveDrive.setHeadingCorrection` でヘディング補正を設定できます。
* `SwerveDrive.setAutoCenteringModules` でオートセンタリングモジュールを制御できます（入力がない場合にモジュールを 0 に戻す機能）。

## 安全機能

* 絶対エンコーダーで解決可能な読み取りエラーが発生した場合、相対エンコーダーの値にフォールバックします。
* 角度モーターはデフォルトでコーストモードになっており、モーターへの負担を軽減します。
* モーターの角度は最も近い等価角度への回転に最適化されています。
* `SwerveMath.limitVelocity` でロボットの重量と重心をもとに速度を制限できます。
* JSON 設定ファイルで電流制限を設定できます。
* JSON 設定ファイルでランプレート制限を設定できます。
* SparkMAX および TalonFX 専用のクローズドループ PID をモーターコントローラー上で実行します。
* ロボットが 100ms 停止している間、絶対エンコーダーと相対エンコーダーを同期します。
* PID 入力は -180〜180 の範囲で連続または連続をエミュレートしています。

## オドメトリ

* `SwerveDrive.updateOdometry` は 20ms ごとに呼び出され、周期は `SwerveDrive.setOdometryPeriod` で変更できます。
* オドメトリスレッドを停止するには `SwerveDrive.stopOdometryThread` を使用し、periodic でオドメトリを更新してください。
* ジャイロスコープのリセットは `SwerveDrive.zeroGyro` で行います。
* ロボット基準の速度は `SwerveDrive.getRobotVelocity`、フィールド基準の速度は `SwerveDrive.getFieldVelocity` で取得できます。
* ロボットの位置は `SwerveDrive.getPose` で取得できます。
* ジャイロスコープの値は `SwerveDrive.getGyroRotation3d`、`SwerveDrive.getYaw`、`SwerveDrive.getPitch`、`SwerveDrive.getRoll` で取得できます。
* ビジョン入力によるロボット位置の更新は `SwerveDrive.addVisionMeasurement`（標準偏差のオプション引数あり）で行えます。
* ジャイロスコープのオフセットは `SwerveDrive.setGyroOffset` で設定でき、PathPlanner との連携に使用します。
* 並進オドメトリがずれているが操作は正しい場合、`SwerveDrive.invertOdometry` で並進オドメトリを反転できます。

## テレメトリ

* スワーブテレメトリは [frc-web-components](https://github.com/frc-web-components/frc-web-components/tree/version4) [アプリ](https://github.com/frc-web-components/app) に対応しています。
* 各モジュールの角度（相対・絶対エンコーダー両方）が報告されます。
* 各モジュールの速度が報告されます。
* 目標シャシー速度が報告されます。
* フィールド上のロボットの現在位置と向きを継続的に更新する Field2d が作成されます。
* テスト中に `SwerveDrive.postTrajectory` でトラジェクトリをフィールドに追加できます。

# ロボットを理解する

## YAGSL 設定に必要な前提知識

YAGSLの設定はあなた自身が行います！動作する場合もそうでない場合もあるサンプル設定を用意しています。ロボットの前提情報はベンダークライアントおよびロボットの物理的な特性から設定します。以下は、YAGSL設定前に把握しておくべきロボットの特性をほぼ網羅した一覧です。

| 特性 | 典型的な値 | 関連性 |
| --- | --- | --- |
| ドライブ [ギア比](../../fundamentals/swerve-modules.md#conversion-factor) | N/A | _ドライブギア比_ は、ホイールが1回転するためにドライブモーターシャフトが何回転する必要があるかを表す比率です。購入したスワーブモジュールのウェブサイトで通常確認できます。 |
| ステアリング [ギア比](../../fundamentals/swerve-modules.md#conversion-factor) | N/A | _ステアリングギア比_ は、ホイールが1回転するためにステアリングモーターシャフトが何回転する必要があるかを表す比率です。購入したスワーブモジュールのウェブサイトで通常確認できます。 |
| アブソリュートエンコーダーの1回転あたりのティック数 | 1 | [こちらを参照](../../devices/absolute-encoders.md) |
| CAN バス名 | `rio` | [CANivore](https://store.ctr-electronics.com/canivore/) を使用している場合、[Falcon500](https://store.ctr-electronics.com/falcon-500-powered-by-talon-fx/)、[Kraken](https://store.ctr-electronics.com/kraken-x60/)、[Pigeon2.0](https://store.ctr-electronics.com/pigeon-2/)、[CANCoder](https://store.ctr-electronics.com/cancoder/) などのCTREデバイスをそのバスに配置できます。[この値をCANivoreの名前に設定する必要があります](https://pro.docs.ctr-electronics.com/en/stable/docs/canivore/canivore-setup.html#renaming-canivores)。 |
| すべてのセンサーおよびモーターコントローラーのCAN、PWM、またはアナログ入力ID | N/A | これが正しくないと、あるモーターを別のモーターと思って制御してしまうという重大な問題が発生します！ |
| ジャイロスコープの接続方式（NavXのみ） | N/A | USBでNavXを使用する場合、デバイスタイプは `navx_usb` にしてください。MXP経由の場合は `navx_spi` を使用してください。 |
| モーターの反転状態 | N/A | ホイールを前進方向に回転させ、時計回りに回転するよう反転状態を設定する必要があります。 |
| アブソリュートエンコーダーの反転状態 | `false` | 通常、アブソリュートエンコーダーの値はステアリングモーターの動きと同じ方向に増加します。そうでない場合は変更が必要です！！！ |
| ジャイロスコープの反転状態 | `false` | ジャイロスコープは反時計回りを正方向とする必要があります。そうでない場合は反転が必要です！ |
| アブソリュートエンコーダーオフセット | N/A | アブソリュートエンコーダーオフセットは、すべてのモジュールを同じ方向に揃えた状態（**同じ向きに向いていること！**）で、ロボットが無効状態のときにベンダークライアントまたはSmartDashboardから値を読み取ることで得られます！ |
| モーターコントローラーのPID値 | N/A | NEOとFalcon500の典型的な値はYAGSL-Exampleで確認できますが、さらなる調整が必要な場合があります。理想的にはベンダークライアントで行えます。 |
| ロボット中心から各ホイール中心までのインチ単位の距離 | N/A | YAGSLでロボットを設定する際の [`SwerveDriveKinematics`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveKinematics.html) に使用します。 |

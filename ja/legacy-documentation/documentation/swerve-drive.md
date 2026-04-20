# スワーブドライブ

## スワーブドライブ（`swervedrive.json`）

スワーブドライブのJSON設定ファイルは、スワーブドライブ全体に関するすべての設定を行います。[`SwerveDriveJson.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/json/SwerveDriveJson.java) と1:1でマッピングされており、[`SwerveDrive`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/SwerveDrive.java) オブジェクト作成に使用される [`SwerveDriveConfiguration`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/SwerveDriveConfiguration.java) を生成します。

## JSONフィールド

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `imu` | [デバイス](../../configuring-yagsl/configuration/device-configuration.md) | Y | ロボットの向きを決定するIMU |
| `invertedIMU` | Boolean | Y | IMUの反転状態 |
| `modules` | String配列 | Y | 前左から時計回り順のモジュールJSON |

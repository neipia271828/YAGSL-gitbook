# コントローラープロパティ

## スワーブコントローラー（`controllerproperties.json`）

スワーブコントローラーは、自律走行モードおよびジョイスティックでロボットの向きを制御するドライブモード中のスワーブドライブの動作に関する設定オプションを格納します。このJSONファイルは [`ControllerPropertiesJson.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/json/ControllerPropertiesJson.java) と1:1でマッピングされており、[`SwerveControllerConfiguration`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/SwerveControllerConfiguration.java) を生成します。ここに記載されている値は**自律走行にとって極めて重要**です。自律走行機能を正常に動作させるには、ロボットのヘディングPIDを正しく調整する必要があります。

## フィールド

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `angleJoystickRadiusDeadband` | Double | Y | ロボットのヘディング調整を許可する角度制御ジョイスティックの最小半径 |
| `heading` | [PID](../../configuring-yagsl/configuration/pidf-properties-configuration/pidf.md) | Y | ロボットのヘディングを制御するPID |

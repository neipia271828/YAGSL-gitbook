# スワーブモジュール

## スワーブモジュール設定（`module/x.json`）

スワーブモジュール設定は各スワーブモジュール固有のプロパティを設定します。[`ModuleJson.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/json/ModuleJson.java) と1:1でマッピングされており、[`SwerveModuleConfiguration`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/SwerveModuleConfiguration.java) の作成に使用されます。この設定ファイルはスワーブキネマティクスと直接連動します。

## フィールド

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `drive` | [デバイス](../../configuring-yagsl/configuration/device-configuration.md) | Y | ドライブモーターデバイスの設定 |
| `angle` | [デバイス](../../configuring-yagsl/configuration/device-configuration.md) | Y | 角度モーターデバイスの設定 |
| `encoder` | [デバイス](../../configuring-yagsl/configuration/device-configuration.md) | Y | 絶対エンコーダーデバイスの設定 |
| `inverted` | [MotorConfig](swerve-module.md#motorconfig) | Y | 各モーターの反転状態（boolean） |
| `absoluteEncoderOffset` | 度 | Y | 0からの絶対エンコーダーオフセット（度）。負の値が必要な場合があります。 |
| `absoluteEncoderInverted` | Bool | N | 絶対エンコーダーの反転状態 |
| `location` | [Location](swerve-module.md#location) | Y | ロボット中心からスワーブモジュール中心までのインチ単位の位置。+xはロボット前方、+yはロボット左方向。 |
| `conversionFactor` | [MotorConfig](swerve-module.md#motorconfig) | N | _上書き_ オンボードPIDのモーターコントローラーに適用するコンバージョンファクター。[`swervedrive.json`](https://github.com/BroncBotz3481/YAGSL/wiki/Swerve-Drive) のこの設定を上書きするために使用。 |

#### MotorConfig

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `drive` | 値 | Y | ドライブモーターの値 |
| `angle` | 値 | Y | 角度モーターの値 |

#### Location

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `front` | 値 | Y | ロボット中心からモジュールまでの前方インチ距離 |
| `left` | 値 | Y | ロボット中心からモジュールまでの左方インチ距離 |

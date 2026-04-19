# スワーブモジュール物理プロパティ

## スワーブモジュール物理プロパティ（`physicalproperties.json`）

このJSONは全スワーブモジュールに共通する物理プロパティを設定します。[`PhysicalPropertiesJson.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/json/PhysicalPropertiesJson.java) と1:1でマッピングされており、[`SwerveModulePhysicalCharacteristics.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/SwerveModulePhysicalCharacteristics.java) の作成に使用されます。

## フィールド

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `conversionFactor` | [MotorConfig](swerve-module-physical-properties.md#motorconfig) | N | オンボードPIDのモーターコントローラーに適用するコンバージョンファクター |
| `optimalVoltage` | 電圧 | Y | 補正の基準となる最適電圧。フィードフォワード計算にも使用されます |
| `currentLimit` | [MotorConfig](swerve-module-physical-properties.md#motorconfig) | N | モーターに適用する電流制限（アンペア） |
| `rampRate` | [MotorConfig](swerve-module-physical-properties.md#motorconfig) | N | モーターが0からフルスロットルに達するまでの最小秒数 |
| `wheelGripCoefficientOfFriction` | カーペット上の摩擦係数 | N | カーペット上のグリップテープの摩擦係数。実用的な最大加速度の計算に使用されます |

#### MotorConfig

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `drive` | 数値 | Y | ドライブモーターの値 |
| `angle` | 数値 | Y | 角度モーターの値 |

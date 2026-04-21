# スワーブモジュールPIDFプロパティ

## スワーブモジュールPID設定（`pidfpropreties.json`）

このファイルは全スワーブモジュールのドライブモーターとアングルモーターのPIDF値（インテグラルゾーンおよび最大出力を含む）を設定します。[`PIDFPropertiesJson.java`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/parser/json/PIDFPropertiesJson.java) と1:1でマッピングされており、`SwerveDriveConfiguration` オブジェクトの初期化時に使用されます。

## フィールド

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `drive` | [インテグラルゾーンと出力制限付きPIDF](../../configuring-yagsl/configuration/pidf-properties-configuration/pidf.md) | Y | ドライブモーターのPIDFに使用される設定 |
| `angle` | [インテグラルゾーンと出力制限付きPIDF](../../configuring-yagsl/configuration/pidf-properties-configuration/pidf.md) | Y | 角度モーターのPIDFに使用される設定 |

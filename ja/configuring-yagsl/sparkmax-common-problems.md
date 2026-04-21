---
description: MAXSwerveロボットでよく発生します...
---

# SparkMax と SparkFlex のよくある問題

## NEOはSparkMAXキラー！

NEOが長時間のストールで十分に加熱されると、センサーケーブルへの正極からのショートが発生し、低電流チャンネルに非常に高い電流が流れてSparkMaxのPCBが溶けます。これによりSparkMAXが壊れ、同じ電源に接続されているすべてのSparkMAXも壊れる可能性があります。

電源が入ったSparkMAXにUSBで接続している場合、ショートがUSBポートを通じてノートパソコンにまで及ぶ可能性があります。

{% embed url="https://docs.google.com/presentation/d/1uM_uPD5HjzGwYUOmzZOmmgb1-KANiTTKI3o9aIJlPcc/edit?slide=id.p#slide=id.p" %}

## SparkMAX 絶対エンコーダーボード

[SparkMAX 絶対エンコーダーボード](https://www.revrobotics.com/rev-11-3326/)は、ボードが外れないよう冗長性を持たせるためにSparkMAXにジップタイとホットグルーで固定してください。

### 絶対エンコーダーボードへのスループットボアの接続

[SparkMax 絶対エンコーダーボード](https://www.revrobotics.com/rev-11-3326/)と[REV スループットボア](https://www.revrobotics.com/rev-11-1271/)のコネクターは、簡単に外れてしまうため**両方**の接続箇所をホットグルーで固定してください！

配線の張り具合も適切に調整してください。過度に引っ張られているとコネクターにケーブルが完全に入っていない可能性があり、データが不完全・破損・利用不可になることがあります。

## スループットボアが絶対エンコーダーオフセットに追従しない

{% hint style="danger" %}
attached絶対エンコーダーで `factor` を `360` に設定する必要はなくなりましたが、設定する場合の説明は以下の通りです。
{% endhint %}

絶対エンコーダーボード経由でSparkMaxに接続された絶対エンコーダーを使用している場合、通常はDutyCycleEncoderとして認識されます。

`physicalproperties.json` の `factor` を `360` に設定することで、これをプライマリフィードバックデバイスとして使用できます。**ただし**、その場合はJSONで指定した絶対エンコーダーオフセットが**適用されません**。そのため [`SwerveDrive.useExternalFeedbackSensor()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#useExternalFeedbackSensor()) を使用することをお勧めします。これにより、エンコーダーオフセットがSparkMAX DutyCycleEncoderのオフセットフラッシュ設定として**設定されます**。

SparkMax DutyCycleEncoderオフセットをリセットするには [`SwerveDrive.useInternalFeedbackSensor()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#useInternalFeedbackSensor()) を使用してください。これによりSpark DutyCycleEncoderオフセットが0に設定されます。

## ステータスフレームエラー

ステータスフレームエラーが発生する場合は、通常REVスループットボアへの接続が不完全であるか、モーターからのセンサーケーブルが緩んでいるか断線しています！

これが表示されたら、**そのSparkMAXに接続されているすべてのセンサーケーブルを確認してください！**

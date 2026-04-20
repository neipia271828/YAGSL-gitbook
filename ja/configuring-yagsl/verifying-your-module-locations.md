---
description: ロボットの前面が思っている場所と異なる場合があります。
---

# モジュール位置の確認

YAGSLで初期設定が完了したら、ブロックに乗せてモジュールが正しい方向を向いているかテストしてください。[モジュールを入れ替える](the-eight-steps.md#how-to-swap-module-configurations)必要がある場合は、ロボットプログラムが想定している通りの動作をするよう確認してください。

## 直進する

最初のテストは、ロボットに直進を命令することです。モジュールは実際のロボットでも、Elastic、[FRC Web Components](../analytics-and-debugging/frc-web-components.md)、または [Advantage Scope](../analytics-and-debugging/advantage-scope.md) 上でも以下のように見えるはずです。

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

実際のロボットがこのように見えない場合は、問題のあるモジュールをこれに合わせるよう変更する必要があります。

## ロボットを回転させる

確認ステップとして、モジュールが直進していることを確認した後は、ロボットはCCW+方向に回転するはずです。回転しようとしているときにこのどちらかに見えている限り問題ありません。

{% hint style="warning" %}
左側の図のようになっている場合は、ロボットの向きに基づいて並進軸が変化しているため、[8つのステップ](the-eight-steps.md)を実行する必要があります。
{% endhint %}

<figure><img src="../.gitbook/assets/image-48.png" alt=""><figcaption></figcaption></figure>

# Spark Flex

このページはREV Spark Flexを**ブラシレスモーター（REV Vortex）**に接続した場合について説明しています。

{% embed url="https://www.revrobotics.com/next-generation-spark-neo/" %}

{% embed url="https://www.revrobotics.com/rev-21-1652/" %}

{% embed url="https://www.revrobotics.com/rev-11-2159/" %}

## はじめに

[`CANSparkFlex`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkflex) オブジェクトを作成する簡単なサンプルプログラムです。

<pre class="language-java"><code class="lang-java">import com.revrobotics.CANSparkFlex;
import com.revrobotics.CANSparkLowLevel.MotorType;


new CANSparkFlex(<a data-footnote-ref href="#user-content-fn-1">10</a>, MotorType.kBrushless);
</code></pre>

## ドキュメント

Spark Flexでは、REV Hardware ClientからモーターコントローラーのPIDテスト、一定パーセントでのモーター駆動、ファームウェアの更新が可能です。YAGSLでSparkFlexを使用するにはREV Hardware Clientのインストールが必要です。

{% embed url="https://docs.revrobotics.com/rev-hardware-client-2" %}
REV Hardware Client 2のインストールガイドとダウンロード
{% endembed %}

{% embed url="https://docs.revrobotics.com/brushless/spark-flex/gs/make-it-spin" %}
REV Hardware Clientで回転させる
{% endembed %}

{% hint style="warning" %}
REV Hardware Clientで操作するには、Spark Flexを起動時にCANバスから**切断**する必要があります！
{% endhint %}

## PIDの調整

Spark FlexのPIDを調整するには、REV Hardware ClientでSpark Flexを選択し、テレメトリータブを開いてポジション制御に設定した状態で左パネルのパラメーターを変更してください。

{% embed url="https://docs.revrobotics.com/rev-hardware-client/ion/telemetry" %}

## YAGSLチェックリスト

* [ ] すべてのSpark FlexにユニークなCAN IDを設定する。
* [ ] すべてのSpark Flexを最新のファームウェアに更新する。
* [ ] Spark Flexがモーターを反時計回り正方向に回転させる。（そうでない場合はSparkFlexをプログラムで反転させる）
* [ ] 1つのドライブモーターのPID値を調整する。
* [ ] 1つのステアリング/アジマス/角度モーターのPID値を調整する。

## 通信

Spark FlexはCAN経由でのみ通信可能で、CAN FDサポートで近日中に新しい機能が追加されるかもしれません！

## モジュール設定例

`frontleft.json`、`frontright.json`、`backleft.json`、`backright.json` などのモジュール設定ファイルの例です。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": <a data-footnote-ref href="#user-content-fn-2">"sparkflex"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-3">5</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-4">null</a>
  },
  "angle": {
    "type": <a data-footnote-ref href="#user-content-fn-5">"sparkflex"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-6">6</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-7">null</a>
  },
  "encoder": {
    "type": "cancoder",
    "id": 11,
    "canbus": null
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-8">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-9">false</a>
  },
  "absoluteEncoderOffset": -18.281,
  "location": {
    "front": -12,
    "left": -12
  }
}
</code></pre>

## `pidfproperties.json` の例

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "p": <a data-footnote-ref href="#user-content-fn-10">0.0020645</a>,
    "i": <a data-footnote-ref href="#user-content-fn-11">0</a>,
    "d": <a data-footnote-ref href="#user-content-fn-12">0</a>,
    "f": <a data-footnote-ref href="#user-content-fn-13">0</a>,
    "iz": 0
  },
  "angle": {
    "p": <a data-footnote-ref href="#user-content-fn-14">0.01</a>,
    "i": <a data-footnote-ref href="#user-content-fn-15">0</a>,
    "d": <a data-footnote-ref href="#user-content-fn-16">0</a>,
    "f": <a data-footnote-ref href="#user-content-fn-17">0</a>,
    "iz": 0
  }
}
</code></pre>

## `physicalproperties.json` の例

<pre class="language-json"><code class="lang-json">{
  "conversionFactor": {
    "drive": <a data-footnote-ref href="#user-content-fn-18">0.047286787200699704</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-19">16.8</a>
  },
  "currentLimit": {
    "drive": <a data-footnote-ref href="#user-content-fn-20">40</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-21">20</a>
  },
  "rampRate": {
    "drive": <a data-footnote-ref href="#user-content-fn-22">0.25</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-23">0.25</a>
  },
  "wheelGripCoefficientOfFriction": 1.19,
  "optimalVoltage": 12
}
</code></pre>

[^1]: CAN IDが `10` のSpark Flexを指す。

[^2]: モータータイプとしてSpark Flexを選択。

[^3]: このドライブモーターコントローラーSpark FlexのCAN IDは `5`。

[^4]: Spark FlexはCANivoreと互換性がないため、`null` または `""` にする必要があります。将来変更される可能性があります！

[^5]: モータータイプとしてSpark Flexを選択。

[^6]: このドライブモーターコントローラーSparkFlexのCAN IDは `6`。

[^7]: Spark FlexはCANivoreと互換性がないため、`null` または `""` にする必要があります。将来変更される可能性があります！

[^8]: ドライブモーターは反転なしで反時計回り正方向に回転。

[^9]: ステアリング/アジマス/角度モーターは反転なしで反時計回り正方向に回転。

[^10]: Spark Flexがメートル/秒で目標速度を維持するために使用するkP。

[^11]: kIは通常設定不要。

[^12]: kDはオーバーシュートを最小限に抑えながら速度を達成するために有効です。

[^13]: ホイールを回転させるための電圧パーセントとしての静的フィードフォワード。

[^14]: Spark Flexが度で目標角度を維持するために使用するkP。

[^15]: kIは通常設定不要。

[^16]: kDはオーバーシュートを最小限に抑えながら速度を達成するために有効です。

[^17]: ホイールを回転させるための電圧パーセントとしての静的フィードフォワード。

[^18]: NEOを搭載したMK4i L2のコンバージョンファクター。回転/分をメートル/秒に変換します。[`CANSparkFlex.getEncoder().setPositionConversionFactor()`](https://codedocs.revrobotics.com/java/com/revrobotics/relativeencoder#setPositionConversionFactor\(double\)) で設定します。

[^19]: NEOを搭載したMK4i L2のコンバージョンファクター。回転を度に変換します。[`CANSparkFlex.getEncoder().setPositionConversionFactor()`](https://codedocs.revrobotics.com/java/com/revrobotics/relativeencoder#setPositionConversionFactor\(double\)) で設定します。

[^20]: ドライブモーターが引き出せる最大電流は `40` アンペア。[`CANSparkFlex.setSmartCurrentLimit`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setSmartCurrentLimit\(int\)) で設定します。

[^21]: 角度モーターが引き出せる最大電流は `20` アンペア。[`CANSparkFlex.setSmartCurrentLimit`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setSmartCurrentLimit\(int\)) で設定します。

[^22]: ブラウンアウト防止のためのSparkMAXの最大ランプレート。[`CANSparkFlex.setClosedLoopRampRate`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setClosedLoopRampRate\(double\)) と [`CANSparkFlex.setOpenLoopRampRate`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setOpenLoopRampRate\(double\)) で設定します。

[^23]: ブラウンアウト防止のためのSparkMAXの最大ランプレート。[`CANSparkMAX.setClosedLoopRampRate`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setClosedLoopRampRate\(double\)) と [`CANSparkMax.setOpenLoopRampRate`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkbase#setOpenLoopRampRate\(double\)) で設定します。

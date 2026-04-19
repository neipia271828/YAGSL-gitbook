# TalonFX

## はじめに

TalonFXはKraken X60とFalcon 500を制御するモーターコントローラーです。

{% embed url="https://wcproducts.com/products/kraken" %}

{% hint style="warning" %}
Falcon 500 v2（実質的に廃番）を使用している場合は、こちらに記載されているLocktiteの修正を必ず適用してください！\
[https://content.vexrobotics.com/vexpro/Falcon/217-6515-753-Falcon500-V2-Upgrade.pdf](https://content.vexrobotics.com/vexpro/Falcon/217-6515-753-Falcon500-V2-Upgrade.pdf)
{% endhint %}

REV NEOよりも強力で効率的なため、ドライブモーターとしてよく使用されます。

```java
import com.ctre.phoenix6.hardware.TalonFX;
import com.ctre.phoenix6.controls.DutyCycleOut;

TalonFX motor = new TalonFX(10);
motor.setControl(new DutyCycleOut(1.0)); // 100% full speed positive.
```

## ドキュメント

このデバイスはCTRE Tuner Xでアップグレードできます。

{% embed url="https://v6.docs.ctr-electronics.com/en/latest/docs/tuner/index.html" %}
Tuner Xのインストールとドキュメント
{% endembed %}

{% embed url="https://pro.docs.ctr-electronics.com/en/stable/docs/hardware-reference/talonfx/index.html" %}
公式ドキュメント
{% endembed %}

デバッグの際にはLEDステータスコードに注目してください。なお、Tuner XでTalon FXの設定を変更しても、YAGSLの起動時に上書きされます。

## YAGSLチェックリスト

* [ ] すべてのTalonFXにユニークなCAN IDを設定する。
* [ ] すべてのTalonFXを最新のファームウェアに更新する。
* [ ] TalonFXがモーターを反時計回り正方向に回転させる。（そうでない場合はTalonFXをプログラムで反転させる）
* [ ] 1つのドライブモーターのPID値を調整する。
* [ ] 1つのステアリング/アジマス/角度モーターのPID値を調整する。

## モジュール設定例

`frontleft.json`、`frontright.json`、`backleft.json`、`backright.json` などのモジュール設定ファイルの例です。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": <a data-footnote-ref href="#user-content-fn-1">"talonfx"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-2">5</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-3">null</a>
  },
  "angle": {
    "type": <a data-footnote-ref href="#user-content-fn-4">"talonfx"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-5">6</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-6">null</a>
  },
  "encoder": {
    "type": "cancoder",
    "id": 11,
    "canbus": null
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-7">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-8">false</a>
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
    "p": <a data-footnote-ref href="#user-content-fn-9">0.0020645</a>,
    "i": <a data-footnote-ref href="#user-content-fn-10">0</a>,
    "d": <a data-footnote-ref href="#user-content-fn-11">0</a>,
    "f": <a data-footnote-ref href="#user-content-fn-12">0</a>,
    "iz": 0
  },
  "angle": {
    "p": <a data-footnote-ref href="#user-content-fn-13">0.01</a>,
    "i": <a data-footnote-ref href="#user-content-fn-14">0</a>,
    "d": <a data-footnote-ref href="#user-content-fn-15">0</a>,
    "f": <a data-footnote-ref href="#user-content-fn-16">0</a>,
    "iz": 0
  }
}
</code></pre>

## `physicalproperties.json` の例

<pre class="language-json"><code class="lang-json">{
  "conversionFactor": {
    "drive": <a data-footnote-ref href="#user-content-fn-17">0.047286787200699704</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-18">16.8</a>
  },
  "currentLimit": {
    "drive": <a data-footnote-ref href="#user-content-fn-19">40</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-20">20</a>
  },
  "rampRate": {
    "drive": <a data-footnote-ref href="#user-content-fn-21">0.25</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-22">0.25</a>
  },
  "wheelGripCoefficientOfFriction": 1.19,
  "optimalVoltage": 12
}
</code></pre>

[^1]: TalonFXを選択。

[^2]: このドライブモーターコントローラーTalonFXのCAN IDは `5`。

[^3]: TalonFXはCANivoreと互換性があり、使用することでCANバスの使用率を下げられます。TalonFXがCANivoreのCANループ上にある場合はCANivoreの名前を設定してください。

[^4]: TalonFXを選択。

[^5]: このドライブモーターコントローラーTalonFXのCAN IDは `6`。

[^6]: TalonFXはCANivoreと互換性があり、使用することでCANバスの使用率を下げられます。TalonFXがCANivoreのCANループ上にある場合はCANivoreの名前を設定してください。

[^7]: ドライブモーターは反転なしで反時計回り正方向に回転。

[^8]: ステアリング/アジマス/角度モーターは反転なしで反時計回り正方向に回転。

[^9]: TalonFXがメートル/秒で目標速度を維持するために使用するkP。

[^10]: kIは通常設定不要。

[^11]: kDはオーバーシュートを最小限に抑えながら速度を達成するために有効です。

[^12]: ホイールを回転させるための電圧パーセントとしての静的フィードフォワード。

[^13]: TalonFXが度で目標角度を維持するために使用するkP。

[^14]: kIは通常設定不要。

[^15]: kDはオーバーシュートを最小限に抑えながら速度を達成するために有効です。

[^16]: ホイールを回転させるための電圧パーセントとしての静的フィードフォワード。

[^17]: Krakenを搭載したMK4i L2のコンバージョンファクター。回転/分をメートル/秒に変換します。

[^18]: Krakenを搭載したMK4i L2のコンバージョンファクター。回転を度に変換します。

[^19]: ドライブモーターが引き出せる最大電流は `40` アンペア。

[^20]: 角度モーターが引き出せる最大電流は `20` アンペア。

[^21]: ブラウンアウト防止のためのTalonFXの最大ランプレート。

[^22]: ブラウンアウト防止のためのTalonFXの最大ランプレート。

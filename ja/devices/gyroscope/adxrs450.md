# ADXRS450

{% hint style="warning" %}
ADXRS450は±300°/秒の制限があり、限界を超えると回転データの提供が停止します。\
このジャイロスコープは競技会での使用は推奨されません。
{% endhint %}

{% hint style="warning" %}
このジャイロスコープを使用する場合は、起動のたびにロボットを静止させてください。
{% endhint %}

## ドキュメント

{% embed url="https://wiki.analog.com/first/adxrs450_gyro_board_frc" %}
公式ドキュメント
{% endembed %}

{% embed url="https://docs.wpilib.org/en/stable/docs/software/hardware-apis/sensors/gyros-software.html#adxrs450-gyro" %}
初期化方法のWPILibドキュメント
{% endembed %}

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>単軸読み取り</p></figcaption></figure>

ADXRS450は古い単軸ジャイロスコープのため、正常に動作するには地面と平行に配置する必要があります！

## YAGSLチェックリスト

* [ ] ADXRS450がroboRIOにしっかり取り付けられていることを確認する。
* [ ] roboRIOがロボットの中央に配置されている。
* [ ] roboRIOが地面と平行になっている。
* [ ] 起動時に少なくとも10秒間ロボットをまったく動かさないこと！

## 通信

ADXRS450はroboRIOのSPIポートに直接接続され、YAGSLでの接続はこの方法のみです。

## `swervedrive.json` の例

<pre class="language-json"><code class="lang-json">{
  "imu": {
    "type": <a data-footnote-ref href="#user-content-fn-1">"adxrs450"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-2">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-3">null</a>
  },
  "invertedIMU": <a data-footnote-ref href="#user-content-fn-4">false</a>,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: `adxrs450` ジャイロスコープをプライマリジャイロスコープとして選択。

[^2]: NavXではIDは関係ないため `0` を任意で選択。

[^3]: NavXでは `canbus` は関係ないため `null` で設定なしを示す。

[^4]: デフォルトで反時計回りを正方向として読み取る。

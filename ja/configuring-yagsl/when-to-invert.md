# 反転させるタイミング

スワーブモジュールとスワーブドライブを正常に動作させるには、いくつかの反転設定が必要です。目標は、すべてが反時計回りを正方向とするようにすることです！

{% hint style="warning" %}
ブロックに乗せているときは問題ないのに地面で走るとギアが鳴る場合、ホイールが正しい方向を向いていて正しく回転しているなら、反転ではなくPIDの調整が必要かもしれません！
{% endhint %}

{% hint style="warning" %}
反転が正しくない場合、モジュールまたはロボット全体が「制御不能」に回転する場合があります。
{% endhint %}

## スワーブモーター

モーターを反時計回りに回転させると、NetworkTablesの `swerve/modules/.../Raw Absolute Encoder` と `swerve/modules/.../Raw Angle Encoder` の両方が増加するはずです。

* ![](../.gitbook/assets/ShuffleboardAbsoluteEncoderHighlight.png)
* `swerve/modules/.../Raw Absolute Encoder` の値に注目し、モジュールJSONの `absoluteEncoderOffset` に使用してください。

## モジュールを反時計回りに回転させる

{% hint style="danger" %}
モジュールは上から見て**反時計回り**に回転させてください。
{% endhint %}

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption><p>ベベルが向くべき方向を紫で表示（Team 2876 提供の写真）</p></figcaption></figure>

スワーブドライブは横に倒すか、その他の方法で持ち上げてください。スワーブモジュールのベベルは図のように左を向いている必要があります。スワーブモジュールを回転させる際は、図のように反時計回りに回転させてください。

### `swerve/modules/.../Raw Angle Encoder` が減少している場合...

減少しているすべてのモジュールの角度モーターを反転させてください！

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
    "drive": false,
<strong>    "angle": true
</strong>  },
  "absoluteEncoderInverted": false,
  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

### `swerve/modules/.../Raw Absolute Encoder` が減少している場合...

モジュールJSONの `absoluteEncoderInverted` で絶対エンコーダーを反転させてください。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
    "drive": false,
    "angle": false
  },
<strong>  "absoluteEncoderInverted": true,
</strong>  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

## ホイールを反時計回りに回転させる

{% hint style="warning" %}
フィールド指向モードで運転するとロボットの前後が入れ替わって回転する場合、これらを反転させる必要があるかもしれません。
{% endhint %}

{% hint style="danger" %}
#### オドメトリーの不一致

スピン・イン・プレイスが反時計回り正方向の動きをしている状態で、オドメトリー上ではロボットが後退しているのに実際は前進している場合、このパッチを適用する必要があります。

この動作を修正するには、すべてのモジュールファイルのすべての `absoluteEncoderOffset` に `180` を加算してください。
{% endhint %}

### `swerve/modules/.../Raw Drive Encoder` が減少している場合...

減少しているすべてのモジュールのドライブモーターを反転させてください！

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
<strong>    "drive": true,
</strong>    "angle": false
  },
  "absoluteEncoderInverted": false,
  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

## ロボットを反時計回りに回転させる

{% hint style="danger" %}
ホイールに電力を供給するとロボットの反時計回り回転はこのようになるはずです！モジュールファイルの各IDを変更する必要がある場合があります。
{% endhint %}

<figure><img src="../.gitbook/assets/id_change1.png" alt=""><figcaption><p>スワーブモジュールファイルのIDを変更</p></figcaption></figure>

ドライバーダッシュボードの `Raw IMU Yaw` フィールドが増加するはずです。増加しない場合は、以下のようにIMUを反転させる必要があります。

<pre class="language-json"><code class="lang-json">{
  "imu": {
    <a data-footnote-ref href="#user-content-fn-1">"type": "pigeon2"</a>,
    "id": 13,
    "canbus": "canivore"
  },
<strong>  "invertedIMU": true,
</strong>  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: 詳細は [gyroscope.md](../devices/gyroscope.md "mention") を参照してください。

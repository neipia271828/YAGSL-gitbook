# スワーブモジュール設定

## スワーブモジュール設定（`module/x.json`）

スワーブモジュール設定は各スワーブモジュール固有のプロパティを設定します。[`ModuleJson`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/json/ModuleJson.html) と1:1でマッピングされており、[`SwerveModuleConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html) の作成に使用されます。この設定ファイルはスワーブキネマティクスと直接連動します。

{% hint style="warning" %}
角度モーターが制御不能に回転する場合は、`inverted` で反転させる必要があるかもしれません。
{% endhint %}

{% hint style="warning" %}
ドライブモーターが制御不能に回転する場合は、IDによってドライブモーターが角度モーターとして設定されている可能性があります。必ず確認してください！
{% endhint %}

## フィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>drive</code></td><td><a href="../../devices/motor-controllers/#motor-controller-configuration">モーターコントローラー</a></td><td>Y</td><td>ドライブモーターデバイスの設定</td></tr><tr><td><code>angle</code></td><td><a href="../../devices/motor-controllers/#motor-controller-configuration">モーターコントローラー</a></td><td>Y</td><td>角度モーターデバイスの設定</td></tr><tr><td><code>encoder</code></td><td><a href="../../devices/absolute-encoders.md#absolute-encoder-configuration">絶対エンコーダー</a></td><td>Y</td><td>絶対エンコーダーデバイスの設定</td></tr><tr><td><code>inverted</code></td><td><a href="swerve-module-configuration.md#motorconfig">MotorConfig</a></td><td>Y</td><td>各モーターの反転状態（boolean）</td></tr><tr><td><code>absoluteEncoderOffset</code></td><td>度</td><td>Y</td><td>0からの絶対エンコーダーオフセット（度）。負の値が必要な場合があります。</td></tr><tr><td><code>absoluteEncoderInverted</code></td><td>Bool</td><td>N</td><td>絶対エンコーダーの反転状態</td></tr><tr><td><code>location</code></td><td><a href="swerve-module-configuration.md#location">Location</a></td><td>Y</td><td>ロボット中心からスワーブモジュール中心までのインチ単位の位置。+xはロボット前方、+yはロボット左方向。</td></tr><tr><td><code>conversionFactors</code></td><td><a href="physical-properties-configuration.md#conversion-factor-composition">コンバージョンファクター構成</a><br>存在かつ <code>0</code> でない場合は <a href="physical-properties-configuration.md#fields"><code>physicalproperties.json</code></a> の値を上書き。</td><td>N</td><td><em>上書き</em> オンボードPIDのモーターコントローラーに適用するコンバージョンファクター。<a href="https://github.com/BroncBotz3481/YAGSL/wiki/Swerve-Drive"><code>swervedrive.json</code></a> のこの設定を上書きするために使用。</td></tr><tr><td><code>useCosineCompensator</code></td><td>Bool</td><td>N</td><td>コサイン補正を無効化。目標角度との差のコサインでモジュール速度をスケールします。</td></tr></tbody></table>

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

## 設定例

<pre class="language-json"><code class="lang-json">{
  "drive": {
    <a data-footnote-ref href="#user-content-fn-1">"type": "sparkmax"</a>,
    "id": 2,
    "canbus": null
  },
  "angle": {
    <a data-footnote-ref href="#user-content-fn-2">"type": "sparkmax"</a>,
    "id": 1,
    "canbus": null
  },
  "encoder": {
    <a data-footnote-ref href="#user-content-fn-3">"type": "cancoder"</a>,
    "id": 10,
    "canbus": null
  },
  "inverted": {
    "drive": false,
    "angle": false
  },
  "absoluteEncoderInverted": false,
  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

[^1]: 詳細は [motor-controllers](../../devices/motor-controllers/ "mention") を参照してください。

[^2]: 詳細は [motor-controllers](../../devices/motor-controllers/ "mention") を参照してください。

[^3]: 詳細は [absolute-encoders.md](../../devices/absolute-encoders.md "mention") を参照してください。

# PIDFプロパティ設定

## スワーブモジュールPID設定（`modules/pidfpropreties.json`）

このファイルは、すべてのスワーブモジュールのドライブモーターと角度モーターのPIDF値（積分ゾーンと最大出力含む）を設定します。[`PIDFPropertiesJson`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/json/PIDFPropertiesJson.html) と1:1でマッピングされており、[`SwerveDriveConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveDriveConfiguration.html) オブジェクトの初期化に使用されます。

{% hint style="warning" %}
TalonFX/Kraken/Falconの角度モーターには高いPIDが必要です（kPは約 `50`、kDは約 `0.32`）。
{% endhint %}

## フィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>drive</code></td><td><a href="pidf.md">積分ゾーンと出力制限付きPIDF</a></td><td>Y</td><td>ドライブモーターのPIDF設定</td></tr><tr><td><code>angle</code></td><td><a href="pidf.md">積分ゾーンと出力制限付きPIDF</a></td><td>Y</td><td>角度モーターのPIDF設定</td></tr></tbody></table>

## 設定ファイルの例

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "p": 0.0020645,
    "i": 0,
    "d": 0,
    "f": 0,
    "iz": 0
  },
  "angle": {
<strong>    <a data-footnote-ref href="#user-content-fn-1">"p": 0.01</a>,
</strong>    "i": 0,
<strong>    <a data-footnote-ref href="#user-content-fn-2">"d": 0</a>,
</strong>    "f": 0,
    "iz": 0
  }
}
</code></pre>

[how-to-tune-pidf.md](../../how-to-tune-pidf.md "mention")

[^1]: KrakenやFalconのようなTalonFXモーターコントローラーの場合は約 `50` にしてください。詳細は [how-to-tune-pidf.md](../../how-to-tune-pidf.md "mention") を参照してください。

[^2]: TalonFXモーターコントローラーの場合は約 `0.32` にしてください。詳細は [how-to-tune-pidf.md](../../how-to-tune-pidf.md "mention") を参照してください。

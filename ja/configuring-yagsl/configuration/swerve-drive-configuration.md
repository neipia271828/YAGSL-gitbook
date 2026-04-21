# スワーブドライブ設定

## スワーブドライブ（`swervedrive.json`）

スワーブドライブのJSON設定ファイルは、スワーブドライブ全体に関するすべての設定を行います。[`SwerveDriveJson`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/json/SwerveDriveJson.html) と1:1でマッピングされており、[`SwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html) オブジェクト作成に使用される [`SwerveDriveConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveDriveConfiguration.html) を生成します。

## JSONフィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>imu</code></td><td><a href="../../devices/gyroscope.md#gyroscope-configuration">ジャイロスコープ</a></td><td>Y</td><td>ロボットの向きを決定するIMU。</td></tr><tr><td><code>invertedIMU</code></td><td>Boolean</td><td>Y</td><td>IMUの反転状態。</td></tr><tr><td><code>modules</code></td><td>String配列</td><td>Y</td><td>前左から時計回り順のモジュールJSON。</td></tr></tbody></table>

## 設定例

<pre class="language-json"><code class="lang-json">{
  "imu": {
    <a data-footnote-ref href="#user-content-fn-1">"type": "pigeon2"</a>,
    <a data-footnote-ref href="#user-content-fn-2">"id": 13</a>,
    <a data-footnote-ref href="#user-content-fn-3">"canbus"</a>: <a data-footnote-ref href="#user-content-fn-4">"canivore"</a>
  },
  "invertedIMU": true,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: 詳細は [gyroscope.md](../../devices/gyroscope.md "mention") を参照してください。

[^2]: IMU/ジャイロスコープがroboRIOに接続されていて、SPIまたはMXPポート経由ではない場合は接続チャンネル番号を指定します。CANデバイスの場合はデバイスのCAN IDを指定します。

[^3]: この例ではCANivoreは `"canivore"` という名前で、`pigeon2` はroboRIOのCANループではなくCANivoreに接続されています（roboRIOに接続する場合は `null` または `"rio"` にします）。

[^4]: IMU/ジャイロスコープがCANivore上にない場合は `null` または `"rio"` にしてください。

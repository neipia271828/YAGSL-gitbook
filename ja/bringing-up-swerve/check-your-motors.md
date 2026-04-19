# モーターを確認する

{% hint style="danger" %}
CAN ID がユニークであることを確認してください！REV Power Distribution Hub の CAN ID が SparkMAX の CAN ID と同じ場合、**SparkMAX がモーターを動かさない**という既知の問題があります！
{% endhint %}

## 各コンポーネントに ID を物理的にラベリングする

スワーブドライブ（または他のドライブシステム）の設定でミスを引き起こす最も簡単な方法は、ドライブモーター、角度モーター、アブソリュートエンコーダー、ジャイロスコープなどのコンポーネントに対して、間違ったチャンネル番号、CAN ID、または CAN バス（デバイスが CANivore 上にある場合）を入力することです。NavX のように ID を持たないコンポーネントは、接続方式を定義するいくつかの `type`（`navx_spi`、`navx_usb`、`navx_i2c`）があり、事前に知っておく必要があります。

設定を始める前に [getting-to-know-your-robot](../configuring-yagsl/getting-to-know-your-robot/ "mention") シートから情報を確認することを強くお勧めします。

## モーターは反時計回りを正方向とすること

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption><p>左右は物理的な左右です。</p></figcaption></figure>

ロボットが無効状態でモーターを回転させると、`swerve/modules/.../Raw Angle Encoder`（角度/ステアリング/アジマスの相対エンコーダー）と `swerve/modules/.../Raw Absolute Encoder`（アブソリュートエンコーダー）が確認できます。どちらも反時計回りに回転させると増加する必要があります。詳細はこちらを参照してください。

{% content-ref url="../configuring-yagsl/when-to-invert.md" %}
[when-to-invert.md](../configuring-yagsl/when-to-invert.md)
{% endcontent-ref %}

## コンバージョンファクターとモーター

コンバージョンファクターはモーターに適用され、ネイティブ単位（通常は**回転数**）をステアリング/アジマス/角度モーターでは度に、ドライブモーターではメートルに変換します。コンバージョンファクターはモーターコントローラーにのみ関係し、アブソリュートエンコーダーがモーターコントローラーに接続されている場合を除きます。

## アブソリュートエンコーダーのオフセット

{% hint style="warning" %}
ここでのすべての例は Shuffleboard をドライバーダッシュボードとして使用していますが、[Advantage Scope](https://docs.advantagescope.org/) や [Elastic](https://github.com/Gold872/elastic-dashboard) などの代替手段でも実施可能です。
{% endhint %}

アブソリュートエンコーダーのオフセットは、電源オフ後もスワーブモジュールがホイールの向きを維持するためのものです。スワーブドライブの機能に不可欠です。

1. すべてのホイールのベベルが左を向くよう揃える。

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

2. コードをデプロイする。
3. **ロボットを有効にしないこと！**
4. ドライバーダッシュボードを開く。

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ShuffleboardAbsoluteEncoderHighlight.png" alt=""><figcaption></figcaption></figure>

7. `swerve/modules/.../Raw Absolute Encoder` の値をメモし、モジュール JSON の `absoluteEncoderOffset` として使用する。

{% hint style="warning" %}
**MAXSwerve チームへの特別注意！**

MAXSwerveModules を使用して位置を合わせ、各モジュールの `absoluteEncoderOffset` を見つけた際、モジュール位置を修正するために `absoluteEncoderOffset` に **+/-`90`** する必要がある場合があります。また、すべてのモジュール設定で `absoluteEncoderInverted` を `true` にする必要があります。

<pre class="language-json"><code class="lang-json">{
  "drive": {"type": "sparkmax", "id": 12, "canbus": null},
  "angle": {"type": "sparkmax", "id": 11, "canbus": null},
  "encoder": {"type": "attached", "id": 0, "canbus": null},
  "inverted": {"drive": false, <strong>"angle": false</strong>},
  <strong>"absoluteEncoderInverted": true,</strong>
  "absoluteEncoderOffset": -90,
  "location": {"front": 12, "left": -12}
}
</code></pre>
{% endhint %}

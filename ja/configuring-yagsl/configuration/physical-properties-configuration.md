# 物理プロパティ設定

## スワーブモジュール物理プロパティ（`modules/physicalproperties.json`）

このJSONはすべてのスワーブモジュールで共通の物理プロパティを設定します。[`PhysicalPropertiesJson`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/json/PhysicalPropertiesJson.html) と1:1でマッピングされており、[`SwerveModulePhysicalCharacteristics`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModulePhysicalCharacteristics.htmll) を生成します。

{% hint style="info" %}
COTSスワーブモジュールを使用している場合は、[standard-conversion-factors.md](../standard-conversion-factors.md "mention") ページでロボットのコンバージョンファクターを設定してください！
{% endhint %}

## フィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th width="115">必須</th><th>説明</th></tr></thead><tbody><tr><td><code>conversionFactors</code></td><td><a href="physical-properties-configuration.md#conversion-factor-composition">コンバージョンファクター構成</a></td><td>Y</td><td>コンバージョンファクターの構成。起動時に係数が計算されます。</td></tr><tr><td><code>optimalVoltage</code></td><td>電圧</td><td>Y</td><td>補正の基準となる最適電圧とフィードフォワード計算の基準電圧。</td></tr><tr><td><code>currentLimit</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a><br>詳細は<a href="../../fundamentals/swerve-modules.md#current-limiting">こちら</a></td><td>N</td><td>モーターに適用する電流制限（アンペア）。SparkMAXはステーター制限、TalonFXはサプライ制限。</td></tr><tr><td><code>rampRate</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a></td><td>N</td><td>モーターが0から最大スロットルに達するまでの最小秒数。</td></tr><tr><td><code>friction</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a></td><td>N</td><td>ホイールまたはモジュールを動かすための最小電圧。ドライブモーターのデフォルトは <code>0.2</code>、角度モーターは <code>0.3</code>。</td></tr><tr><td><code>robotMass</code></td><td>ポンド（Lb）</td><td>N</td><td>デフォルトは <code>50</code> kg。</td></tr><tr><td><code>steerRotationalInertia</code></td><td>キログラム平方メートル</td><td>N</td><td>`KilogramMetersSquare` 単位の回転慣性。デフォルトは <code>0.03</code>。</td></tr><tr><td><code>wheelGripCoefficientOfFriction</code></td><td>カーペット上の摩擦係数</td><td>N</td><td>カーペット上のグリップテープの摩擦係数。実用最大加速度の計算に使用。</td></tr></tbody></table>

#### MotorConfig

| 名前 | 単位 | 必須 | 説明 |
| --- | --- | --- | --- |
| `drive` | 数値 | Y | ドライブモーターの値 |
| `angle` | 数値 | Y | 角度モーターの値 |

#### コンバージョンファクター構成

<table><thead><tr><th>名前</th><th>単位</th><th width="62">必須</th><th>説明</th></tr></thead><tbody><tr><td><code>drive</code></td><td><a href="physical-properties-configuration.md#drive-conversion-factor-composition">ドライブ構成</a></td><td>Y</td><td>ドライブコンバージョンファクター構成</td></tr><tr><td><code>angle</code></td><td><a href="physical-properties-configuration.md#angle-conversion-factor-composition">角度構成</a></td><td>Y</td><td>角度コンバージョンファクター構成</td></tr></tbody></table>

#### ドライブコンバージョンファクター構成

<table><thead><tr><th width="149">名前</th><th width="117">単位</th><th width="106">必須</th><th>説明</th></tr></thead><tbody><tr><td><code>diameter</code></td><td>インチ</td><td>Y</td><td>ホイールの直径</td></tr><tr><td><code>gearRatio</code></td><td>数値</td><td>Y</td><td>ドライブモーターからホイールへのギア比</td></tr><tr><td><code>factor</code></td><td>数値</td><td>N</td><td>事前計算済みのコンバージョンファクター</td></tr></tbody></table>

#### 角度コンバージョンファクター構成

<table><thead><tr><th width="156">名前</th><th width="123">単位</th><th width="87">必須</th><th>説明</th></tr></thead><tbody><tr><td><code>gearRatio</code></td><td>数値</td><td>Y</td><td>角度/ステアリング/アジマスモーターからホイールへのギア比</td></tr><tr><td><code>factor</code></td><td>数値</td><td>N</td><td>事前計算済みのコンバージョンファクター</td></tr></tbody></table>

## 設定ファイルの例

<pre class="language-json"><code class="lang-json">{
  <a data-footnote-ref href="#user-content-fn-1">"conversionFactors"</a>: {
	"angle": {"gearRatio": 28.125},
	"drive": {"diameter": 4, "gearRatio": 6.75}
  },
  "currentLimit": {
    "drive": 40,
    "angle": 20
  },
  "rampRate": {
    "drive": 0.25,
    "angle": 0.25
  },
  "wheelGripCoefficientOfFriction": 1.19,
  "optimalVoltage": 12
}
</code></pre>

[^1]: COTSスワーブモジュール向けの値は [standard-conversion-factors.md](../standard-conversion-factors.md "mention") で確認できます。

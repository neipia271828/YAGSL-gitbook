# PIDFのチューニング方法

{% hint style="warning" %}
スワーブのステアリング/角度/アジマスモーターは、最上点と最下点（例：「前」と「後」）でPIDのラッピングが有効になっています。PIDをチューニングする際は、左右方向の並進でテストすることをお勧めします。
{% endhint %}

## 基本的な考え方

YAGSLのPIDF値は、swerveモジュールフォルダ内の [`pidfproperties.json`](configuration/pidf-properties-configuration/) にあります。

以下のような簡潔な表現に落とし込まれています。

* P：目標位置にいない場合、そこへ向かう。
* I：目標位置に長い間到達できていない場合、より速く向かう。
* D：目標位置に近づいている場合、速度を落とす。
  * Team 78 が見つけた [動画](https://www.youtube.com/watch?v=qKy98Cbcltw)（[このCDの投稿](https://www.chiefdelphi.com/t/finally-i-understand-pid/450811)より）

<figure><img src="../.gitbook/assets/pid_explainer.png" alt=""><figcaption></figcaption></figure>

## PIDFのチューニング

[CTREによる](https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/device-specific/talonfx/closed-loop-requests.html)別の良い説明として、手動チューニングは一般的に以下のプロセスに従います。

1. $$P$$、$$I$$、$$D$$ をゼロに設定する。
2. $$P$$ を、出力がセットポイント周辺で**発振し始める**まで増加させる。
3. $$D$$ を、応答に**ジッターが発生しない**範囲でできる限り増加させる。

## WPILのウォークスルー

WPILibにはPIDを含む優れたドキュメントが多数あります。ぜひ確認してください！

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/introduction-to-pid.html" %}
PIDの基礎
{% endembed %}

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-flywheel.html" %}
速度チューニング（ドライブモーターなど）
{% endembed %}

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-turret.html" %}
位置チューニング（ステアリング/角度/アジマスモーターなど）
{% endembed %}

## 開始値

PIDF値はロボットによって異なるため、チューニングとテストが必要です。対応するモーターコントローラーの種類ごとに開始値を以下に示します。

{% tabs %}
{% tab title="ドライブモーター" %}
#### SparkMax

各モジュールの `drive` モーターの `type` が `sparkmax` の場合の開始値です。

<pre class="language-json"><code class="lang-json">{
<strong>  "drive": {
</strong><strong>    "p": 0.0020645,
</strong><strong>    "i": 0,
</strong><strong>    "d": 0,
</strong><strong>    "f": 0,
</strong><strong>    "iz": 0
</strong><strong>  },
</strong>  "angle": {
    "p": 0.01,
    "i": 0,
    "d": 0,
    "f": 0,
    "iz": 0
  }
}
</code></pre>

#### TalonFX

各モジュールのドライブモーターの `type` が `talonfx` または類似（`kraken`/`falcon`）の場合の開始値です。

<pre class="language-json"><code class="lang-json">{
<strong>  "drive": {
</strong><strong>    "p": 1,
</strong><strong>    "i": 0,
</strong><strong>    "d": 0,
</strong><strong>    "f": 0,
</strong><strong>    "iz": 0
</strong><strong>  },
</strong>  "angle": {
    "p": 50,
    "i": 0,
    "d": 0.32,
    "f": 0,
    "iz": 0
  }
}
</code></pre>
{% endtab %}

{% tab title="角度モーター" %}
#### SparkMax

各モジュールの `angle` モーターの `type` が `sparkmax` の場合の開始値です。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "p": 0.0020645,
    "i": 0,
    "d": 0,
    "f": 0,
    "iz": 0
  },
<strong>  "angle": {
</strong><strong>    "p": 0.01,
</strong><strong>    "i": 0,
</strong><strong>    "d": 0,
</strong><strong>    "f": 0,
</strong><strong>    "iz": 0
</strong><strong>  }
</strong>}
</code></pre>

#### TalonFX

各モジュールの `angle` モーターの `type` が `talonfx` または類似（`kraken`/`falcon`）の場合の開始値です。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "p": 1,
    "i": 0,
    "d": 0,
    "f": 0,
    "iz": 0
  },
<strong>  "angle": {
</strong><strong>    "p": 50,
</strong><strong>    "i": 0,
</strong><strong>    "d": 0.32,
</strong><strong>    "f": 0,
</strong><strong>    "iz": 0
</strong><strong>  }
</strong>}
</code></pre>
{% endtab %}
{% endtabs %}

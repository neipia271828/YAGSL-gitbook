---
description: 場合によっては役立ちます
---

# コサイン補正

## コサイン補正の仕組み

コサイン補正は、スワーブモジュールの角度デルタのコサイン値によってホイールの速度をスケーリングします。

## 有効にする方法

{% hint style="warning" %}
コサイン補正は**シミュレーションでは動作しません**。ロボット実機専用です！競技で使用する前に十分にテストしてください。
{% endhint %}

コサイン補正の有効化は簡単です！`SwerveDrive.setCosineCompensator` 関数を呼び出すだけです。

<pre class="language-java"><code class="lang-java"> /**
   * 指定されたディレクトリで {@link SwerveDrive} を初期化する。
   *
   * @param directory スワーブドライブ設定ファイルのディレクトリ。
   */
  public SwerveSubsystem(File directory)
  {
    // 不要なオブジェクトの生成を避けるため、SwerveDrive 作成前にテレメトリを設定する。
    SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
    swerveDrive = new SwerveParser(directory).createSwerveDrive(Constants.MAX_SPEED);
<strong>    swerveDrive.setCosineCompensator(false); // シミュレーションではコサイン補正を無効にする（実機と異なる挙動が生じるため）。
</strong>  }
</code></pre>

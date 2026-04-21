---
description: 一部のモーターコントローラーのみ対応しています！
---

# オフセットオフロード

## オフセットオフロードとは？

オフセットオフロードとは、絶対エンコーダーのオフセットを roboRIO ではなく、絶対エンコーダー自体（または絶対エンコーダーがモーターコントローラーに接続されている場合はモーターコントローラー）に保存することです。通常、roboRIO のループサイクルを高速化できますが、試合中に CAN バスの障害やブラウンアウトが発生した場合にスワーブドライブが不安定になる可能性があります。

## オフセットオフロードの有効化

{% hint style="warning" %}
[`SwerveDrive.pushOffsetsToEncoders`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#pushOffsetsToEncoders()) および [`SwerveDrive.restoreInternalOffset`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#restoreInternalOffset()) は YAGSL 2026 以降で非推奨となっています。
{% endhint %}

オフセットオフロードは [`SwerveDrive.pushOffsetsToEncoders`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#pushOffsetsToEncoders()) で有効にし、[`SwerveDrive.restoreInternalOffset`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#restoreInternalOffset()) で無効にします。いずれも、インスタンス化されているすべてのモジュールに対して `SwerveModule` の同名関数を呼び出します。

<pre class="language-java"><code class="lang-java">  /**
   * 指定されたディレクトリで {@link SwerveDrive} を初期化する。
   *
   * @param directory スワーブドライブ設定ファイルのディレクトリ。
   */
  public SwerveSubsystem(File directory)
  {
    // 不要なオブジェクトの生成を避けるため、SwerveDrive 作成前にテレメトリを設定する。
    SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
    swerveDrive = new SwerveParser(directory).createSwerveDrive(Constants.MAX_SPEED);
<strong>    swerveDrive.pushOffsetsToEncoders();
</strong>  }
</code></pre>

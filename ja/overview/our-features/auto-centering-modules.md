---
description: >-
  オートセンタリングモジュールは意図しないジッターを引き起こす可能性があるため、デフォルトでは無効になっています！
---

# オートセンタリングモジュール

## オートセンタリングモジュールとは？

オートセンタリングとは、コントローラーや自律制御からの入力がない場合に、ロボットがホイールを中央に戻す動作です。オートセンタリングが有効の場合、スワーブモジュールは入力がないたびに `0` に「スナップ」します。これを望むチームもありますが、推奨はされません。オートセンタリングは起動時に問題を引き起こし、自律制御でのロボットの向きがずれることがあります。また、通常より多くのドリフトを引き起こす可能性があります。オートセンタリングモジュールは [`SwerveModule.setAntiJitter(false)`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html#setAntiJitter(boolean)) を呼び出すことで機能するため、その副作用がすべて適用されます。

## オートセンタリングモジュールの有効化

`SwerveDrive.setAutoCenteringModules(true)` でオートセンタリングを有効にできます。

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
<strong>    swerveDrive.setAutoCenteringModules(true);
</strong>  }
</code></pre>

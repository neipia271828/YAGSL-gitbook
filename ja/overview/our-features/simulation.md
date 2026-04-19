---
description: YAGSL-Example はシミュレーションをすぐに利用できます！
---

# シミュレーション

## 仕組み

YAGSL は各ベンダーが提供するシミュレーションモジュールを使用して、ロボット全体をシミュレーションします（サポートの程度はベンダーによって異なります）。必要なデータを提供するために、`SwerveModuleSimulation` および `SwerveIMUSimulation` という専用のシミュレーションクラスが用意されています。

## 有効にする方法

{% hint style="warning" %}
シミュレーション実行中はコサイン補正とヘディング補正を無効にしてください。有効のままにすると、シミュレーションが制御不能になる場合があります！
{% endhint %}

シミュレーションを正しく動作させるには、以下の例のようにヘディング補正とコサイン補正を無効にするだけです。

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
<strong>    swerveDrive.setHeadingCorrection(false); // ヘディング補正は角度によるロボット制御時のみ使用する。
</strong><strong>    swerveDrive.setCosineCompensator(false); // シミュレーションではコサイン補正を無効にする（実機と異なる挙動が生じるため）。
</strong>  }
</code></pre>

その後、WPILib の通常のシミュレーション手順でシミュレーションを実行してください。

詳細は WPILib のシミュレーションドキュメントをご覧ください！

{% embed url="https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/introduction.html" %}

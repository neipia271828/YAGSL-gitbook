---
description: さらに数学的な処理...
---

# シャシー速度離散化

## シャシー速度離散化とは？

シャシー速度離散化は、スワーブドライブの並進移動時の軌道のずれを低減するために使用されます。[`SwerveDrive.chassisVelocityCorrection`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#chassisVelocityCorrection) が `true` の場合にのみ呼び出されます。

## シャシー速度離散化の使い方

シャシー速度離散化を使うと、WPILib の [`ChassisSpeeds.discretize`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/ChassisSpeeds.html#discretize\(edu.wpi.first.math.kinematics.ChassisSpeeds,double\)) に独自の値を設定できます。デフォルト値の `0.02` dt はほとんどのチームで十分です。離散化を有効にしてカスタマイズするには、以下のように [`SwerveDrive.setChassisDiscretization`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setChassisDiscretization(boolean,double)) を呼び出すだけです。

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
<strong>    swerveDrive.setChassisDiscretization(true, 0.02);
</strong>  }
</code></pre>

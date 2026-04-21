---
description: これは少し複雑です
---

# 最大速度

## 最大速度とは？

YAGSL は最大速度を保持し、以下の関数で使用します。

* [ ] [`SwerveDriveKinematics.desaturateWheelSpeeds`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveKinematics.html#desaturateWheelSpeeds\(edu.wpi.first.math.kinematics.SwerveModuleState\[],double\))
* [ ] [`SwerveMath.calculateMaxAngularVelocity`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/math/SwerveMath.html#calculateMaxAngularVelocity(double,double,double))
* [ ] [`SwerveDriveTelemetry.maxSpeed`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.html#maxSpeed)
* [ ] [`SwerveMath.antiJitter`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/math/SwerveMath.html#antiJitter(edu.wpi.first.math.kinematics.SwerveModuleState,edu.wpi.first.math.kinematics.SwerveModuleState,double))
* [ ] [`SwerveMath.createDriveFeedforward`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/math/SwerveMath.html#createDriveFeedforward(double,double,double))

最大速度はロボットの物理的な最大速度を表します。

## 最大速度の変更方法

最大速度は `SwerveDrive.setMaximumSpeed` で変更できます。初期最大速度は [`SwerveParser.createSwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveParser.html#createSwerveDrive(double)) で指定し、最大角速度・ドライブフィードフォワード・テレメトリの最大速度の初期値に使用されます。

{% hint style="info" %}
テレメトリの最大速度は変更されません。多くのウィジェットがこれをサポートしておらず、変更するとウィジェットが壊れる可能性があるためです！
{% endhint %}

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
<strong>    swerveDrive.setMaximumSpeed(Units.feetToMeters(12.4)); // 12.4ft/s を m/s に変換
</strong>  }
</code></pre>

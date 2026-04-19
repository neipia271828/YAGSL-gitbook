# コードのセットアップ

## YAGSLをインポートする

### オンライン

WPILibのvendor depsを使用してインストールしてください。

{% embed url="https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#rd-party-libraries" %}

### オフライン

[YAGSL-Exampleの `swervelib` ディレクトリ](https://github.com/Yet-Another-Software-Suite/YAGSL)をプロジェクトの `src/main/java` にコピーしてください。

{% hint style="warning" %}
オフライン版とオンライン版を同時にインストールすることはできません！エラーが発生します！
{% endhint %}

{% hint style="info" %}
[すべての依存関係](dependency-installation.md)もインストールしてください！
{% endhint %}

## スワーブドライブを作成するには？

YAGSLの特徴は、JSON設定ファイルだけでスワーブドライブを作成できる点です。JSON設定ファイルは [`deploy`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/deploy/swerve/neo) ディレクトリに配置してください。Configurationオブジェクトを手動で作成してSwerveDriveをインスタンス化することもできます。

### JSONを使ってSwerveDriveを作成する方法

このサンプルプログラムは [`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/blob/main/src/main/java/frc/robot/subsystems/swervedrive/SwerveSubsystem.java) 内で [`SwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html) を作成します。コマンドベースプログラミングを使用している場合、[`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/blob/main/src/main/java/frc/robot/subsystems/swervedrive/SwerveSubsystem.java) 内でのみ操作してください。

<pre class="language-java"><code class="lang-java">import java.io.File;
import edu.wpi.first.wpilibj.Filesystem;
import swervelib.parser.SwerveParser;
import swervelib.SwerveDrive;
import edu.wpi.first.math.util.Units;


double <a data-footnote-ref href="#user-content-fn-1">maximumSpeed </a>= Units.feetToMeters(4.5)
File swerveJsonDirectory = new File(Filesystem.getDeployDirectory(),"swerve");
SwerveDrive  swerveDrive = new SwerveParser(directory).createSwerveDrive(<a data-footnote-ref href="#user-content-fn-1">maximumSpeed</a>);

</code></pre>

この方法は速くて簡単で、大きくてメンテナンスしにくい定数ファイルを気にする必要がなくなります！JSONディレクトリの作成方法は[設定ドキュメント](configuration/)を参照してください。

## テレメトリー

テレメトリーは必要なときに大変役立ち、YAGSLにはドライバーダッシュボードで表示される `Module[...]` フィールドなど便利なテレメトリーが豊富にあります。ただし、テレメトリーはプログラムの遅延やスローダウンを引き起こすため、場合によってはオフにした方が良いこともあります。オフにするには以下を変更してください。

<pre class="language-java"><code class="lang-java">import swervelib.telemetry.SwerveDriveTelemetry;
import swervelib.telemetry.SwerveDriveTelemetry.TelemetryVerbosity;

<a data-footnote-ref href="#user-content-fn-2">SwerveDriveTelemetry.verbosity</a> = <a data-footnote-ref href="#user-content-fn-3">TelemetryVerbosity.HIGH</a>;
</code></pre>

## サンプルはこちら！

{% embed url="https://github.com/Yet-Another-Software-Suite/YAGSL/tree/main/examples" %}

{% hint style="info" %}
サンプルには高度な機能（昨年はdrive to pointコマンドなど）が含まれることがあります。定期的に確認してみてください！
{% endhint %}

## ドライブコード

`SwerveSubsystem` 内で、わずか数行で独自のドライブコードを作成できます。

```java
  /**
   * Command to drive the robot using translative values and heading as a setpoint.
   *
   * @param translationX Translation in the X direction.
   * @param translationY Translation in the Y direction.
   * @param headingX     Heading X to calculate angle of the joystick.
   * @param headingY     Heading Y to calculate angle of the joystick.
   * @return Drive command.
   */
  public Command driveCommand(DoubleSupplier translationX, DoubleSupplier translationY, DoubleSupplier headingX,
                              DoubleSupplier headingY)
  {
    return run(() -> {

      Translation2d scaledInputs = SwerveMath.scaleTranslation(new Translation2d(translationX.getAsDouble(),
                                                                                 translationY.getAsDouble()), 0.8);

      // Make the robot move
      driveFieldOriented(swerveDrive.swerveController.getTargetSpeeds(scaledInputs.getX(), scaledInputs.getY(),
                                                                      headingX.getAsDouble(),
                                                                      headingY.getAsDouble(),
                                                                      swerveDrive.getOdometryHeading().getRadians(),
                                                                      swerveDrive.getMaximumVelocity()));
    });
  }

  /**
   * Command to drive the robot using translative values and heading as angular velocity.
   *
   * @param translationX     Translation in the X direction.
   * @param translationY     Translation in the Y direction.
   * @param angularRotationX Rotation of the robot to set
   * @return Drive command.
   */
  public Command driveCommand(DoubleSupplier translationX, DoubleSupplier translationY, DoubleSupplier angularRotationX)
  {
    return run(() -> {
      // Make the robot move
      swerveDrive.drive(new Translation2d(translationX.getAsDouble() * swerveDrive.getMaximumVelocity(),
                                          translationY.getAsDouble() * swerveDrive.getMaximumVelocity()),
                        angularRotationX.getAsDouble() * swerveDrive.getMaximumAngularVelocity(),
                        true,
                        false);
    });
  }
```

[^1]: 最大速度は**メートル単位**で指定する必要があります！

[^2]: この[値](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.html#verbosity)はstaticであり、DriverStationとSmartDashboardへのテレメトリーを変更します。

[^3]: [テレメトリーの詳細度](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.TelemetryVerbosity.html)にはいくつかのモードがあります。

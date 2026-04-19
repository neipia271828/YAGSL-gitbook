# ヘディング補正

## ヘディング補正とは？

ヘディング補正は、ロボットが並進移動中に前回のヘディング（向き）を維持するために使用されます。積極的に機能するため、角度制御ベースの操縦スキームには干渉します。

ヘディング補正は Team 1466 によって YAGSL に追加され、7525 Pioneers および 6036 の BoiledBurntBagel によって改善されました。

## 有効にする方法

ヘディング補正の有効・無効は、どこからでも [`SwerveDrive.setHeadingCorrection`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setHeadingCorrection(boolean)) で切り替えられます。デッドバンドはメートル毎秒およびラジアン毎秒の両方を表す任意の値です。

## コード上の動作

ヘディング補正は [`SwerveDrive.drive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#drive(edu.wpi.first.math.kinematics.ChassisSpeeds,boolean,edu.wpi.first.math.geometry.Translation2d)) の中で、デッドバンド [`SwerveDrive.setHeadingCorrection`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setHeadingCorrection(boolean,double)) を使ってヘディングを制御します。`controllerproperties.json` のヘディング PID と現在のヨー角を使い、[`SwerveController.headingCalculate`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveController.html#headingCalculate(double,double)) でオメガ回転速度を計算します。

```java
    // ヘディング角速度デッドバンド（後で設定オプション化するかもしれない）。
    // 元々は Team 1466 Webb Robotics が作成。
    // Team 7525 Pioneers および 6036 の BoiledBurntBagel が改良。
    if (headingCorrection)
    {
      if (Math.abs(velocity.omegaRadiansPerSecond) < HEADING_CORRECTION_DEADBAND
          && (Math.abs(velocity.vxMetersPerSecond) > HEADING_CORRECTION_DEADBAND
              || Math.abs(velocity.vyMetersPerSecond) > HEADING_CORRECTION_DEADBAND))
      {
        if (!correctionEnabled)
        {
          lastHeadingRadians = getYaw().getRadians();
          correctionEnabled = true;
        }
        velocity.omegaRadiansPerSecond =
            swerveController.headingCalculate(lastHeadingRadians, getYaw().getRadians());
      } else
      {
        correctionEnabled = false;
      }
    }
```

---
description: >-
  YAGSLはオドメトリを管理し、必要なデータを追加できるよう拡張されています！
---

# ビジョンオドメトリ

## ビジョンオドメトリとは？

ビジョンオドメトリのデータは通常 [`SwerveDrivePoseEstimator.addVisionMeasurement`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/estimator/PoseEstimator.html#addVisionMeasurement\(edu.wpi.first.math.geometry.Pose2d,double,edu.wpi.first.math.Matrix\)) を使って追加されます。YAGSL は [`SwerveDrivePoseEstimator`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/estimator/SwerveDrivePoseEstimator.html) を管理しているため、自前のエスティメーターを作成する代わりに [`SwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html) 内の [`SwerveDrive.addVisionMeasurement`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#addVisionMeasurement(edu.wpi.first.math.geometry.Pose2d,double)) でビジョン計測を処理できます。

## ビジョンオドメトリの使い方

YAGSL-Example には、ダミーのビジョン計測値を使ったサンプルがあります。基本的には [`SwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html) クラスをポーズエスティメーターとして使用します。

<pre class="language-java"><code class="lang-java">/**
 * テスト用にダミーのビジョン読み取り値を追加する。
 */
public void addFakeVisionReading()
{
<strong>  swerveDrive.addVisionMeasurement(new Pose2d(3, 3, Rotation2d.fromDegrees(65)), Timer.getFPGATimestamp());
</strong>}
</code></pre>

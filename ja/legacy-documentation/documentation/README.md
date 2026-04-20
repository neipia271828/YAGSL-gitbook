---
hidden: true
---

# ドキュメント

## 機能とリソース

* 機能とリソースはディスカッションページの[こちら](https://github.com/BroncBotz3481/YAGSL-Example/discussions)にあります。https://github.com/BroncBotz3481/YAGSL-Example/discussions

## スワーブドライブを作成するには？

YAGSLの特徴は、JSON設定ファイルだけでスワーブドライブを作成できる点です。JSON設定ファイルは [`deploy`](https://github.com/BroncBotz3481/YAGSL-Example/src/main/deploy) ディレクトリに配置してください。Configurationオブジェクトを手動で作成してSwerveDriveをインスタンス化することもできます。

### JSONを使ってSwerveDriveを作成する方法

このサンプルプログラムは [`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/frc/robot/subsystems/swervedrive2/SwerveSubsystem.java) 内で [`SwerveDrive`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/SwerveDrive.java) を作成します。コマンドベースプログラミングを使用している場合、SwerveSubsystem内でのみ操作してください。

```java
import java.io.File;
import edu.wpi.first.wpilibj.Filesystem;
import swervelib.parser.SwerveParser;
import swervelib.SwerveDrive;
import edu.wpi.first.math.util.Units;


double maximumSpeed = Units.feetToMeters(4.5)
File swerveJsonDirectory = new File(Filesystem.getDeployDirectory(),"swerve");
SwerveDrive  swerveDrive = new SwerveParser(directory).createSwerveDrive(maximumSpeed);

```

この方法は速くて簡単で、大きくてメンテナンスしにくい定数ファイルを気にする必要がなくなります！JSONディレクトリの作成方法はJSONドキュメントを参照してください。

## スワーブドライブの作成

* [ ] NavX、ReduxLib、CTRE、REVのベンダー依存関係をインストールする。
* [ ] YAGSLをインストールする（`https://broncbotz3481.github.io/YAGSL-Lib/yagsl/yagsl.json`）
* [ ] [サンプルJSONディレクトリ](https://github.com/BroncBotz3481/YAGSL/tree/main/deploy)をコピーするか自分で作成して `src/main/frc/deploy/swerve` に配置するか、[こちら](https://broncbotz3481.github.io/YAGSL-Example)から生成する。
* [ ] JSONディレクトリからSwerveDriveを作成する: `SwerveDrive drive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve")).createSwerveDrive(maximumSpeed);`
* [ ] [docs/index.html](https://broncbotz3481.github.io/YAGSL/) のJavadocsを参照する。
* [ ] 実験する！

## 古い設定ファイルの移行

1. `physicalproperties.json` から `wheelDiamter`、`gearRatio`、`encoderPulsePerRotation` を削除する。
2. `physicalproperties.json` に `optimalVoltage` を追加する。
3. `swervedrive.json` から `maxSpeed` と `optimalVoltage` を削除する。
4. スワーブモジュールのドライブモーターまたはステアリングモーターが他のモジュールと異なる場合、そのモジュールの設定JSONファイルにドライブとステアリング両方の `conversionFactor` を**必ず**指定してください。一方のモーターが他のモジュールと同じで、その `conversionFactor` を使用したい場合は、モジュールJSONの `conversionFactor` を0に設定してください。
5. `new SwerveParser(directory).createSwerveDrive(maximumSpeed);` でSwerveDriveを作成する際に最大速度を必ず指定してください。
6. `swervedrive.json` に `conversionFactor` を設定したくない場合は、以下のようにコンストラクターのパラメーターとして渡せます。

```java
double DriveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(WHEEL_DIAMETER), GEAR_RATIO, ENCODER_RESOLUTION);
double SteeringConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(GEAR_RATIO, ENCODER_RESOLUTION);
new SwerveParser(directory).createSwerveDrive(maximumSpeed, SteeringConversionFactor, DriveConversionFactor);
```

## 貢献

このライブラリは95チームの [SwervyBot コード](https://github.com/first95/FRC2023/tree/main/SwervyBot) をはじめ、多様なコードベースを参考にしています。スワーブコードをオープンソース化してくださったすべてのチームに心より感謝申し上げます！

---
description: ドリフトはスワーブドライブのあらゆる要因から発生します...
---

# スワーブドライブのドリフト

スワーブドライブのドリフトは長年にわたる問題であり、その原因についても多くの議論がなされてきました。このページでは、スワーブドライブのドリフトの一般的な原因を3つのカテゴリに分けて説明します。ロボットにはこれらの問題が複数同時に存在する可能性があることを念頭に置いてください。

## ハードウェア

### 配線

配線の問題はスワーブドライブにおいてさらに3つのカテゴリに分けられます。配線が劣化している、誤ったルーティングされている、またはどこかで断線している（特にコネクター！）場合の症状です。

#### エンコーダー

* [ ] テレメトリーの読み取りが不規則で意味不明になる。
* [ ] モーターが制御不能に回転する場合がある。
* [ ] ドライバーステーションのコンソールにエラーが表示される場合がある。

#### モーターコントローラー

* [ ] テレメトリーの更新が遅くなる場合がある。
* [ ] モーターが突然停止したり制御不能に回転したりする場合がある。
* [ ] モーターがコマンドに応答しない場合がある。
* [ ] ドライバーステーションのコンソールにエラーが表示される場合がある。

#### ジャイロスコープ/IMU

* [ ] テレメトリーの更新が遅くなる場合がある。
* [ ] フィールド相対のロボット制御が使用不能になる場合がある。
* [ ] 自律走行が制御不能にドリフトする。
* [ ] ドライバーステーションにエラーが表示される場合がある。
* [ ] ジャイロスコープが激しく揺れたり振動したりする場合。
* [ ] 移動中にジャイロスコープがズレた場合。

### 物理部品

#### マグネティックエンコーダー

* [ ] マグネットがスワーブモジュールに固定されておらず、動作中にずれる。
  * マグネットが固定されるまでアブソリュートエンコーダーオフセットの再調整が継続的に必要になる。
* [ ] マグネティックエンコーダーがスワーブモジュールに固定されていない。

#### モーター

* [ ] モーターがスワーブモジュールのシャフトにしっかり接続されていない。
  * 電流制限が設定されている場合、モーターが動作中に「ぴくぴく」する。
* [ ] モーターが劣化している。
  * 同じ速度を出すために必要な電流が増加している場合がある。

#### スワーブモジュール

* [ ] ホイールの位置がずれている場合。
* [ ] 同じPIDで各モジュールの動きが一定でない。
  * ギアにグリスを塗布する必要がある場合がある。

#### ロボット

* [ ] ロボットの重心が適切でない。

## ソフトウェア

* [ ] コードの他の部分による継続的な `Command Scheduler loop overruns`。
* [ ] 最大物理速度が正しく設定されていない。
* [ ] オープンループを使用している場合、モジュールが異なる速度で動作する。
* [ ] コントローラー入力のフィルタリング。
* [ ] [`SwerveDrive.setCosineCompensator`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setCosineCompensator(boolean)) の使用。
* [ ] [`SwerveDrive.setHeadingCorrection`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setHeadingCorrection(boolean)) の使用。
* [ ] [`SwerveModule.setAntiJitter`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html#setAntiJitter(boolean)) の使用。
* [ ] [`SwerveDrive.chassisVelocityCorrection`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#chassisVelocityCorrection) の使用。
* [ ] [`SwerveDrive.setMaximumSpeeds`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setMaximumAllowableSpeeds(double,double)) の使用。
* [ ] [`SwerveDrive.replaceSwerveModuleFeedforward`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#replaceSwerveModuleFeedforward(edu.wpi.first.math.controller.SimpleMotorFeedforward)) の使用。
* [ ] [`SwerveDrive.setGyroOffset`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setGyroOffset(edu.wpi.first.math.geometry.Rotation3d)) の使用。
* [ ] [`SwerveDrive.updateCacheValidityPeriods`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#updateCacheValidityPeriods(long,long,long)) の使用。
* [ ] [`SwerveDrive.setOdometryPeriod`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#setOdometryPeriod(double)) の使用。
* [ ] [`SwerveDrive.addVisionMeasurement`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#addVisionMeasurement(edu.wpi.first.math.geometry.Pose2d,double)) の使用。
* [ ] `SwerveDrive.setChassisDiscretization` の使用。

### PathPlanner

* [ ] PathPlanner の `AutoBuilder` のPIDが適切に調整されていない。
* [ ] モジュールの最大速度が高すぎるまたは低すぎる。
* [ ] テストパスがカーブしている。
* [ ] PathPlannerのオートが初期ポーズを設定していない。
* [ ] PathPlannerのパスが開始時にオドメトリーをリセットしていない。

## 設定

* [ ] モジュールの位置が正しく定義されていない。
  * ロボット中心からモジュール中心までの距離であることを忘れずに！
* [ ] スワーブモジュール設定でアブソリュートエンコーダー、角度モーター、ドライブモーターの定義が正しくない。
* [ ] スワーブモジュール設定ファイルで定義されているコンバージョンファクターが `physicalproperties.json` のものと異なる。
  * スワーブモジュールファイルのコンバージョンファクターは完全に**任意**であり、特別な理由がない限り定義する必要はありません。冗長性を避けるためにコードか `physicalproperties.json` のみで定義することをお勧めします。
* [ ] コンバージョンファクターが正しくない。
  * PathPlannerでスワーブドライブが常に目標より遠くまで進む場合、これが原因の可能性があります。
  * 角度モーターがモジュールを正しく位置合わせできない場合も原因になる可能性があります。
* [ ] ドライブおよびステアリング/アジマス/角度モーターのPIDが十分に調整されていない。
* [ ] 電流制限が低すぎる。
* [ ] [ロボットの向きに基づく並進軸の変化。](the-eight-steps.md)

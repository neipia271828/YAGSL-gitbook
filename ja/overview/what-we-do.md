---
description: YAGSLがスワーブドライブを動かします！
---

# YAGSLとは

## プログラムにおけるYAGSLの位置づけ

YAGSLは本質的に1つのクラス [`SwerveDrive`](https://broncbotz3481.github.io/YAGSL-Lib/docs/swervelib/SwerveDrive.html) に集約されており、[WPILib](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/wpi-drive-classes.html) の [`DifferentialDrive`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/drive/DifferentialDrive.html) と同様の役割をスワーブドライブ向けに果たすことを目指しています。

<figure><img src="../../.gitbook/assets/yagsl.png" alt="created by DeltaDizzy"><figcaption><p>ドライブサブシステムと <code>SwerveDrive</code> の関係を示す図（DeltaDizzy 作成）</p></figcaption></figure>

## このガイドの目標

* `SwerveDrive` と `SwerveModule` の基礎を理解し、必要であれば自分でプログラムを作成したり YAGSL を改変できるようにする。
* サンプルをもとに YAGSL プロジェクトのセットアップ手順を案内する。
* このガイドを通じて、以下の機能を持つプログラムが完成する。
  * 自律制御（PathPlanner + 統合コマンドを使用）
  * スワーブドライブの手動操作コード

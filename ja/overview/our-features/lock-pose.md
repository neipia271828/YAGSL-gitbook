---
description: YAGSLにはそのためのヘルパーがあります！
---

# ロックポーズ

## ロックポーズとは？

ロックポーズは、ロボットを特定の位置に固定し、ほぼ動かせない状態にしたいときに使う特殊な機能です。すべてのホイールが内向きに X 字型を向きます。これは**他に入力がない場合にのみ**使用してください。そうしないと、予期しない破壊的な動作が発生する可能性があります。

## ロックポーズの使い方

コントローラー入力を渡す代わりに、[`SwerveDrive.lockPose()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#lockPose()) を繰り返し呼び出してください。

<pre class="language-java"><code class="lang-java">public class RobotContainer
{

  // 必要に応じて CommandPS4Controller または CommandJoystick に変更してください
  final CommandXboxController driverXbox = new CommandXboxController(0);
  // ロボットのサブシステムとコマンドをここで定義します...
  private final SwerveSubsystem drivebase = new SwerveSubsystem(new File(Filesystem.getDeployDirectory(),
                                                                         "swerve/neo"));

  /**
   * ロボットのコンテナ。サブシステム、OI デバイス、コマンドを管理します。
   */
  public RobotContainer()
  {
    // トリガーバインディングを設定する
    configureBindings();

    Command driveFieldOrientedDirectAngleSim = drivebase.simDriveCommand(
        () -> MathUtil.applyDeadband(driverXbox.getLeftY(), OperatorConstants.LEFT_Y_DEADBAND),
        () -> MathUtil.applyDeadband(driverXbox.getLeftX(), OperatorConstants.LEFT_X_DEADBAND),
        () -> driverXbox.getRawAxis(2));

    drivebase.setDefaultCommand(driveFieldOrientedDirectAngleSim);
  }

  /**
   * トリガーとコマンドのマッピングを定義するメソッド。
   */
  private void configureBindings()
  {
<strong>    driverXbox.x().whileTrue(Commands.runOnce(drivebase::lock, drivebase).repeatedly());
</strong>  }

  /**
   * 自律コマンドをメインの {@link Robot} クラスに渡すメソッド。
   *
   * @return 自律モードで実行するコマンド
   */
  public Command getAutonomousCommand()
  {
    // サンプルとして自律コマンドを実行します
    return drivebase.getAutonomousCommand("New Auto");
  }

  public void setDriveMode()
  {
    //drivebase.setDefaultCommand();
  }

  public void setMotorBrake(boolean brake)
  {
    drivebase.setMotorBrake(brake);
  }
}

</code></pre>

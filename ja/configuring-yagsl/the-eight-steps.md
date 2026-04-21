# 8つのステップ

## なんとなく動いているが何かがおかしい？

反転状態を変えても他に試したことが効果なく、問題が解決しない場合、このようなセットアップのデバッグは苦痛で長い作業になります。以下の8つのステップは、残念ながらデバッグに必要なものです。

_「ロボットの向きに基づく並進軸の変化」_（フィールド指向モードでロボットが回転するとXとYの軸が変わる）とも呼ばれるこの問題は、途中のどこかのステップで解決するはずです。

## ロボットコードを準備する

これらのテストを素早く効果的に行うために、以下または類似のコードをドライブコードとして使用してください。ジョイスティックへの移植や軸データの符号反転が必要な場合もありますが、これで問題のデバッグが速くなります。

{% code title="RobotContainer.java" %}
```java
...
    SwerveSubsystem drivebase;
    CommandXboxController driverXbox;
...
    // Applies deadbands and inverts controls because joysticks
    // are back-right positive while robot
    // controls are front-left positive
    // left stick controls translation
    // right stick controls the desired angle NOT angular rotation
    Command driveFieldOrientedDirectAngle = drivebase.driveCommand(
        () -> MathUtil.applyDeadband(driverXbox.getLeftY(), OperatorConstants.LEFT_Y_DEADBAND),
        () -> MathUtil.applyDeadband(driverXbox.getLeftX(), OperatorConstants.LEFT_X_DEADBAND),
        () -> driverXbox.getRightX(),
        () -> driverXbox.getRightY());
        
    drivebase.setDefaultCommand(driveFieldOrientedDirectAngle);
....
```
{% endcode %}

<pre class="language-java" data-title="SwerveSubsystem.java"><code class="lang-java">...
public class SwerveSubsytem extends SubsystemBase
{
...
  SwerveDrive swerveDrive;
  /**
   * Command to drive the robot using translative values and heading as a setpoint.
   *
   * @param translationX Translation in the X direction. Cubed for smoother controls.
   * @param translationY Translation in the Y direction. Cubed for smoother controls.
   * @param headingX     Heading X to calculate angle of the joystick.
   * @param headingY     Heading Y to calculate angle of the joystick.
   * @return Drive command.
   */
  public Command driveCommand(DoubleSupplier translationX, DoubleSupplier translationY, DoubleSupplier headingX,
                              DoubleSupplier headingY)
  {
    // swerveDrive.setHeadingCorrection(true); // Normally you would want heading correction for this kind of control.
    return run(() -> {

      Translation2d scaledInputs = SwerveMath.scaleTranslation(new Translation2d(translationX.getAsDouble(),
                                                                                 translationY.getAsDouble()), 0.8);

      // Make the robot move
<strong>      driveFieldOriented(swerveDrive.swerveController.getTargetSpeeds(scaledInputs.getX(), scaledInputs.getY(),
</strong><strong>                                                                      headingX.getAsDouble(),
</strong><strong>                                                                      headingY.getAsDouble(),
</strong><strong>                                                                      swerveDrive.getOdometryHeading().getRadians(),
</strong><strong>                                                                      swerveDrive.getMaximumVelocity()));
</strong>    });
  }
  
  /**
   * Drive the robot given a chassis field oriented velocity.
   *
   * @param velocity Velocity according to the field.
   */
  public void driveFieldOriented(ChassisSpeeds velocity)
  {
    swerveDrive.driveFieldOriented(velocity);
  }
...
</code></pre>

このスニペットはYAGSL-Exampleプロジェクトから直接引用したものです。そのプロジェクトを使用している場合は、`SwerveSubsystem` のデフォルトコマンドとして `driveFieldOrientedDirectAngle` を使用し、ジョイスティックの反転を適宜修正するだけで大丈夫です。

## 8つのステップ

{% hint style="danger" %}
**これらのステップは危険です！**

6つはロボットが制御不能に回転します。

1つは正しい状態です。

1つは正しいように見えますが、ロボットの向きに基づいて並進軸が変化します。
{% endhint %}

{% hint style="warning" %}
すべてのステップを完了することが求められているわけではありません。途中のどこかのステップで問題が解決するはずです。
{% endhint %}

1. `swervedrive.json` の `invertIMU` を `false` に設定し、**かつ**モジュールJSONのドライブの `invert` を `false` に設定する。
2. 次に `invertIMU` を `true` に設定する。
3. 次にモジュールJSONのすべてのドライブモーターを `true` に設定して反転させる。
4. 次に `invertIMU` を `false` に設定する。
5.  次に[モジュールを入れ替える](#user-content-fn-1)[^1]。\\

    <figure><img src="../.gitbook/assets/image-48.png" alt=""><figcaption><p>正しいモジュールの入れ替え方の図解</p></figcaption></figure>
6. 次にすべてのドライブモーターを `false` に設定して反転を解除する。
7. 次に `invertIMU` を `true` に設定する。
8. 次にドライブモーターの反転状態を `true` に設定する。

{% hint style="danger" %}
これらのステップがすべて効果がない場合は、ハードウェアの設定が正しくないか、何かが期待通りに動作していないか、配線が正しくない可能性が高いです。
{% endhint %}

## 動作の確認

8つのステップが完了していないように見えても、実際には完了している場合があります。以下はロボットが実際には正しくチューニングされているが、希望する前面が実際にはロボットの後ろ側になっている例です。

<details>

<summary>モジュール設定を入れ替える方法</summary>

例では番号を使って説明しますが、モジュールファイルを入れ替える際は番号を元のモジュール設定名に対応させます。上記の例では以下のようになります。

1. `frontleft.json`
2. `frontright.json`
3. `backleft.json`
4. `backright.json`

**`frontleft.json` と `backright.json` を入れ替える**

<pre class="language-json" data-title="frontleft.json"><code class="lang-json">{
<strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 4,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 3,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 9,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": -114.609,
</strong>  "location": {
    "front": 12,
    "left": 12
  }
}
</code></pre>

<pre class="language-json" data-title="backright.json"><code class="lang-json"><strong>{
</strong><strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 5,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 6,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 11,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": -18.281,
</strong>  "location": {
    "front": -12,
    "left": -12
  }
}
</code></pre>

ハイライトされた行を入れ替えると、モジュール設定が正しく入れ替わります。

#### 簡単な方法

1. locationの符号を目的のモジュール側に変更する。
2. ファイルを上書きせずにリネームする。

</details>

[^1]: ドライブモーターコントローラー、角度モーターコントローラー、絶対エンコーダーのデバイス設定を以下のように変更します。\
    \
    (前左 → 後右)\
    (前右 → 後左)\
    (後左 → 前右)\
    (後右 → 前左)

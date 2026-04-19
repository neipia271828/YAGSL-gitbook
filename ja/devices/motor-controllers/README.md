# モーターコントローラー

YAGSLはほとんどの一般的なFRCモーターコントローラー（[Venom](https://www.playingwithfusion.com/productview.php?pdid=99\&catid=1014) を除く）をサポートしています。可能な場合は統合エンコーダー付きのブラシレスモーターの使用を推奨していますが、SparkMAX **のみ**でブラシ付きモーターと外部エンコーダーの組み合わせもサポートしています。

統合エンコーダーの値はドライバーダッシュボードの `swerve/modules/.../Raw Motor Encoder` にエンコーダーの直接値として表示されます。

## モーターチェックリスト

* [ ] すべてのモーターが反時計回りを正方向とする。そうでない場合は必要な反転を記録する。
* [ ] PIDを最大限に調整する。
* [ ] モーターを最新のファームウェアに更新する。
* [ ] モーターがユニークなCAN IDを持つ。
* [ ] モータータイプが設定されている。
* [ ] 指定したCAN IDがモジュールJSONのものと対応している。
* [ ] すべてのベベルが同じ方向を向いた状態でホイールが前方に揃えられている。

## スワーブモーターラッパー

YAGSLはスワーブドライブの動作に必要なデータを均一に取得・設定するために、サポートするすべてのモーターコントローラーのラッパーを作成しています。このラッパーは `SwerveMotor` と呼ばれます。すべての `SwerveMotor` は `SwerveModule` の設定オブジェクト `SwerveModuleConfiguration` のモーター定義 `angleMotor` および `driveMotor` から取得できます。`SwerveModule` は `SwerveDrive.getModules()` で簡単に取得できます。

<pre class="language-java" data-full-width="true"><code class="lang-java"> /**
   * Initialize {@link SwerveDrive} with the directory provided.
   *
   * @param directory Directory of swerve drive config files.
   */
  public SwerveSubsystem(File directory)
  {
    // Angle conversion factor is 360 / (GEAR RATIO * ENCODER RESOLUTION)
    //  In this case the gear ratio is 12.8 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double angleConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(12.8, 1);
    // Motor conversion factor is (PI * WHEEL DIAMETER IN METERS) / (GEAR RATIO * ENCODER RESOLUTION).
    //  In this case the wheel diameter is 4 inches, which must be converted to meters to get meters/second.
    //  The gear ratio is 6.75 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double driveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(4), 6.75, 1);

    // Configure the Telemetry before creating the SwerveDrive to avoid unnecessary objects being created.
    SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
    try
    {
      swerveDrive = new SwerveParser(directory).createSwerveDrive(maximumSpeed, angleConversionFactor, driveConversionFactor);
    } catch (Exception e)
    {
      throw new RuntimeException(e);
    }
    swerveDrive.setHeadingCorrection(false); // Heading correction should only be used while controlling the robot via angle.

    for(SwerveModule m : swerveDrive.getModules())
    {
      System.out.println("Module Name: "+m.configuration.name);
      CANSparkMax steeringMotor = (<a data-footnote-ref href="#user-content-fn-1">CANSparkMax</a>)m.getAngleMotor().getMotor(); 
      CANSparkMax driveMotor = (<a data-footnote-ref href="#user-content-fn-1">CANSparkMax</a>)m.getDriveMotor().getMotor(); 
    }
  }
</code></pre>

## モーターコントローラーの設定

{% hint style="warning" %}
現在、`canbus` オプションをサポートしているのはCTREデバイスのみです。デバイスがroboRIOの `canbus` を使用している場合は、サポートされているCTREデバイスに対して `null` または `"rio"` を使用してください。CANivoreを使用しており、デバイスがCANivoreバス上にある場合は、名前がCANivoreの名前と一致している必要があります。
{% endhint %}

`frontleft.json`、`frontright.json`、`backleft.json`、`backright.json` などのモジュールJSONファイルでは、モーターコントローラーを以下のように設定します。

<pre class="language-json"><code class="lang-json">{
  "drive": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-2">"sparkmax"</a>,
</strong><strong>    "id": <a data-footnote-ref href="#user-content-fn-3">5</a>,
</strong><strong>    "canbus": <a data-footnote-ref href="#user-content-fn-4">null</a>
</strong>  },
  "angle": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-2">"sparkmax"</a>,
</strong><strong>    "id": <a data-footnote-ref href="#user-content-fn-5">6</a>,
</strong><strong>    "canbus": <a data-footnote-ref href="#user-content-fn-4">null</a>
</strong>  },
  "encoder": {
    "type": "cancoder",
    "id": 11,
    "canbus": null
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-6">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-7">false</a>
  },
  "absoluteEncoderOffset": -18.281,
  "location": {
    "front": <a data-footnote-ref href="#user-content-fn-8">-12</a>,
    "left": <a data-footnote-ref href="#user-content-fn-9">-12</a>
  }
}
</code></pre>

## 使用可能なモーターコントローラータイプ

| デバイス | type |
| --- | --- |
| [SparkMAX](sparkmax.md) | `sparkmax_neo`；`sparkmax_neo550`；`sparkmax_vortex`；`sparkmax_minion`；`sparkmax_pulsar` |
| [SparkFlex](sparkflex.md) | `sparkflex_minion`；`sparkflex_pulsar`；`sparkflex_neo`；`sparkflex_vortex` |
| [TalonFX](talonfx.md) | `talonfx`（`krakenx60` と同じ）；`falcon500`；`falcon500foc`；`krakenx60`；`krakenx60foc` |
| TalonSRX | `talonsrx` |
| SparkMAX ブラシ付き | `sparkmax_brushed` |

{% hint style="warning" %}
現時点では `sparkmax_brushed` や `talonsrx` のようなブラシ付きモーターの使用に関するドキュメントは提供していません。
{% endhint %}

[^1]: `SwerveMotor` がラップするモーターコントローラーオブジェクトを元のクラス（この場合は `CANSparkMax`）にキャストします。

[^2]: SparkMAXのブラシレスモードを選択。

[^3]: SparkMAXのCAN IDは `5`。

[^4]: SparkMAXはCANivoreと互換性がないため、`canbus` は `null` または `""` にしてください。

[^5]: SparkMAXのCAN IDは `6`。

[^6]: ドライブモーターは反転なしで反時計回り正方向に回転。

[^7]: ステアリング/角度/アジマスモーターは反転なしで反時計回り正方向に回転。

[^8]: このモジュールの中心はロボット中心から「前方向」に `-12` インチ。

[^9]: このモジュールの中心はロボット中心から「左方向」に `-12` インチ。

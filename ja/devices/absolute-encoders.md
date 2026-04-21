# 絶対エンコーダー

YAGSLはFRCで一般的に使用されているほとんどの絶対エンコーダーをサポートしています。ブラシ付きモーターを使用する場合は絶対エンコーダーが必要です。

絶対エンコーダーの値はドライバーダッシュボードの `swerve/modules/.../Raw Absolute Encoder` に表示され、ほとんどの場合モジュールJSON設定ファイルの `absoluteEncoderOffset` の調整に使用できます。

## 絶対エンコーダーチェックリスト

* [ ] すべての絶対エンコーダーが反時計回りを正方向とする。
* [ ] マグネティック絶対エンコーダーが静止時および動作中（ロボット移動中）にマグネットを良好に読み取れている。
* [ ] 絶対エンコーダーのCAN IDまたはアナログ入力チャンネルがユニークである。
* [ ] 絶対エンコーダーが正しいIDまたはアナログ入力チャンネルで定義されている。
* [ ] 絶対エンコーダーが全範囲（`0`〜`360`）を持っている。

## スワーブ絶対エンコーダーラッパー

YAGSLはスワーブモジュールの動作に必要なデータを均一に取得・設定するために、サポートするすべての絶対エンコーダーのラッパーを作成しています。このラッパーは [`SwerveAbsoluteEncoder`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/encoders/SwerveAbsoluteEncoder.html) と呼ばれます。すべての [`SwerveAbsoluteEncoder`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/encoders/SwerveAbsoluteEncoder.html) は、[`SwerveModule`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html#configuration) の設定オブジェクト [`SwerveModuleConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html) の絶対エンコーダー属性 [`absoluteEncoder`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html#absoluteEncoder) から取得できます。[`SwerveModule`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html) は [`SwerveDrive.getModules()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#getModules()) で簡単に取得できます。

YAGSLはスワーブドライブの動作に必要なデータを均一に取得・設定するために、サポートするすべてのモーターコントローラーのラッパーも作成しています。このラッパーは [`SwerveMotor`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/motors/SwerveMotor.html) と呼ばれます。すべての [`SwerveMotor`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/motors/SwerveMotor.html) は、[`SwerveModule`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html#configuration) の設定オブジェクト [`SwerveModuleConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html) のモーター定義 [`angleMotor`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html#angleMotor) および [`driveMotor`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveModuleConfiguration.html#driveMotor) から取得できます。[`SwerveModule`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveModule.html) は [`SwerveDrive.getModules()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#getModules()) で簡単に取得できます。

<pre class="language-java"><code class="lang-java">import com.ctre.phoenix6.hardware.CANcoder;
 
   /**
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
      <a data-footnote-ref href="#user-content-fn-1">CANcoder</a> absoluteEncoder = (<a data-footnote-ref href="#user-content-fn-2">CANcoder</a>)m.configuration.absoluteEncoder.getAbsoluteEncoder();
    }
  }
</code></pre>

## 絶対エンコーダーの設定

{% hint style="warning" %}
現在、`canbus` オプションをサポートしているのはCTREデバイスのみです。デバイスがroboRIOの `canbus` を使用している場合は、サポートされているCTREデバイスに対して `null` または `"rio"` を使用してください。CANivoreを使用しており、デバイスがCANivoreバス上にある場合は、名前がCANivoreの名前と一致している必要があります。
{% endhint %}

{% hint style="success" %}
絶対エンコーダーがSparkMAXに接続されている場合は、最高のパフォーマンスのために [`SwerveDrive.pushOffsetsToEncoders()`](https://broncbotz3481.github.io/YAGSL-Lib/docs/swervelib/SwerveDrive.html#pushOffsetsToEncoders\(\)) を使用してください。これにより、オンボードPIDセンサーがアタッチされたエンコーダーに設定されます！
{% endhint %}

`frontleft.json`、`frontright.json`、`backleft.json`、`backright.json` などのモジュールJSONファイルでは、絶対エンコーダーを以下のように設定します。

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": <a data-footnote-ref href="#user-content-fn-3">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-4">5</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-5">null</a>
  },
  "angle": {
    "type": <a data-footnote-ref href="#user-content-fn-6">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-7">6</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-8">null</a>
  },
  "encoder": {
    "type": <a data-footnote-ref href="#user-content-fn-9">"cancoder"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-10">11</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-11">null</a>
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-12">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-13">false</a>
  },
  "absoluteEncoderOffset": <a data-footnote-ref href="#user-content-fn-14">-18.281</a>,
  "absoluteEncoderInverted": <a data-footnote-ref href="#user-content-fn-15">false</a>,
  "location": {
    "front": <a data-footnote-ref href="#user-content-fn-16">-12</a>,
    "left": <a data-footnote-ref href="#user-content-fn-17">-12</a>
  }
}
</code></pre>

## 使用可能な絶対エンコーダータイプ

{% hint style="warning" %}
モジュールが回転し続ける場合は、ステアリング/角度/アジマスモーターを反転させてみてください。
{% endhint %}

<table data-full-width="true"><thead><tr><th width="538">デバイス</th><th width="269">type</th></tr></thead><tbody><tr><td>なし</td><td><code>none</code></td></tr><tr><td><a href="https://docs.revrobotics.com/brushless/spark-max/encoders/absolute#data-port-connector-information">Integrated/Attached</a>（SparkMAX DutyCycle経由）</td><td><a data-footnote-ref href="#user-content-fn-18"><code>attached</code></a></td></tr><tr><td><a href="https://docs.revrobotics.com/brushless/spark-max/encoders/absolute#data-port-connector-information">SparkMax Analog</a>（SparkMAXアナログピン経由）</td><td><a data-footnote-ref href="#user-content-fn-19"><code>sparkmax_analog</code></a></td></tr><tr><td><a href="https://docs.revrobotics.com/brushless/spark-max/encoders/absolute#data-port-connector-information">SparkMax Analog</a>（5V電源付きSparkMAXアナログピン経由）</td><td><a data-footnote-ref href="#user-content-fn-20"><code>sparkmax_analog5v</code></a></td></tr><tr><td><a href="https://docs.reduxrobotics.com/canandmag/spark-max#using-the-pwm-output-with-spark-max">Canandmag</a>（SparkMAX経由）</td><td><a data-footnote-ref href="#user-content-fn-21"><code>canandmag</code></a></td></tr><tr><td><a href="https://docs.reduxrobotics.com/canandmag/getting-started">Canandmag</a>（CAN経由）</td><td><a data-footnote-ref href="#user-content-fn-22"><code>canandmag_can</code></a></td></tr><tr><td><a href="https://pro.docs.ctr-electronics.com/en/latest/docs/hardware-reference/cancoder/index.html">CANcoder</a></td><td><a data-footnote-ref href="#user-content-fn-23"><code>cancoder</code></a></td></tr><tr><td><a href="https://www.revrobotics.com/rev-11-1271/">Throughbore</a>（DIO経由）</td><td><a data-footnote-ref href="#user-content-fn-24"><code>throughbore</code></a></td></tr><tr><td><a href="https://www.thethriftybot.com/products/thrifty-absolute-magnetic-encoder">Thrifty Absolute Magnetic Encoder</a>（アナログ入力経由）</td><td><a data-footnote-ref href="#user-content-fn-25"><code>thrifty</code></a></td></tr><tr><td><a href="https://www.andymark.com/products/ma3-absolute-encoder-with-cable">MA3</a>（アナログ入力経由）</td><td><a data-footnote-ref href="#user-content-fn-26"><code>ma3</code></a></td></tr><tr><td><a href="https://store.ctr-electronics.com/srx-mag-encoder/">SRX Mag</a>（PWM経由）</td><td><a data-footnote-ref href="#user-content-fn-27"><code>ctre_mag</code></a></td></tr><tr><td><a href="https://www.andymark.com/products/am-mag-encoder">AM Mag</a>（PWM経由）</td><td><a data-footnote-ref href="#user-content-fn-28"><code>am_mag</code></a></td></tr><tr><td>PWM DutyCycle</td><td><code>dutycycle</code></td></tr><tr><td>アナログエンコーダー</td><td><code>analog</code></td></tr></tbody></table>

[^1]: Phoenix 6の [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html)。CANバスとCANIDから設定で初期化。

[^2]: Phoenix 6の [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html)。ObjectをCANcoderにキャスト。

[^3]: SparkMAXのブラシレスモードを選択。

[^4]: SparkMAXのCAN IDは `5`。

[^5]: SparkMAXはCANivoreと互換性がないため、 `canbus` は `null` または `""` にしてください。

[^6]: SparkMAXのブラシレスモードを選択。

[^7]: SparkMAXのCAN IDは `6`。

[^8]: SparkMAXはCANivoreと互換性がないため、 `canbus` は `null` または `""` にしてください。

[^9]: 以下のCANIDとバスを使用して [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) を選択。

[^10]: このモジュールの [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) のID。

[^11]: [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) が存在するCANivoreの名前。

[^12]: ドライブモーターは反転なしで反時計回り正方向に回転。

[^13]: ステアリング/角度/アジマスモーターは反転なしで反時計回り正方向に回転。

[^14]: ホイールを直進方向に向けてベベルを同じ方向に揃えたときに `Module[...] Raw Absolute Encoder` から読み取ったオフセット。単位は度。

[^15]: これが `true` になることはほとんどありません。

[^16]: このモジュールの中心はロボット中心から「前方向」に `-12` インチ。

[^17]: このモジュールの中心はロボット中心から「左方向」に `-12` インチ。

[^18]: ```json
    "encoder": {
        "type": "attached",
        "id": 11,
        "canbus": null
      },
    ```

[^19]: ```json
    "encoder": {
        "type": "sparkmax_analog",
        "id": 11,
        "canbus": null
      },
    ```

[^20]: ```json
    "encoder": {
        "type": "sparkmax_analog5v",
        "id": 11,
        "canbus": null
      },
    ```

[^21]: ```json
    "encoder": {
        "type": "canandmag",
        "id": 11,
        "canbus": null
      },
    ```

[^22]: ```json
    "encoder": {
        "type": "canandmag_can",
        "id": 11,
        "canbus": null
      },
    ```

[^23]: ```json
    "encoder": {
        "type": "cancoder",
        "id": 11,
        "canbus": null
      },
    ```

[^24]: ```json
    "encoder": {
        "type": "throughbore",
        "id": 11,
        "canbus": null
      },
    ```

[^25]: ```json
    "encoder": {
        "type": "thrifty",
        "id": 11,
        "canbus": null
      },
    ```

[^26]: ```json
    "encoder": {
        "type": "ma3",
        "id": 11,
        "canbus": null
      },
    ```

[^27]: ```json
    "encoder": {
        "type": "ctre_mag",
        "id": 11,
        "canbus": null
      },
    ```

[^28]: ```json
    "encoder": {
        "type": "am_mag",
        "id": 11,
        "canbus": null
      },
    ```

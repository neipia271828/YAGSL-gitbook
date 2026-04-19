# ジャイロスコープ

YAGSLはあらゆる予算のチームをサポートするため、幅広いジャイロスコープに対応しています。NavXとPigeon2を推奨し、公式にサポートおよびテスト済みですが、他のジャイロスコープも様々な程度でサポートを試みています。

## ジャイロスコープチェックリスト

* [ ] [反時計回りに回転させるとジャイロスコープの読み取り値が増加する](#user-content-fn-1)[^1]。
* [ ] ヨーの読み取り値がロボットの向きである。
* [ ] ジャイロスコープの `0` が希望するロボットの「前面」である。

## スワーブIMUラッパー

YAGSLはスワーブドライブの動作に必要なデータを均一に取得・設定するために、サポートするすべてのジャイロスコープタイプのラッパーを作成しています。このラッパーは [`SwerveIMU`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/imu/SwerveIMU.html) と呼ばれます。すべてのジャイロスコープは、`swervedrive.json` ファイルから生成された [`SwerveDriveConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveDriveConfiguration.html) オブジェクトを通じて [`SwerveDrive`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#swerveDriveConfiguration) オブジェクトから取得できます。ユーザープログラムで生のジャイロスコープオブジェクトを取得するには、以下のようにオブジェクトを適切な型にキャストするだけです（この例ではNavXの `AHRS` クラス）。

<pre class="language-java"><code class="lang-java">   /**
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

<strong>    <a data-footnote-ref href="#user-content-fn-2">AHRS</a> navx = (<a data-footnote-ref href="#user-content-fn-3">AHRS</a>)swerveDrive.getGyro().getIMU();
</strong>
  }
</code></pre>

## 推奨ジャイロスコープ

これらのジャイロスコープは十分にテストされており、多くのFRCチームに使用されています。一般的に、最高のパフォーマンスと信頼性を提供し、コミュニティからのサポートも豊富です。

### Studica製 [NavX2 MXP](gyroscope/navx.md)

{% embed url="https://www.studica.ca/en/navx-2-mxp-robotics-navigation-sensor" %}

## CTRE製 [Pigeon2 IMU](gyroscope/pigeon-2.0.md)

{% embed url="https://store.ctr-electronics.com/pigeon-2/" %}

## ジャイロスコープの設定

{% hint style="warning" %}
現在、`canbus` オプションをサポートしているのはCTREデバイスのみです。デバイスがroboRIOの `canbus` を使用している場合は、サポートされているCTREデバイスに対して `null` または `"rio"` を使用してください。CANivoreを使用しており、デバイスがCANivoreバス上にある場合は、名前がCANivoreの名前と一致している必要があります。
{% endhint %}

`swervedrive.json` でジャイロスコープを以下のように指定します。

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": "<a data-footnote-ref href="#user-content-fn-4">pigeon2</a>",
</strong><strong>    <a data-footnote-ref href="#user-content-fn-5">"id"</a>: <a data-footnote-ref href="#user-content-fn-6">13</a>,
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">"canbus"</a>: "<a data-footnote-ref href="#user-content-fn-8">canivore</a>"
</strong>  },
  "invertedIMU": true,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

{% hint style="warning" %}
コントローラー入力なしでロボットが制御不能に回転する場合は、IMUを反転させる必要があります。
{% endhint %}

### 使用可能なジャイロスコープタイプ

| デバイス | type | 通信方式 |
| --- | --- | --- |
| [Pigeon](gyroscope/pigeon.md) | `pigeon` | CAN；CANivore非対応 |
| [Pigeon2](gyroscope/pigeon-2.0.md) | `pigeon2` | CAN；CANivore対応 |
| [Canandgyro](https://docs.reduxrobotics.com/canandgyro/getting-started) | `canandgyro` | CAN；CANivore非対応 |
| [NavX](gyroscope/navx.md) | `navx`、`navx_spi` | roboRIO MXP SPI |
| [NavX](gyroscope/navx.md) | `navx_i2c` | roboRIO MXPのI2Cポート（非推奨！） |
| [NavX](gyroscope/navx.md) | `navx_usb` | USB経由のシリアル通信（非推奨） |
| [NavX](gyroscope/navx.md) | `navx_mxp_serial` | roboRIO MXP経由のシリアル |
| [NavX3](gyroscope/navx3.md) | `navx3` | CAN |
| [ADIS16448](gyroscope/adis16448.md) | `adis16448` | roboRIO MXP |
| [ADIS16470](gyroscope/adis16470.md) | `adis16470` | roboRIO SPIポート |
| [ADXRS450](gyroscope/adxrs450.md) | `adxrs450` | roboRIO SPIポート |
| アナログジャイロ | `analog` | AnalogInput |

[^1]: 反時計回り正方向（CCW+）とも呼ばれます。

[^2]: NavXをJavaで表現・通信するための NavX `AHRS` クラス。

[^3]: JavaのObjectを `AHRS` クラスにキャストします。ジャイロスコープのネイティブクラスに合わせて変更してください。

[^4]: `pigeon2` IMUデバイスを選択してインスタンス化します。

[^5]: この場合はCAN IDを指しますが、選択したタイプによってはroboRIOのAnalogInputを指す場合もあります。

[^6]: Pigeon 2のCAN IDは13です。

[^7]: CANバスは常に使用されるわけではなく、デフォルト（roboRIO）のCANバス（通常は `"rio"`）を選択するために `null` または `""` にすることができます。

[^8]: デバイスがそのCANバス上にある場合はCANivoreの名前と一致させます。roboRIO CANバスの場合は `"rio"` または `null` または `""` を使用してください。

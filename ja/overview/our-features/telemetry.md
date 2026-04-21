# テレメトリ

## 仕組み

YAGSLは `SwerveDrive` と `SwerveModule` でテレメトリを一元管理しています。テレメトリの一部は [`SwerveDrive.updateOdometry()`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/SwerveDrive.html#updateOdometry()) から送信されます。すべてのテレメトリは、静的変数 [`SwerveDriveTelemetry.verbosity`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.html#verbosity) の詳細度レベルによって制御されます。以下が選択可能なオプションです。

```java
/**
 * 送信するテレメトリデータの詳細度。
 */
 public enum TelemetryVerbosity
{
  /**
   * テレメトリデータを送信しない。
   */
  NONE,
  /**
   * 低詳細度。フィールド上のロボット位置のみを送信。
   */
  LOW,
  /**
   * 中詳細度。スワーブドライブの基本情報。
   */
  INFO,
  /**
   * INFO レベル + フィールド情報。
   */
  POSE,
  /**
   * 全スワーブドライブデータを人間可読・機械可読の両形式で送信。
   */
  HIGH,
  /**
   * スワーブドライブに関する機械可読データのみを送信。
   */
  MACHINE
}
```

## 有効にする方法

{% hint style="warning" %}
詳細度が高いほどロボットに負荷がかかり、サイクルタイムが遅くなる場合があります。設定には注意してください！
{% endhint %}

テレメトリを設定するには、[`SwerveDriveTelemetry.verbosity`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.html#verbosity) を希望する [enum 値](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/telemetry/SwerveDriveTelemetry.TelemetryVerbosity.html) に変更してください。

<pre class="language-java"><code class="lang-java">  /**
   * 指定されたディレクトリで {@link SwerveDrive} を初期化する。
   *
   * @param directory スワーブドライブ設定ファイルのディレクトリ。
   */
  public SwerveSubsystem(File directory)
  {
    // 不要なオブジェクトの生成を避けるため、SwerveDrive 作成前にテレメトリを設定する。
<strong>    SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
</strong>    swerveDrive = new SwerveParser(directory).createSwerveDrive(Constants.MAX_SPEED);
  }
</code></pre>

## テレメトリの内容

テレメトリは roboRIO の NetworkTables にスワーブドライブの関連データを出力します。任意のダッシュボードで確認できます！

一部のダッシュボードは `SmartDashboard/swerve` NetworkTable エントリをもとにスワーブドライブウィジェットをサポートしています。

各スワーブモジュールのデータは `SmartDashboard/swerve/modules/` 以下の対応するモジュール名に格納されており、セットアップ時に非常に役立ちます。

### スワーブモジュール

<figure><img src="../../../.gitbook/assets/image (3).png" alt="Shuffleboard Tree"><figcaption><p>Shuffleboard でのスワーブモジュールテレメトリ</p></figcaption></figure>

### スワーブドライブ

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Shuffleboard でのスワーブドライブテレメトリ（関連キーを丸で囲んでいます）</p></figcaption></figure>

## 推奨 Shuffleboard レイアウト

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>左側に Raw Absolute Encoder グループ、右側に Raw Angle Encoder グループ。</p></figcaption></figure>

セットアップ時にすべてのモジュールが CCW+（反時計回りで増加）であることを確認するために、このレイアウトを使用しています。スワーブドライブの設定でよく見落とされる部分です。

**Raw Angle Encoder** が CCW-（反時計回りで減少）の場合、そのモジュールの設定ファイルで角度モーターを反転させる必要があります。

**Raw Absolute Encoder** が CCW-（ホイールを反時計回りにしたときに減少）の場合、そのモジュールの絶対エンコーダーを反転させる必要がある可能性があります。

## 対応ダッシュボード

YAGSL ウィジェットをサポートするダッシュボードもあります！開発者の皆さんの素晴らしい成果をぜひチェックしてください！

* [Elastic Dashboard](https://github.com/Gold872/elastic-dashboard)
* [FRC Web Components](https://github.com/frc-web-components/app/releases/latest)
* [Advantage Scope](https://github.com/Mechanical-Advantage/AdvantageScope)

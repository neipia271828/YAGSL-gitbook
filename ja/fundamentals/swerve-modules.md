---
description: スワーブモジュールとは何か？
---

# スワーブモジュール

他チームのコードにある `SwerveModule` を表すクラスが様々で、なぜ標準クラスがないのか不思議に思うかもしれません。理由は明確で、モーター、アブソリュートエンコーダー、ギア比、取り付け方がすべて異なりうるからです。多くのチームはデバッグ時間を多く取れたかどうかによってスワーブドライブの問題に対処しています。YAGSL はすべての一般的な問題を考慮し、JSON 設定ファイルで対処することを目指しています。

{% hint style="warning" %}
磁気エンコーダーを使用している場合は、動作中にずれないよう磁石が正しく接着されていることを確認してください。
{% endhint %}

## スワーブモジュールの構成要素

* [ ] ドライブギア（ギア比が必要）
* [ ] ステアリングギア（ギア比が必要）
* [ ] ドライブモーター（＋コントローラー）
* [ ] 角度/アジマス/ステアリングモーター（＋コントローラー）
* [ ] アブソリュートエンコーダー

## 確認事項

スワーブドライブのデバッグ時にすぐ役立つ内容です！

スマートモーターコントローラーには通常以下の機能があります：

* [ ] どちらの方向にも回転できる
* [ ] どちらの方向でも読み取れるモーターセンサーを持つ
* [ ] 詰まったり動けなかったりすると過電流で燃える
* [ ] 動作中の電圧レベルを低下させる
* [ ] 摩擦に打ち勝つ最低電圧が必要
* [ ] シャフトに接続されたグリス付きギア
* [ ] 瞬間的な過電力を避けるため設定可能なレートで加速する
* [ ] 接続されたセンサー（通常はエンコーダー）の入力に基づいて出力を制御する統合 PID ループを持つ
* [ ] デフォルトでは `rio` の CAN バスに接続されるが、[CANivore](https://store.ctr-electronics.com/canivore/) が接続されている場合はそのモーターコントローラーがサポートしていれば CANivore の名前になる
* [ ] ギアに比例してシャフト 1 回転以上ホイールを回す

スワーブモジュールを正しく設定するにはこれらすべてを正しく設定する必要があります。1 つでも誤っていると、簡単には特定できない問題が発生することがあります。

#### TL;DR

1. モーターは様々な原因で壊れ、1 つの方法でのみ動作することが期待されます。デバッグ時はここを参照してください。
2. スワーブモジュールは**ドライブギア**、**ステアリングギア**、**ドライブモーター**、**ステアリングモーター**、**アブソリュートエンコーダー**で構成されます。

## チェックリスト

* [ ] ステアリング/アジマス/角度モーターが[アブソリュートエンコーダー](#user-content-fn-1)[^1]の値に合わせて増加する
* [ ] ステアリング/アジマス/角度モーターが反時計回りで増加する
* [ ] ドライブモーターが増加するとロボットが「前進」する
* [ ] アブソリュートエンコーダーがスワーブモジュールにしっかり固定されている
* [ ] コンバージョンファクターが正しく計算されている
* [ ] すべてのホイールが同じ方向を向いた状態でアブソリュートエンコーダーのオフセットが設定されている

{% hint style="warning" %}
アブソリュートエンコーダーのオフセットを取得するには、ホイールのベベルが同じ方向を向くよう揃えてください。
{% endhint %}

## 概要

スワーブモジュールはすぐに複雑になります。このサンプルはシンプルに保ち、将来拡張するかもしれません。上記のすべてのステップは含みませんが、コンストラクターにスペースを設けます。このチュートリアルでは、より冗長に書ける REVLib の [`CANSparkMax`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkmax) を使用します。

## 基本

REVLib を使ってコード上でモジュールを表すスワーブモジュールクラスを作成します。

```java
import com.revrobotics.CANSparkMax;
import com.ctre.phoenix6.hardware.CANcoder;

public class SwerveModule {

    private CANSparkMax driveMotor;
    private CANSparkMax steerMotor;
    private CANcoder    absoluteEncoder;
    
    public SwerveModule(int driveMotorCANID, int steerMotorCANID, int cancoderCANID)
    {
        driveMotor = new CANSparkMax(driveMotorCANID);
        steerMotor = new CANSparkMax(steerMotorCANID);
        absoluteEncoder = new CANcoder(cancoderCANID);
        
        // すべてをファクトリーデフォルトにリセット
        driveMotor.restoreFactoryDefaults();
        steerMotor.restoreFactoryDefaults();
        absoluteEncoder.getConfigurator().apply(new CANcoderConfiguration());
        
        // ここで設定を続ける...
    }
}
```

このクラスはドライブモーター、ステアリングモーター、アブソリュートエンコーダーを実装するだけです。ハードウェアクライアントですべての設定が完了している場合は、ファクトリーデフォルトへのリセットは不要ですが、推奨はしません。

## 設定

スワーブモジュールの設定は**非常に重要**です！根気が必要で、最初の試みで動作することはまずないでしょう。ここで必要な手順を説明します。

### アブソリュートエンコーダーのオフセット

メカニカルチームが非常に精密に作業して、すべてのモジュールでマグネットを完全に中央に配置しない限り、アブソリュートエンコーダーがホイールの向きを正しく読み取れるよう各モジュールのオフセットを設定する必要があります。

ハードウェアクライアントでこの値を取得することができますし、しない場合は以下のような簡単なプログラムを書いてアブソリュートエンコーダーの現在値を表示することもできます。

{% hint style="warning" %}
アブソリュートエンコーダーのオフセットはモジュールが向くべき方向を決めます。起動時またはまっすぐ進むよう命令した後にモジュールがまっすぐ前を向かない場合、アブソリュートエンコーダーのオフセットに**問題があります**。

オフセットがわずかにずれているだけでモジュールが引きずられ、**ペナルティにつながる可能性があります**。
{% endhint %}

```java
package frc.robot;

import com.ctre.phoenix6.hardware.CANcoder;
import com.ctre.phoenix6.StatusSignal;
import com.ctre.phoenix6.signals.AbsoluteSensorRangeValue;
import com.ctre.phoenix6.signals.SensorDirectionValue;
import com.ctre.phoenix6.configs.CANcoderConfiguration;
import com.ctre.phoenix6.configs.CANcoderConfigurator;
import com.ctre.phoenix6.configs.MagnetSensorConfigs;
import edu.wpi.first.math.util.Units;

public class Robot extends TimedRobot
{
  private static Robot   instance;
  private CANcoder    absoluteEncoder;

  public Robot() { instance = this; }
  public static Robot getInstance() { return instance; }

  @Override
  public void robotInit()
  {
   absoluteEncoder = new CANcoder(/* CANcoder の CAN ID に変更 */ 0);
   CANcoderConfigurator cfg = encoder.getConfigurator();
   cfg.apply(new CANcoderConfiguration());
   MagnetSensorConfigs magnetSensorConfiguration = new MagnetSensorConfigs();
   cfg.refresh(magnetSensorConfiguration);
   cfg.apply(magnetSensorConfiguration
                  .withAbsoluteSensorRange(AbsoluteSensorRangeValue.Unsigned_0To1)
                  .withSensorDirection(SensorDirectionValue.CounterClockwise_Positive));
  }

  @Override
  public void robotPeriodic()
  {
   StatusSignal<Double> angle = encoder.getAbsolutePosition().waitForUpdate(0.1);
   System.out.println("アブソリュートエンコーダー角度（度）: " + Units.rotationsToDegrees(angle.getValue()));
  }
}
```

### 反転

スワーブモジュールによっては、モーターを反転させる必要があります。

{% hint style="warning" %}
ステアリング/角度/アジマスモーターコントローラーの反転状態が誤っていると、スワーブモジュールは入力が与えられたとき、場合によっては休止中でも**制御不能に回転します**。
{% endhint %}

### コンバージョンファクター

数学の時間です！次元解析を覚えていますか？

{% embed url="https://youtu.be/hIAdCTNi1S8" %}
単位換算のための次元解析（カーンアカデミー）
{% endembed %}

スワーブモジュールには速度（メートル毎秒）とモジュールの角度（ここでは簡単のため度）を設定するための [`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) オブジェクトが与えられます。つまり、ネイティブ単位（この場合は回転数と RPM）を速度と角度の単位に変換する必要があります。また、1 回のホイール回転に必要なモーターのギア比もわかっています。

ステアリングコンバージョンファクターは、ロータの**回転数**（または SparkMAX のデータポートに接続されている場合はアブソリュートエンコーダーの回転数）を**度**に変換します。

この例では、ギア比を `12.8:1`（[SDS MK4 ステアリング比](https://www.swervedrivespecialties.com/collections/kits/products/mk4-swerve-module)）と仮定します。ロータは 1 回のメカニズム回転に `12.8` 回転します。

$$
SteeringConverionFactor = \frac{1_{degree}}{1_{rot}} = \frac{1_{rot}}{12.8_{rot}} * \frac{1_{rot}}{360_{deg}}
$$

ドライブコンバージョンファクターは、モーターエンコーダーから得られる**回転数/分**を**メートル/秒**に変換します。

次の例では、ギア比 `6.75:1`（[SDS MK4 L2 ドライブ比](https://www.swervedrivespecialties.com/collections/kits/products/mk4-swerve-module)）を使用します。ホイールが 1 回転するためにロータは `6.75` 回転します。

$$
DriveConversionFactor = \frac{\frac{1_{meter}}{1_{sec}}}{\frac{1_{rot}}{1_{min}}} = \frac{1_{rot}}{1_{min}} * \frac{1_{rot}}{6.75_{rot}} * \frac{60_{sec}}{1_{min}} * \frac{\pi*d_{meters}}{1_{rot}}
$$

{% hint style="warning" %}
YAGSL のドライブコンバージョンファクターは、以下の例のように RPM から MPS ではなく、回転数からメートルに変換します。
{% endhint %}

これはすべて、数学が重要であり、コンバージョンファクターは魔法の数字ではないことを示すためです！

{% hint style="warning" %}
コンバージョンファクターは非常に重要で、誤っているとモーターが**制御不能に回転したり**、オドメトリが常にわずかにずれて運転中の動作が誇張されたりする可能性があります。
{% endhint %}

### PID 制御

PID は Proportional-Integral-Derivative（比例・積分・微分）の略です。スワーブドライブは最新のフィードバックセンサーを使おうとするため、SparkMAX のオンボード PID 機能を使用します。

WPILib には PID を学ぶための優れたガイドがあります。タレット位置制御の例が、ステアリングモーターの制御方法です。

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/introduction-to-pid.html" %}

REV のドキュメントもあり、SparkMAX コントローラーでの動作（おおむね）を示しています。

{% embed url="https://docs.revrobotics.com/sparkmax/operating-modes/closed-loop-control" %}

スワーブモジュールを制御するには 2 つの PID が必要です。1 つはドライブモーター用、もう 1 つはステアリングモーター用です。

#### ドライブモーター PID

WPILib ドキュメントの例に基づき、ここに記載されているフライホイールとして調整するだけです。

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-flywheel.html" %}

#### ステアリングモーター PID

ステアリングモーター PID はフィードバックセンサーに基づいてホイールの角度を度単位で制御します。

1. PID ラッピングにより、ホイールは常に目標角度への最短経路を選びます。
2. ギアには頻繁にグリスを！！！
3. 正しいコンバージョンファクターを計算する。
4. ハードウェアクライアントまたは Tuner X で素早く正確に調整する。

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-turret.html" %}

### 電流制限

ブラウンアウトを防ぐため、モーターの電流を制限する必要があります。通常はステアリングモーターで 20A、ドライブモーターで 40A が目安です。

{% hint style="warning" %}
SparkMAX はステーター制限のみ適用できます。
{% endhint %}

{% embed url="https://v6.docs.ctr-electronics.com/en/stable/docs/hardware-reference/talonfx/improving-performance-with-current-limits.html" %}

[^1]: これはコードでモーターコントローラーを反転させることで修正できます。

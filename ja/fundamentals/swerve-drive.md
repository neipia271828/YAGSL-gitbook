---
description: スワーブドライブとは何か？
---

# スワーブドライブ

<figure><img src="../../.gitbook/assets/sim_sample.png" alt=""><figcaption><p>YAGSL のスワーブドライブシミュレーション</p></figcaption></figure>

## スワーブドライブ製作時のヒント

* ジャイロスコープをロボットの中心に設置すると、わずかな軌道のずれを防ぎ、より正確なオドメトリを実現できます。
* 磁石を使用している場合は、正しくしっかりと接着してください！
* 時間に余裕をもちましょう。モジュールを 1 つ組み立てミスすること、または競技会中にスペアが必要になることを想定しておきましょう。
* スワーブドライブのプログラミングは難しく、YAGSL が簡単にしようとしていても、正しく理解するためには多くのことを知る必要があります！
* 適切なツールを使いましょう！テキストだけでのデバッグは十分難しいので、[AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope/tree/main) や [FRC Web Components](https://github.com/frc-web-components/app/releases) などの優れた可視化ツールを備えたダッシュボードを試してみてください！

## 基本

スワーブドライブは、各ホイールを特定の角度（アジマス）に向け、その方向へホイールを回転させることで移動します。スワーブドライブの特徴は、並進移動とは独立して回転できることです。つまり、どの方向を向いていてもあらゆる方向に移動できます。これにより「その場旋回」や移動しながらの回転が可能です。ロボットの回転方向は**ヘディング**と呼ばれます。

## スワーブドライブとは？

スワーブドライブは通常 4 つのスワーブモジュール（ドライブモーター、角度/アジマスモーター、絶対エンコーダーから構成）と、中央に設置されたジャイロスコープで構成されます。モーター、絶対エンコーダー、ジャイロスコープの種類は問わず、さまざまな組み合わせが可能ですが、動作の安定性はさまざまです。可能であれば 1 つのシステムに統一することをお勧めします（すべて REV、またはすべて CTRE）。統一することで最良の機能セットが得られますが、同等ではありません！その他のユースケースでは YAGSL が最良の選択です。YAGSL はすべてのセンサーとモーターコントローラーを機能的に等価にするための抽象化を念頭に置いて開発されたからです。

#### TL;DR

スワーブドライブの構成要素:

* [ ] ジャイロスコープ
* [ ] スワーブモジュール
  * [ ] 角度/アジマスモーター（＋コントローラー）
  * [ ] ドライブモーター
  * [ ] 絶対エンコーダー

#### TL;DR 問題を起こす原因

* [ ] 重心が悪い
* [ ] ジャイロスコープが中心にない
* [ ] 正方形でないドライブトレイン

これは完全なリストではなく、随時追加されます。

## コード上でのスワーブドライブの動作

スワーブドライブは、目標の方向と向きから決まる角度に各モジュールを移動させます。FRC では、手動でロボットのキネマティクスを計算するか、モジュールの位置から [`ChassisSpeeds`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/ChassisSpeeds.html) オブジェクトを受け取り各ホイールの回転速度と速さを計算する [`SwerveDriveKinematics`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveKinematics.html) を使います。結果として [`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) 配列が返され、各スワーブモジュールの角度と速度を設定するために使用します。

### `SwerveDriveKinematics`

<pre class="language-java" data-title="SwerveDrive.java" data-line-numbers data-full-width="false"><code class="lang-java">// 関連クラスをインポート
import edu.wpi.first.math.kinematics.SwerveDriveKinematics;
import edu.wpi.first.math.geometry.Translation2d;
import edu.wpi.first.math.util.Units;

// SwerveDrive クラスのサンプル
public class SwerveDrive {

    // フィールド
<strong>    SwerveDriveKinematics kinematics;
</strong>    
    // コンストラクター
    public SwerveDrive() {
        // SwerveDriveKinematics オブジェクトを生成
        // ロボット中心からホイール中心まで 12.5in
        // 12.5in はメートルに変換して使用
        // Translation2d(x,y) == Translation2d(前, 左)
<strong>        kinematics = new SwerveDriveKinematics(
</strong><strong>            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(12.5)), // 前左
</strong><strong>            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(-12.5)), // 前右
</strong><strong>            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(12.5)), // 後左
</strong><strong>            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(-12.5))  // 後右
</strong><strong>        );
</strong>    }
}
</code></pre>

{% hint style="info" %}
順序によって、モジュールの角度と速度の出力順が決まります！
{% endhint %}

### `SwerveModuleState`

[`SwerveDriveKinematics`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveKinematics.html) は指定した順序で各モジュールの [`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) を生成します。以下の例では `ChassisSpeeds` オブジェクトを受け取って `drive()` 関数内でこれを行う方法を示します。

[`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) にはスワーブモジュールの特性を反映する `angle` と `speedMetersPerSecond` の 2 つのプロパティがあります。取得後の目標は、[`SwerveDriveKinematics`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveKinematics.html) 生成時に指定した順序に対応する正しいスワーブモジュールに、[`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) の角度と速度を設定することです。

<pre class="language-java" data-title="SwerveDrive.java" data-line-numbers data-full-width="false"><code class="lang-java">import edu.wpi.first.math.kinematics.SwerveDriveKinematics;
import edu.wpi.first.math.geometry.Translation2d;
import edu.wpi.first.math.util.Units;
import edu.wpi.first.math.kinematics.SwerveModuleState;

public class SwerveDrive 
{
    SwerveDriveKinematics kinematics;
    
    public SwerveDrive() 
    {
        kinematics = new SwerveDriveKinematics(
            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(12.5)),
            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(-12.5)),
            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(12.5)),
            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(-12.5))
        );
    }
    
    public void drive()
    {
<strong>        // X=14in, Y=4in で移動し、30deg/s で回転するテスト用 ChassisSpeeds
</strong><strong>        ChassisSpeeds testSpeeds = new ChassisSpeeds(Units.inchesToMeters(14), Units.inchesToMeters(4), Units.degreesToRadians(30));
</strong>        
<strong>        SwerveModuleState[] swerveModuleStates = kinematics.toSwerveModuleStates(testSpeeds);
</strong><strong>        // 出力順: 前左、前右、後左、後右
</strong>    }
}
</code></pre>

{% hint style="danger" %}
スワーブドライブのコードは、モジュールの位置と角度を追跡する [`SwerveDriveOdometry`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveDriveOdometry.html) または [`SwerveDrivePoseEstimator`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/estimator/SwerveDrivePoseEstimator.html) なしでは機能しません！
{% endhint %}

### `SwerveDriveOdometry`

残念ながら、それほど簡単ではありません。ロボットの現在位置を継続的に追跡する必要があります。具体的には**ヘディング**、**速度**、**モジュール位置**で、これらをまとめて**オドメトリ**と呼びます。これが、使用可能な [`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) を正しく生成する唯一の方法です。

<pre class="language-java" data-title="SwerveDrive.java" data-line-numbers data-full-width="false"><code class="lang-java">import edu.wpi.first.math.kinematics.SwerveDriveKinematics;
import edu.wpi.first.math.geometry.Translation2d;
import edu.wpi.first.math.util.Units;
import edu.wpi.first.math.kinematics.SwerveModuleState;
import edu.wpi.first.math.kinematics.SwerveModulePosition;
import edu.wpi.first.math.kinematics.SwerveDriveOdometry;
import edu.wpi.first.math.geometry.Pose2d;
import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class SwerveDrive extends SubsystemBase
{
    SwerveDriveKinematics kinematics;
<strong>    SwerveDriveOdometry   odometry;
</strong><strong>    Gyroscope             gyro; // ジャイロスコープを表す擬似クラス
</strong><strong>    SwerveModule[]        swerveModules; // スワーブモジュールを表す擬似クラス
</strong>    
    public SwerveDrive() 
    {
<strong>        swerveModules = new SwerveModule[4]; // 擬似コード。ここでスワーブモジュールを生成する。
</strong>        
        kinematics = new SwerveDriveKinematics(
            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(12.5)),
            new Translation2d(Units.inchesToMeters(12.5), Units.inchesToMeters(-12.5)),
            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(12.5)),
            new Translation2d(Units.inchesToMeters(-12.5), Units.inchesToMeters(-12.5))
        );
        
<strong>        gyro = new Gyroscope(); // ジャイロスコープの擬似コンストラクター
</strong>
<strong>        // 現在の角度で SwerveDriveOdometry を生成。ロボットは x=0, y=0, heading=0 にいる
</strong><strong>        odometry = new SwerveDriveOdometry(
</strong><strong>            kinematics,
</strong><strong>            gyro.getAngle(),
</strong><strong>            new SwerveModulePosition[]{new SwerveModulePosition(), new SwerveModulePosition(), new SwerveModulePosition(), new SwerveModulePosition},
</strong><strong>            new Pose2d(0,0,new Rotation2d())
</strong>        );
    }
    
    public void drive()
    {
        ChassisSpeeds testSpeeds = new ChassisSpeeds(Units.inchesToMeters(14), Units.inchesToMeters(4), Units.degreesToRadians(30));
        SwerveModuleState[] swerveModuleStates = kinematics.toSwerveModuleStates(testSpeeds);
        
<strong>        swerveModules[0].setState(swerveModuleStates[0]);
</strong><strong>        swerveModules[1].setState(swerveModuleStates[1]);
</strong><strong>        swerveModules[2].setState(swerveModuleStates[2]);
</strong><strong>        swerveModules[3].setState(swerveModuleStates[3]);
</strong>    }
    
    public SwerveModulePosition[] getCurrentSwerveModulePositions()
    {
<strong>        return new SwerveModulePosition[]{
</strong><strong>            new SwerveModulePosition(swerveModules[0].getDistance(), swerveModules[0].getAngle()),
</strong><strong>            new SwerveModulePosition(swerveModules[1].getDistance(), swerveModules[1].getAngle()),
</strong><strong>            new SwerveModulePosition(swerveModules[2].getDistance(), swerveModules[2].getAngle()),
</strong><strong>            new SwerveModulePosition(swerveModules[3].getDistance(), swerveModules[3].getAngle())
</strong><strong>        };
</strong>    }
    
    @Override
    public void periodic()
    {
<strong>        // 毎回オドメトリを更新する
</strong><strong>        odometry.update(gyro.getAngle(), getCurrentSwerveModulePositions());
</strong>    }
}
</code></pre>

{% hint style="info" %}
`SwerveDriveOdometry` は `SwerveDrivePoseEstimator` に置き換えられます。
{% endhint %}

スワーブドライブのオドメトリは、サブシステムの `periodic` 関数と同様に毎回更新する必要があります。

## まとめ

このページで扱っていないスワーブドライブの細かい点は他にも多くありますが、スワーブドライブのプログラム方法の基本的な理解としては十分です。できるだけ多くのサンプルを参照して注意点を理解するか、YAGSL の使用を続けることを強くお勧めします。頑張ってください！

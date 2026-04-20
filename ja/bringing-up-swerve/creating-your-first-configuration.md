# はじめての設定

## DevilBotz 2876 スワーブ立ち上げチェックリスト

[DevilBotz 2876](https://www.thebluealliance.com/team/2876/2024) 提供！

更新日: 2024-02-03

## 印刷可能バージョン

{% file src="../.gitbook/assets/2024-02-03 DevilBotz 2876 Swerve Bring-Up Checklist.pdf" %}

## リソース

* [YAGSL Wiki](https://docs.yagsl.com/) - [https://yagsl.gitbook.io/yagsl/](https://docs.yagsl.com/)
* [REV Robotics Hardware Client 2](https://docs.revrobotics.com/rev-hardware-client-2)
  * _Spark Max モーターコントローラーおよび他の REV デバイスの設定用_
* [Phoenix Tuner X](https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/index.html)
  * _CanCoder および他の CTR デバイスの設定用_

## スワーブ向き図

{% hint style="warning" %}
_注意: 上から見たとき、ベベルギアのある側のホイールが**左**を向いていることを確認してください。_
{% endhint %}

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

### ステップ 1: モジュールタイプ

<table data-header-hidden data-full-width="true"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td></td><td><strong>モデル、バージョンなど</strong></td></tr><tr><td><em>モーター</em></td><td></td></tr><tr><td><em>コントローラー</em></td><td></td></tr><tr><td><em>アブソリュートエンコーダー</em></td><td></td></tr><tr><td><em>IMU</em></td><td></td></tr></tbody></table>

### ステップ 2: ビルド固有の詳細

1. **ロボット中心**に対するモジュール中心の位置を測定する

<table data-full-width="true"><thead><tr><th>モジュール</th><th>X「前」（インチ）</th><th>Y「左」（インチ）</th></tr></thead><tbody><tr><td><em>前左 (FL)</em></td><td>+</td><td>+</td></tr><tr><td><em>前右 (FR)</em></td><td>+</td><td>-</td></tr><tr><td><em>後左 (BL)</em></td><td>-</td><td>+</td></tr><tr><td><em>後右 (BR)</em></td><td>-</td><td>-</td></tr></tbody></table>

2. ホイール直径を_インチ_で測定する
3. [内部エンコーダーの分解能を確認する](#user-content-fn-1)[^1]
   * _注意: 最近のほとんどのエンコーダーは `-1` から `1` に正規化された値を報告するため、コンバージョンファクター計算時のエンコーダー分解能は通常 "`1`" です。唯一の例外として知られるのは TalonSRX です。_
4. スワーブモジュールメーカーの仕様からドライブ/角度ギア比を確認する
5. （任意）[ドライブ/角度コンバージョンファクターを計算する](#user-content-fn-2)[^2]
   * ドライブモーターコンバージョンファクター（メートル/回転） = (PI \* ホイール直径（メートル）) / (ギア比 \* エンコーダー分解能)
   * 角度モーターコンバージョンファクター（度/回転） = 360 / (ギア比 \* エンコーダー分解能)

<table data-full-width="true"><thead><tr><th>モーター</th><th align="center">ホイール直径（メートル）</th><th>ギア比</th><th align="center">エンコーダー分解能（CPR）</th><th>コンバージョンファクター</th></tr></thead><tbody><tr><td><em>ドライブ</em></td><td align="center"></td><td></td><td align="center">1</td><td></td></tr><tr><td><em>角度</em></td><td align="center"><strong>N/A</strong></td><td></td><td align="center">1</td><td></td></tr></tbody></table>

### ステップ 3: 電気的特性

6. 各モジュールの CAN ID を設定/確認する

{% hint style="warning" %}
_注意: 各モジュールの FW を更新し、保存された設定をファクトリーデフォルトにリセットしてください。_
{% endhint %}

<table data-full-width="true"><thead><tr><th>モジュール</th><th>モーター CAN ID</th><th>モーター CAN ID</th><th>エンコーダー CAN/チャンネル ID</th></tr></thead><tbody><tr><td></td><td><strong>ドライブ</strong></td><td><strong>角度</strong></td><td><strong>アブソリュートエンコーダー</strong></td></tr><tr><td><em>前左 (FL)</em></td><td></td><td></td><td></td></tr><tr><td><em>前右 (FR)</em></td><td></td><td></td><td></td></tr><tr><td><em>後左 (BL)</em></td><td></td><td></td><td></td></tr><tr><td><em>後右 (BR)</em></td><td></td><td></td><td></td></tr></tbody></table>

7. 反転を確認する
   1. _ドライブ_ホイールを **CCW**（「前進」方向）に回転させる
      * 内蔵エンコーダーの値が**増加**するべき。しない場合はドライブモーターを_反転_させる。
   2. _角度_ホイールを**CCW**（上から見て反時計回り）に回転させる
      * 内蔵エンコーダーの値が**増加**するべき。しない場合は角度モーターを_反転_させる。
      * アブソリュートエンコーダーの値が**増加**するべき。しない場合はアブソリュートエンコーダーを_反転_させる。
   3. ロボット全体を**CCW**に回転させる。ジャイロの角度（ヨー）が**増加**するべき。しない場合は IMU を_反転_させる。

{% hint style="warning" %}
_注意: ハードウェアユーティリティでモーターコントローラーやアブソリュートエンコーダーにアクセスしている場合、roboRIO は CAN バス上でアクティブであってはなりません。CAN BUS の終端に影響を与えずに roboRIO を確実に無効化する最も信頼性の高い方法は、Power Distribution Panel (PDP) の roboRIO に電力を供給している 10A ヒューズを一時的に抜いてから電源を再投入することです。_
{% endhint %}

<table data-full-width="true"><thead><tr><th>モジュール</th><th>反転？</th><th></th><th></th><th></th></tr></thead><tbody><tr><td></td><td><strong>ドライブ</strong></td><td><strong>角度</strong></td><td><strong>アブソリュートエンコーダー</strong></td><td><strong>IMU</strong></td></tr><tr><td><em>前左 (FL)</em></td><td></td><td></td><td></td><td></td></tr><tr><td><em>前右 (FR)</em></td><td></td><td></td><td></td><td></td></tr><tr><td><em>後左 (BL)</em></td><td></td><td></td><td></td><td></td></tr><tr><td><em>後右 (BR)</em></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### ステップ 4: アブソリュートエンコーダーのオフセット

8. ロボットの電源を入れる（無効状態でホイールを手動で回せるように）
9. ドライブエンコーダーの値が増加する方向に前進するよう、4 つのホイールすべてが前を向くよう手動で回転させる（[向き図](creating-your-first-configuration.md#swerve-orientation-diagram-1)の黒い矢印を参照）
10. 各モジュールのアブソリュートエンコーダーの値を記録する

<table data-full-width="true"><thead><tr><th>モジュール</th><th>角度アブソリュートオフセット（度）</th><th></th></tr></thead><tbody><tr><td><em>前左 (FL)</em></td><td></td><td></td></tr><tr><td><em>前右 (FR)</em></td><td></td><td></td></tr><tr><td><em>後左 (BL)</em></td><td></td><td></td></tr><tr><td><em>後右 (BR)</em></td><td></td><td></td></tr></tbody></table>

### ステップ 5: 設定ウェブページにデータを入力する

ウェブページを開いて設定ファイルにデータを入力してください。\
[https://yet-another-software-suite.github.io/YAGSL/config_generator/](https://yet-another-software-suite.github.io/YAGSL/config_generator/)

[^1]: SparkMAX または TalonFX を使用している場合、これは `1` です！

[^2]: モーターコントローラーの回転数をメートル/度に変換します。

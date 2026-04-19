---
description: >-
  Advantage Scopeは、Team 6328 Mechanical Advantageが提供するデータビジュアライゼーションツールで、
  スワーブドライブをビジュアライズしてデバッグのフィードバックを提供します。
---

# Advantage Scope

## 起動

2024シーズン以降、[Advantage Scope](https://github.com/Mechanical-Advantage/AdvantageScope) はWPILibのインストールに含まれています。外部からのダウンロードは不要ですが、WPILibのアップデートのたびに[WPILibインストーラー](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html)を再ダウンロードして最新バージョンのWPILibツールを入手してください。

## Advantage Scopeの設定

1. ノートパソコンをロボットに接続する。
2. `AdvantageScope (WPILib)` を開くか、VS Codeでコマンドパレットを開いて `WPILib: Start Tool` と入力し、`AdvantageScope` をクリックする。
3. `Help` をクリックし、`Show Preferences` をクリックする。

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences.png" alt=""><figcaption><p>Advantage Scopeのhelpメニュー</p></figcaption></figure>

4. チーム番号に基づいてroboRIOのIPアドレスを入力する。`10.TE.AM.2`

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences-IP.png" alt=""><figcaption><p>roboRIOアドレスフィールドのハイライト</p></figcaption></figure>

5. ロボット（またはシミュレーター）に接続する。

<figure><img src="../.gitbook/assets/AdvantageScope-Connect.png" alt=""><figcaption><p>Robotへの接続メニュー</p></figcaption></figure>

6. ウィンドウ右側の `+` をクリックして新しいタブを追加する。

<figure><img src="../.gitbook/assets/AdvantageScope-Add.png" alt=""><figcaption><p>新しいタブを追加</p></figcaption></figure>

7. 新しい :crab: Swerveタブを追加する。

<figure><img src="../.gitbook/assets/AdvantageScope-Swerve.png" alt=""><figcaption><p>Swerveタブ</p></figcaption></figure>

8.  SmartDashboardからFieldsにモジュール状態と回転を接続する。

    1. `SmartDashboard/swerve` メニューの `advantagescope/` 内のすべてを `Sources` フィールドにドラッグする。
    2. `SmartDashboard/swerve/advantagescope/desiredStates` を追加するにはロボットを有効にする必要があります。

    <figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
9.  Dataカラムをスワーブドライブのプロパティに合わせて調整する。

    1. `Data` カラムの `Max Speed` フィールドを `SmartDashboard/swerve/maxSpeed` エントリの値に変更する。

    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
10. （任意）Displayカラムの Size (Left-Right) と Size (Front-Back) を `SmartDashboard/swerve/sizeLeftRight` と `SmartDashboard/swerve/sizeFrontBack` の値に合わせることで、ロボットのシャーシ寸法を正確に表示するよう調整する。

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## 概要

<figure><img src="../.gitbook/assets/FRC_web_component_snapshot.png" alt=""><figcaption><p>誤った設定で動作中のスワーブドライブベース<br>（実際のAdvantage Scopeの画像に変更する必要があります）</p></figcaption></figure>

{% hint style="info" %}
**赤い**線はスワーブモジュールの測定された速度と位置です。

**青い**線は送信されたモジュールの速度と位置です！
{% endhint %}

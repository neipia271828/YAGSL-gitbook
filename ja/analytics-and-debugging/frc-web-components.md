# FRC Web Components

FRC Web ComponentsはスワーブドライブをビジュアライズしてYAGSL開発者へのフィードバックを提供する、簡単な方法です。

## ダウンロード

このページから正しいファイルを選んでダウンロード・インストールしてください。

{% embed url="https://github.com/frc-web-components/app/releases/latest" %}

## FRC Web Componentsの設定

<figure><img src="../.gitbook/assets/yagsl_fwc.gif" alt=""><figcaption></figcaption></figure>

1. ノートパソコンをロボットに接続する。
2. 「FRC Web Components」を開く。
3. 「Settings」をクリックする。

<figure><img src="../.gitbook/assets/fwc_config6.png" alt=""><figcaption><p>SettingsがハイライトされたリーRC Web Components</p></figcaption></figure>

4. チーム番号に基づいてroboRIOのIPアドレスを入力する。`10.TE.AM.2`

{% embed url="https://docs.wpilib.org/en/stable/docs/networking/networking-introduction/ip-configurations.html#te-am-ip-notation" %}

<figure><img src="../.gitbook/assets/fwc_conrfig5.png" alt=""><figcaption><p>FRC Web ComponentsのDashboard設定</p></figcaption></figure>

5. ウィジェットメニューを開く。

<figure><img src="../.gitbook/assets/fwc_config4.png" alt=""><figcaption><p>ここを押す</p></figcaption></figure>

6. `Swerve Drivebase` を選択して `Append` をクリックする。

<figure><img src="../.gitbook/assets/fwc_config3.png" alt=""><figcaption></figcaption></figure>

7. ウィジェットをクリックする。
8. 「Connect to data source...」をクリックする。

<figure><img src="../.gitbook/assets/fwc_config2.png" alt=""><figcaption></figcaption></figure>

9. ここのSelectを押して `SmartDashboard/swerve` に接続する。

<figure><img src="../.gitbook/assets/FWC_config1.png" alt=""><figcaption></figcaption></figure>

10. 「Close」を押す。

## 概要

<figure><img src="../.gitbook/assets/FRC_web_component_snapshot.png" alt=""><figcaption><p>誤った設定で動作中のスワーブドライブベース</p></figcaption></figure>

{% hint style="warning" %}
**青い**線はスワーブモジュールの測定された速度と位置です。

**赤い**線は送信されたモジュールの速度と位置です！
{% endhint %}

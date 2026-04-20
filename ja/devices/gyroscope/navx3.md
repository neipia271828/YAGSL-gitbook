# NavX3-CAN

{% hint style="info" %}
NavX3-CANはCAN 2.0（roboRIO）をサポートし、CAN-FD機能も備えています。CAN-FDの要件はStudicaのノートを参照してください。
{% endhint %}

## ドキュメント

{% embed url="https://www.studica.co/navx3-can-imu" %}
Studica製品ページ
{% endembed %}

{% embed url="https://github.com/Studica-Robotics/NavX" %}
Studica NavXのリリース、ファームウェアツール、vendordeps
{% endembed %}

## YAGSLチェックリスト

* [ ] **StudicaLib** vendordepをインストールする（StudicaとStudicaLibを両方インストールしない）。
* [ ] Studica Hardware ManagerでNavX3-CANのファームウェアを **5.0.4+** に更新する。
* [ ] Studica Hardware ManagerでNavX3-CANのCAN IDを確認してconfigに入力する（多くの場合、出荷時は0）。

## 校正

NavX3-CANは工場校正済みです。高精度が必要な場合はStudica Hardware Managerで再校正してください。センサーは校正中に自動的に向きを検出します。

## 通信

NavX3-CANはCAN経由で通信します。CAN-H/CAN-LをロボットのCANバスに接続し、バスが適切に終端されていることを確認してください。

## `swervedrive.json` の例

<pre class="language-json"><code class="lang-json">{
  "imu": {
    "type": <a data-footnote-ref href="#user-content-fn-1">"navx3"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-2">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-3">null</a>
  },
  "invertedIMU": <a data-footnote-ref href="#user-content-fn-4">false</a>,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: `navx3` IMUデバイスを選択してインスタンス化します。

[^2]: Studica Hardware Managerで確認できるNavX3-CANのCAN ID（多くの場合、出荷時は0）。

[^3]: `null` に設定（または省略）。`canbus` はCTRE専用でnavX3では使用しません。

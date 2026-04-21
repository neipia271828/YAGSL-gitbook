# Pigeon 2.0

Pigeon 2.0はCTREによるPigeon IMUの次世代バージョンです。このデバイスはCANivoreと互換性があります。

{% embed url="https://store.ctr-electronics.com/pigeon-2/" %}

## Tuner X

このデバイスはCTRE Tuner Xでアップグレードできます。

{% embed url="https://v6.docs.ctr-electronics.com/en/latest/docs/tuner/index.html" %}

## ドキュメント

{% embed url="https://pro.docs.ctr-electronics.com/en/latest/docs/hardware-reference/pigeon2/index.html" %}

{% embed url="https://v6.docs.ctr-electronics.com/en/latest/docs/tuner/pigeon-cal.html" %}

デバッグの際にはLEDステータスコードに注目してください。なお、Tuner XでPigeon 2.0の設定を変更しても、YAGSLの起動時に上書きされます。

## YAGSL チェックリスト

* [ ] ヨーが反時計回りを正方向とする。
* [ ] Pigeon 2.0 をできるだけ中央に配置する。
* [ ] Pigeon 2.0 を最新のファームウェアに更新する。
* [ ] Pigeon 2.0 をユニークなCAN IDに更新する。
* [ ] ロボットに取り付けた後でPigeon 2.0 を校正する。

## 通信

このデバイスはCANバス経由でroboRIOと通信し、roboRIOのCANバスの帯域を使わないように [CANivore](https://store.ctr-electronics.com/canivore/) と組み合わせることができます。そのためには[CANivore](https://pro.docs.ctr-electronics.com/en/latest/docs/canivore/canivore-setup.html)の名前を設定し、YAGSLの `canbus` 名としてそれを使用する必要があります。

{% embed url="https://pro.docs.ctr-electronics.com/en/latest/docs/canivore/canivore-setup.html" %}

## `swervedrive.json` の例

以下はPigeon 2.0を設定した `swervedrive.json` の例です。Pigeon 2.0はCANivoreと互換性があるため、`canbus` パラメーターが実際に機能することに注意してください！

<pre class="language-json"><code class="lang-json">{
  "imu": {
    "type": <a data-footnote-ref href="#user-content-fn-1">"pigeon2"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-2">13</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-3">"canivore"</a>
  },
  "invertedIMU": <a data-footnote-ref href="#user-content-fn-4">true</a>,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: Pigeon 2.0 IMUを選択。

[^2]: PigeonのCAN IDは13。

[^3]: Pigeon 2.0 はroboRIOに接続された "`canivore`" という名前のCANivore上にある。

[^4]: Pigeonは反転している。

# Pigeon

これはCTRE Pigeonの初代バージョンを指しており、既に生産終了となりPhoenix 5でのみサポートされています。

## Phoenix Tuner

このデバイスと通信してアップデートするには Phoenix Tuner をインストールする必要があります。

{% embed url="https://store.ctr-electronics.com/software/" %}
Phoenix Tunerは「Phoenix Framework for HERO C#」から入手できます
{% endembed %}

## ドキュメント

PigeonはPhoenix 5でのみ利用可能なため、ドキュメントは以下にあります。このデバイスも一因となり、YAGSLにPhoenix 5ライブラリを含める必要があります。

{% embed url="https://v5.docs.ctr-electronics.com/en/stable/ch11_BringUpPigeon.html" %}
Pigeonのプログラミングと設定ガイド
{% endembed %}

## YAGSL チェックリスト

* [ ] ユニークなCAN IDを設定する。
* [ ] 最新のファームウェアに更新する。

## 通信

CAN IDを設定したら、JSON設定の `pigeon` の `id` にそのCAN IDを設定します。このデバイスはCANivoreと互換性がないため、`canbus` オプションは適用されず `""` または `"rio"` または `null` にしてください。

## `swervedrive.json` の例

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": "<a data-footnote-ref href="#user-content-fn-1">pigeon</a>",
</strong>    "id": <a data-footnote-ref href="#user-content-fn-2">13</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-3">null</a>
  },
  "invertedIMU": true,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: Pigeonデバイス

[^2]: CAN ID 13

[^3]: 該当しないため `null` を選択。

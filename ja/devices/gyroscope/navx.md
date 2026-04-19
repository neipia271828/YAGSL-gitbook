---
description: NavX 2の校正
---

# NavX

NavX2はStudica（旧KauiLabs）によるNavXシリーズの最新版で、優れた読み取り精度と校正に関する豊富なドキュメントを提供しています。

{% embed url="https://www.studica.ca/en/navx-2-mxp-robotics-navigation-sensor" %}

## はじめに

NavXを入手しました、おめでとうございます！設定と校正を行いましょう。以下の手順に従わないと、最高のパフォーマンスを発揮できません。

NavXのJavaクラスは `AHRS` で、`import com.kauailabs.navx.frc.AHRS;` でインポートします。

## NavX オムニマウント

NavXはヨーをどの位置にでも再設定できます！方法はこちらのガイドを参照してください。

{% embed url="https://pdocs.kauailabs.com/navx-mxp/installation/omnimount/" %}

## NavXUIのインストール

Java 1.7以上が必要です。

{% embed url="https://pdocs.kauailabs.com/navx-mxp/software/navx-mxp-ui/" %}

{% embed url="https://www.kauailabs.com/public_files/navx-mxp/navx-mxp.zip" %}
インストールZIP
{% endembed %}

ロボットでの最適な精度を確保するための高度な校正に役立ちます。

## 校正

校正の完全なドキュメントはこちらをご覧ください。元の校正はハワイの工場で行われているため、少なくとも1回は校正を行うことを強くお勧めします！

{% embed url="https://pdocs.kauailabs.com/navx-mxp/guidance/gyroaccelcalibration/" %}

以下に簡単な概要を記載します。

ジャイロスコープは周囲温度の影響を受けやすいため、受け取ったとき**および**競技会でも必ず再校正してください。NavXの手順は以下の通りです。

1. NavXを地面と平行な安定した台に置いてください（水準器を使用してください！）。
2. **CAL** ボタンを少なくとも10秒間押し続けてください。
3. **CAL** ボタンを離した際に **CAL** LEDが一時点灯することを確認してください。
4. **RESET** ボタンを押してください。
5. 最大15秒待ってください。
6. 次のオンザフライジャイロ校正が完了するまでヨーの精度が低下する場合があります（つまり最初の自律走行がうまくいかない可能性があります！）。

## YAGSL チェックリスト

* [ ] 最新のファームウェアに更新する。
* [ ] NavXを地面と平行に水平に置く。
* [ ] NavXを校正する。
* [ ] 使用する接続/通信方式を選択する。

## 接続方式

### SPI（`navx`、`navx_spi`）

YAGSLはroboRIO MXP経由のNavX SPI通信をサポートしています。これはNavXデバイスとの通信に推奨される方式です。

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-1">"navx_spi"</a>,
</strong>    "id": <a data-footnote-ref href="#user-content-fn-2">0</a>,
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

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-4">"navx"</a>,
</strong>    "id": <a data-footnote-ref href="#user-content-fn-5">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-6">null</a>
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

### シリアル（`navx_mxp_serial`、`navx_usb`）

シリアル通信はSPI通信より低速で干渉を受けやすい場合がありますが、[NavX2 micro](https://www.andymark.com/products/navx2-micro-navigation-sensor)（`navx_usb`）とすぐに通信できる唯一の方法のため、使用する場合はこれを選択する必要があります。MXPシリアル通信はSPI MXP通信が一時的に機能しない場合にも使用できます。

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-7">"navx_mxp_serial"</a>,
</strong>    "id": <a data-footnote-ref href="#user-content-fn-8">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-9">null</a>
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

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-10">"navx_usb"</a>,
</strong>    "id": <a data-footnote-ref href="#user-content-fn-11">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-12">null</a>
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

### I2C（`navx_i2c`）

MXPのI2C通信はサポートされていますが、roboRIOでランダムに永続的なロックアウトを引き起こすことが知られており、この問題は徹底的に調査されましたが既知の原因は見つかっていません。この選択肢は可能な限り避けることをお勧めします！！

<pre class="language-json"><code class="lang-json">{
  "imu": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-13">"navx_i2c"</a>,
</strong>    "id": <a data-footnote-ref href="#user-content-fn-14">0</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-15">null</a>
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

## NavX Micro

<figure><img src="../../.gitbook/assets/navx2_micro.png" alt=""><figcaption><p>AndymarkのNavX micro</p></figcaption></figure>

NavX microを使用する場合は、タイプとして `navx_usb` を選択してUSB経由のシリアル通信を使用することを示してください。そうしないとジャイロスコープがYAGSLで動作しません！

[^1]: MXP上のSPI経由のNavXを選択。

[^2]: 該当なし。`0` は任意の値。

[^3]: 該当なし。`null` を使用。

[^4]: 内部的にはデフォルトで `navx_spi` になる。

[^5]: NavXではIDは関係ないため `0` を任意で選択。

[^6]: NavXでは `canbus` は関係ないため `null` で設定なしを示す。

[^7]: MXPシリアルポート経由のNavXシリアル通信。

[^8]: NavXではIDは関係ないため `0` を任意で選択。

[^9]: NavXでは `canbus` は関係ないため `null` で設定なしを示す。

[^10]: USBポート経由のNavXシリアル通信。NavX2 microとの通信に最も簡単な方法。

[^11]: NavXではIDは関係ないため `0` を任意で選択。

[^12]: NavXでは `canbus` は関係ないため `null` で設定なしを示す。

[^13]: MXPのI2Cポート経由のNavX接続。危険であるため、他の選択肢を使用してください！

[^14]: NavXではIDは関係ないため `0` を任意で選択。

[^15]: NavXでは `canbus` は関係ないため `null` で設定なしを示す。

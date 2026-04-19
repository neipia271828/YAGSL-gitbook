# デバイス設定

## デバイス設定

スワーブドライブのすべてのデバイスは基本的なフィールドセットに集約されます。デバイス設定はパース中にそれらのデバイスを格納・作成するために使用され、[`DeviceJson`](https://docs.yagsl.com/configuring-yagsl/configuration/device-configuration) と1:1でマッピングされます。

## フィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>type</code></td><td><a href="../../devices/absolute-encoders.md#possible-absolute-encoder-types">エンコーダー <code>type</code></a><br><a href="../../devices/gyroscope.md#possible-gyroscope-types">IMU <code>type</code></a><br><a href="../../devices/motor-controllers/#possible-motor-controller-types">モーター <code>type</code></a></td><td>Y</td><td>スワーブタイプを作成するために使用されるデバイスタイプ。</td></tr><tr><td><code>id</code></td><td>整数</td><td>Y</td><td>CANバス上のデバイスID、または特定デバイスの場合はroboRIOのピンID。</td></tr><tr><td><code>canbus</code></td><td>String</td><td>N</td><td>デバイスをインスタンス化するCANバス。代替CANバスと互換性のあるデバイスのみで機能します。<br><br><code>type</code> が <code>sparkmax_brushed</code> の場合、ドライブモーターに必要なモーターに接続されたエンコーダーを定義します（角度モーターでもアタッチされたアブソリュートエンコーダーがない場合は使用）。有効な値は <code>greyhill_63r256</code>、<code>srx_mag_encoder</code>、<code>throughbore</code>、<code>throughbore_dataport</code>、<code>greyhill_63r256_dataport</code>、<code>srx_mag_encoder_dataport</code> です。<br>接続されたエンコーダーがアブソリュートエンコーダーの場合はこれを設定しないでください。</td></tr></tbody></table>

## 便利なヒント

1. IMUが期待通り正確でない場合は最適化してください。NavXには[こちら](https://pdocs.kauailabs.com/navx-mxp/guidance/best-practices/)に便利なガイドがあります。
2. コードを読んでください！理解できないことがあれば、すべてのコードはドキュメント化されていて読みやすいです。

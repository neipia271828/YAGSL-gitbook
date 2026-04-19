# フローチャート

## スワーブドライブ図

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

## 注意事項

{% hint style="danger" %}
Pigeon 1 や NavX 1 のような古いジャイロスコープは、使用時間が長くなるほどドリフトが発生します。プログラム開始時にジャイロスコープを再起動またはゼロリセットすることを検討してください。
{% endhint %}

## スワーブモジュール

### スワーブモジュールが「制御不能」に回転する

<figure><img src="../.gitbook/assets/flowchart2.png" alt=""><figcaption></figcaption></figure>

## スワーブモジュールが正しく回転しない

後でフローチャートを追加予定ですが、基本的には以下を確認してください。

1. CAN ID、モータータイプ、アブソリュートエンコーダータイプは正しいか？
2. モジュールの位置は正しいか？
3. アブソリュートマグネティックエンコーダーを使用しているか？
   1. マグネットをLocktiteで固定したか？
   2. マグネットの読み取りが良好（通常は緑のLED）か？
4. ホイールのベベルを左向きにして車輪を直進方向に向けた状態でアブソリュートエンコーダーオフセットを設定したか？
5. 反転状態について [when-to-invert.md](when-to-invert.md "mention") を確認したか？
6. PIDを調整したか？

## スワーブドライブが「制御不能」に回転する

<figure><img src="../.gitbook/assets/flowchart4.png" alt=""><figcaption></figcaption></figure>

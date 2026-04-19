# 設定

チューニング用ウェブページ

YAGSLは、設定ファイルをゼロから作成するための補助ツールをこちらで提供しています。

{% embed url="https://github.com/Yet-Another-Software-Suite/YAGSL/tree/main/examples" %}

このウェブページからすべてのファイルをダウンロードして `deploy` ディレクトリに配置すると、`new SwerveParser(new File(Filesystem.getDeployDirectory(),"swerve").createSwerveDrive(Units.feetToMeters(4.5));` で使用できます。

動作確認済みの設定はこちらのリポジトリでも管理しています！

{% embed url="https://github.com/BroncBotz3481/YAGSL-Configs" %}

## モジュールがどれかを把握してください！

把握していないと、モジュールがこのような状態になります！

<figure><img src="../../.gitbook/assets/id_change3.png" alt=""><figcaption><p>モジュールが逆転し、CCW+方向に回転している</p></figcaption></figure>

これは、ジャイロスコープのロボット上での向きに起因するモジュール位置の入力ミス、または設定ファイルのモジュール位置にデバイス設定を誤って配置した場合に発生します。**この状態になった場合、モジュールの設定が誤っています。**

<details>

<summary>モジュール設定を入れ替える方法</summary>

例では番号を使って説明しますが、モジュールファイルを入れ替える際は番号を元のモジュール設定名に対応させます。上記の例では以下のようになります。

1. `frontleft.json`
2. `frontright.json`
3. `backleft.json`
4. `backright.json`

**`frontleft.json` と `backleft.json` を入れ替える**

<pre class="language-json" data-title="frontleft.json"><code class="lang-json">{
<strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 4,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 3,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 9,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": -114.609,
</strong>  "location": {
    "front": 12,
    "left": 12
  }
}
</code></pre>

<pre class="language-json" data-title="backleft.json"><code class="lang-json"><strong>{
</strong><strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 7,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 8,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 12,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": 6.504,
</strong>  "location": {
    "front": -12,
    "left": 12
  }
}
</code></pre>

ハイライトされた行を入れ替えると、モジュール設定が正しく入れ替わります。

**簡単な方法**

1. locationの符号を目的のモジュール側に変更する。
2. ファイルを上書きせずにリネームする。

</details>

#### 設定チャレンジ

<details>

<summary>チャレンジ</summary>

この誤設定をどうやって解決しますか？

<img src="../../.gitbook/assets/id_spin1.png" alt="右回転中" data-size="original"><img src="../../.gitbook/assets/challenge_reorder2.png" alt="右へ並進中" data-size="original">

</details>

<details>

<summary>解答</summary>

<img src="../../.gitbook/assets/challenge_reorder1.png" alt="" data-size="original">

手順：

1. 4と2を反転する。
2. 1と4を入れ替える。

</details>

これが役立つかもしれません！

{% file src="../../.gitbook/assets/SwerveDiagram.docx" %}

## スワーブモジュールを正しく向けてください！

`absoluteEncoderOffset` を正しく取得するには、値を読み取る際にスワーブモジュールが以下の向きになっている必要があります。

<figure><img src="../../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

## 設定ディレクトリ

### JSONファイル

JSONとは「JavaScript Object Notation」の略で、設定データや文字列データの表現に広く使われているフォーマットです。詳しくは[こちら](https://www.w3schools.com/js/js_json_intro.asp)をご覧ください。

### swerveディレクトリの構成

[`swervedrive.json`](swerve-drive-configuration.md) で `"modules": [ "frontleft.json", "frontright.json", "backleft.json", "backright.json"]` と定義した場合、スワーブドライブを生成するためのswerveディレクトリの構成は以下のようにする必要があります。

```
└── swerve
    ├── controllerproperties.json
    ├── modules
    │   ├── backleft.json
    │   ├── backright.json
    │   ├── frontleft.json
    │   ├── frontright.json
    │   ├── physicalproperties.json
    │   └── pidfproperties.json
    └── swervedrive.json
```

`modules` フォルダ、[`controllerproperties.json`](controller-properties-configuration.md)、[`physicalproperties.json`](physical-properties-configuration.md)、[`pidfproperties.json`](pidf-properties-configuration/)、[`swervedrive.json`](swerve-drive-configuration.md) ファイルはスワーブドライブの構築に必要です。それぞれのファイルにはスワーブドライブの属性に対応する固有のフィールドがあり、物理的なものと抽象的なものがあります。

## 古い設定ファイルの移行

1. `physicalproperties.json` から `wheelDiamter`、`gearRatio`、`encoderPulsePerRotation` を削除する
2. `physicalproperties.json` に `optimalVoltage` を追加する
3. `swervedrive.json` から `maxSpeed` と `optimalVoltage` を削除する
4. スワーブモジュールのドライブモーターまたはステアリングモーターがスワーブドライブの他のモジュールと異なる場合、そのモジュールの設定JSONファイルにドライブとステアリング両方の `conversionFactor` を**必ず**指定してください。一方のモーターが他のモジュールと同じで、その `conversionFactor` を使用したい場合は、モジュールJSONの `conversionFactor` を0に設定してください。
5. `new SwerveParser(directory).createSwerveDrive(maximumSpeed);` でSwerveDriveを作成する際に最大速度を必ず指定してください。
6. `swervedrive.json` に `conversionFactor` を設定したくない場合は、以下のようにコンストラクターのパラメーターとして渡せます。

```java
double DriveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(WHEEL_DIAMETER), GEAR_RATIO, ENCODER_RESOLUTION);
double SteeringConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(GEAR_RATIO, ENCODER_RESOLUTION);
new SwerveParser(directory).createSwerveDrive(maximumSpeed, SteeringConversionFactor, DriveConversionFactor);
```

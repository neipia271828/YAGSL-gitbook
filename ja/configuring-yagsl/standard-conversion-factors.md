# 標準コンバージョンファクター

このページでは、一般的なモジュール向けの標準コンバージョンファクターを提供します。

## 算出方法

```java
    // 角度コンバージョンファクターは 360 / (ギア比)
    //  この場合のギア比は、ホイール1回転につきモーター12.8回転。
    //  モーター1回転あたりのエンコーダー分解能は1。
    double angleConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(12.8);
    // モーターコンバージョンファクターは (PI * ホイール直径(メートル)) / (ギア比)
    //  この場合のホイール直径は4インチ（メートル/秒を得るためにメートルに変換）。
    //  ギア比はホイール1回転につきモーター6.75回転。
    //  モーター1回転あたりのエンコーダー分解能は1。
    double driveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(4), 6.75);
    System.out.println("\"conversionFactors\": {");
    System.out.println("\t\"angle\": {\"factor\": " + angleConversionFactor + "},");
    System.out.println("\t\"drive\": {\"factor\": " + driveConversionFactor + "}");
    System.out.println("}");
```

## MAX Swerve

{% tabs %}
{% tab title="12T" %}
```json
"conversionFactors": {
    "angle": {"gearRatio": 46.42},
    "drive": {"gearRatio": 5.50, "diameter": 3}
}
```
{% endtab %}

{% tab title="13T" %}
```json
"conversionFactors": {
    "angle": {"gearRatio": 46.42},
    "drive": {"gearRatio": 5.08, "diameter": 3}
}
```
{% endtab %}

{% tab title="14T" %}
```json
"conversionFactors": {
    "angle": {"gearRatio": 46.42},
    "drive": {"gearRatio": 4.71, "diameter": 3}
}
```
{% endtab %}
{% endtabs %}

## Swerve Drive Specialties (SDS)

{% tabs %}
{% tab title="MK4 L1" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 12.8},
	"drive": {"gearRatio": 8.14, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4 L2" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 12.8},
	"drive": {"gearRatio": 6.75, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4 L3" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 12.8},
	"drive": {"gearRatio": 6.12, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4 L4" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 12.8},
	"drive": {"gearRatio": 5.14, "diameter": 4}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK4i L1 " %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 21.4285714286},
	"drive": {"gearRatio": 8.14, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4i L2" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 21.4285714286},
	"drive": {"gearRatio": 6.75, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4i L3" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 21.4285714286},
	"drive": {"gearRatio": 6.12, "diameter": 4}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK5i R1" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26},
	"drive": {"gearRatio": 7.03, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK5i R2" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26},
	"drive": {"gearRatio": 6.03, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK5i R3" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26},
	"drive": {"gearRatio": 5.27, "diameter": 4}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK4n L1" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 18.75},
	"drive": {"gearRatio": 7.13, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4n L2" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 18.75},
	"drive": {"gearRatio": 5.9, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK4n L3" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 18.75},
	"drive": {"gearRatio": 5.36, "diameter": 4}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK5n R1" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26.09},
	"drive": {"gearRatio": 7.03, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK5n R2" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26.09},
	"drive": {"gearRatio": 6.03, "diameter": 4}
}
```
{% endtab %}

{% tab title="MK5n R3" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 26.09},
	"drive": {"gearRatio": 5.27, "diameter": 4}
}
```
{% endtab %}
{% endtabs %}

## Thrifty Swerve

{% tabs %}
{% tab title="18T" %}
<figure><img src="../.gitbook/assets/thriftswerve.PNG" alt=""><figcaption><p>Thrifty Swerve ギア比表</p></figcaption></figure>

以下の例は、18T出力ギア・12Tピニオンギア・3インチホイール・NEO制御の設定です。お使いの構成はチャートを参照してください。

ステアリングモーターのギア比は **25:1** です。

```json
"conversionFactors": {
	"angle": {"gearRatio": 25},
	"drive": {"gearRatio": 15, "diameter": 3}
}
```
{% endtab %}

{% tab title="16T" %}

<figure><img src="../.gitbook/assets/thriftswerve.PNG" alt=""><figcaption><p>Thrifty Swerve ギア比表</p></figcaption></figure>

以下の例は、16T出力ギア・12Tピニオンギア・3インチホイール・NEO制御の設定です。お使いの構成はチャートを参照してください。

ステアリングモーターのギア比は **25:1** です。

```json
"conversionFactors": {
	"angle": {"gearRatio": 25},
	"drive": {"gearRatio": 16.9, "diameter": 3}
}
```
{% endtab %}
{% endtabs %}

## Plummer Industries

{% tabs %}
{% tab title="Corner Mid Ratio" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 28},
	"drive": {"gearRatio": 4, "diameter": 2.5}
}
```
{% endtab %}

{% tab title="Corner High Ratio" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 28},
	"drive": {"gearRatio": 3.25, "diameter": 2.5}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Corner Mid Ratio - Linear" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 28},
	"drive": {"gearRatio": 4, "diameter": 2.5}
}
```
{% endtab %}

{% tab title="Corner High Ratio - Linear" %}
```json
"conversionFactors": {
	"angle": {"gearRatio": 28},
	"drive": {"gearRatio": 3.25, "diameter": 2.5}
}
```
{% endtab %}
{% endtabs %}

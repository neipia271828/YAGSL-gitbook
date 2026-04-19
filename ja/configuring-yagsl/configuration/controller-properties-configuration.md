# コントローラープロパティ設定

## スワーブコントローラー（`controllerproperties.json`）

スワーブコントローラーは、自律走行中およびジョイスティックに基づいてロボットの向きを設定するドライブモードにおけるスワーブドライブの動作に関する設定オプションを格納します。このJSONファイルは [`ControllerPropertiesJson`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/json/ControllerPropertiesJson.html) と1:1でマッピングされており、[`SwerveControllerConfiguration`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/SwerveControllerConfiguration.html) を生成します。ここの値は自律走行において**非常に**重要で、自律走行機能が正しく動作するためにはロボットの向きPIDが適切に調整されている必要があります。

## フィールド

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>angleJoystickRadiusDeadband</code></td><td>Double</td><td>Y</td><td>ロボットの向き調整を許可する角度制御ジョイスティックの最小半径。</td></tr><tr><td><code>heading</code></td><td><a href="pidf-properties-configuration/pidf.md">PID</a></td><td>Y</td><td>ロボットの向きを制御するPID。</td></tr></tbody></table>

## 設定例

<pre class="language-json"><code class="lang-json">{
  <a data-footnote-ref href="#user-content-fn-1">"angleJoystickRadiusDeadband": 0.5</a>,
  "heading": {
    "p": 0.4,
    "i": 0,
    "d": 0.01
  }
}
</code></pre>

[^1]: 2軸に基づいて回転目標角度を求める際のデッドバンドを設定するために [`SwerveController.getTargetSpeeds`](https://broncbotz3481.github.io/YAGSL-Lib/docs/swervelib/SwerveController.html#getTargetSpeeds\(double,double,double,double,double,double\)) で使用されます。

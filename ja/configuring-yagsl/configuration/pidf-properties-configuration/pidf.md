# PIDF

## PIDF設定

PIDF設定は [`PIDFConfig`](https://yet-another-software-suite.github.io/YAGSL/javadocs/swervelib/parser/PIDFConfig.html) と1:1でマッピングされており、モジュールの速度・位置やロボットの向きなど、ロボットのPIDまたはPIDF設定に関する情報を格納します。すべてのPIDF設定ですべてのパラメーターが使用されるわけではありません。例えば `heading` はPIDのみを使用します。

## フィールド

いずれかの箇所を `0` にすると、そのPIDF部分が実質的に無効になります。

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>p</code></td><td>kPゲイン</td><td>Y</td><td>PIDの比例ゲイン</td></tr><tr><td><code>i</code></td><td>kIゲイン</td><td>Y</td><td>PIDの積分ゲイン</td></tr><tr><td><code>d</code></td><td>kDゲイン</td><td>Y</td><td>PIDの微分ゲイン</td></tr><tr><td><code>f</code></td><td>数値</td><td>N</td><td>PIDのフィードフォワード</td></tr><tr><td><code>iz</code></td><td>数値</td><td>N</td><td>PIDインテグレーターの積分ゾーン</td></tr><tr><td><code>output</code></td><td><a href="pidf.md#range">Range</a></td><td>N</td><td>PIDの出力範囲</td></tr></tbody></table>

#### Range

<table data-full-width="true"><thead><tr><th>名前</th><th>単位</th><th>必須</th><th>説明</th></tr></thead><tbody><tr><td><code>min</code></td><td>数値</td><td>N</td><td>PID範囲の最小値。デフォルトは <code>-1</code>。</td></tr><tr><td><code>max</code></td><td>数値</td><td>N</td><td>PID範囲の最大値。デフォルトは <code>1</code>。</td></tr></tbody></table>

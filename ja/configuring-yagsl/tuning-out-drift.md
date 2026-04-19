# ドリフトを調整して除去する

現在のポーズを計算しようとする際、ポーズ推定器はある程度まで優れた仕事をします（調整することができますが、ほとんどのチームは行いません）。

まず最初にチューニングすべきは[ドライブと角度のPIDです。](how-to-tune-pidf.md)

ビジョンを使ってオドメトリーを補助している場合、チームは `SwerveDrivePoseEstimator` の `visionStdDev` を調整すべきです。ビジョンのチューニングは主に距離に応じたスケールで標準偏差を試行錯誤することです。推定ポーズをある距離以上フィルタリングすることでより安定するチームもあります。

その後、システム遅延を補正するためにdiscretizationを調整できます。discretizationのチューニングの詳細は[こちら](../overview/our-features/chassis-speed-discretization.md)にあります。

次に、ジャイロシステムの遅延を補正するための[角速度係数を調整できます](../overview/our-features/angular-velocity-compensation.md)（CANivore上にPigeonがある場合はそれほど必要ありません）。

これらはすべて、互いに積み重なる個別のチューニングステップです。

{% hint style="warning" %}
下位のステップを変更すると、上位のすべてのステップがずれてしまいます。また、ジャイロスコープのような部品を交換した場合は、すべてをやり直す必要があるかもしれません。
{% endhint %}

ドリフトを調整して除去するための数学的な詳細説明はこちらをご覧ください。

{% embed url="https://github.com/calcmogul/controls-engineering-in-frc" %}

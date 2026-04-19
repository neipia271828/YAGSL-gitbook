# 目次

* [スワーブドライブドキュメントへようこそ](README.md)
  * [リソース](readme/resources.md)

## 概要

* [YAGSLとは](overview/what-we-do.md)
* [主な機能](overview/our-features/README.md)
  * [テレメトリ](overview/our-features/telemetry.md)
  * [シミュレーション](overview/our-features/simulation.md)
  * [ロックポーズ](overview/our-features/lock-pose.md)
  * [最大速度](overview/our-features/max-speed.md)
  * [シャシー速度離散化](overview/our-features/chassis-speed-discretization.md)
  * [ビジョンオドメトリ](overview/our-features/vision-odometry.md)
  * [ヘディング補正](overview/our-features/heading-correction.md)
  * [オートセンタリングモジュール](overview/our-features/auto-centering-modules.md)
  * [オフセットオフロード](overview/our-features/offset-offloading.md)
  * [コサイン補正](overview/our-features/cosine-compensation.md)
  * [モジュール自動同期](overview/our-features/module-auto-synchronization.md)
  * [角速度補正](overview/our-features/angular-velocity-compensation.md)
* [変更履歴](overview/changelog.md)

## 基礎知識

* [スワーブドライブ](fundamentals/swerve-drive.md)
* [スワーブモジュール](fundamentals/swerve-modules.md)

## スワーブの立ち上げ

* [はじめに](bringing-up-swerve/preface.md)
* [スワーブ情報](bringing-up-swerve/swerve-information.md)
* [ジャイロスコープの確認](bringing-up-swerve/check-your-gyroscope.md)
* [モーターの確認](bringing-up-swerve/check-your-motors.md)
* [最初の設定を作成する](bringing-up-swerve/creating-your-first-configuration.md)

## YAGSLの設定

* [ロボットを知る](configuring-yagsl/getting-to-know-your-robot/README.md)
* [依存関係のインストール](configuring-yagsl/dependency-installation.md)
* [設定](configuring-yagsl/configuration/README.md)
  * [スワーブドライブ設定](configuring-yagsl/configuration/swerve-drive-configuration.md)
  * [物理プロパティ設定](configuring-yagsl/configuration/physical-properties-configuration.md)
  * [PIDFプロパティ設定](configuring-yagsl/configuration/pidf-properties-configuration/README.md)
    * [PIDF](configuring-yagsl/configuration/pidf-properties-configuration/pidf.md)
  * [スワーブモジュール設定](configuring-yagsl/configuration/swerve-module-configuration.md)
  * [コントローラープロパティ設定](configuring-yagsl/configuration/controller-properties-configuration.md)
  * [デバイス設定](configuring-yagsl/configuration/device-configuration.md)
* [コードセットアップ](configuring-yagsl/code-setup.md)
* [標準コンバージョンファクター](configuring-yagsl/standard-conversion-factors.md)
* [PIDFの調整方法](configuring-yagsl/how-to-tune-pidf.md)
* [反転のタイミング](configuring-yagsl/when-to-invert.md)
* [フローチャート](configuring-yagsl/flowcharts.md)
* [8つのステップ](configuring-yagsl/the-eight-steps.md)
* [スワーブドライブのドリフト](configuring-yagsl/swerve-drive-drift.md)
* [SparkMaxとSparkFlexのよくある問題](configuring-yagsl/sparkmax-common-problems.md)
* [モジュール位置の確認](configuring-yagsl/verifying-your-module-locations.md)
* [ドリフトの調整](configuring-yagsl/tuning-out-drift.md)

## デバイス

* [ジャイロスコープ](devices/gyroscope.md)
  * [NavX](devices/gyroscope/navx.md)
  * [NavX3](devices/gyroscope/navx3.md)
  * [Pigeon](devices/gyroscope/pigeon.md)
  * [Pigeon 2.0](devices/gyroscope/pigeon-2.0.md)
  * [ADXRS450](devices/gyroscope/adxrs450.md)
  * [ADIS16448](devices/gyroscope/adis16448.md)
  * [ADIS16470](devices/gyroscope/adis16470.md)
* [モーターコントローラー](devices/motor-controllers/README.md)
  * [SparkMAX](devices/motor-controllers/sparkmax.md)
  * [SparkFlex](devices/motor-controllers/sparkflex.md)
  * [TalonFX](devices/motor-controllers/talonfx.md)
* [アブソリュートエンコーダー](devices/absolute-encoders.md)

## アナリティクスとデバッグ

* [FRC Web Components](analytics-and-debugging/frc-web-components.md)
* [Advantage Scope](analytics-and-debugging/advantage-scope.md)

## レガシードキュメント

* [ドキュメント](legacy-documentation/documentation/README.md)
  * [JSON](legacy-documentation/documentation/json.md)
  * [スワーブドライブ](legacy-documentation/documentation/swerve-drive.md)
  * [スワーブモジュール](legacy-documentation/documentation/swerve-module.md)
  * [スワーブモジュール物理プロパティ](legacy-documentation/documentation/swerve-module-physical-properties.md)
  * [スワーブモジュールPIDFプロパティ](legacy-documentation/documentation/swerve-module-pidf-properties.md)
  * [コントローラープロパティ](legacy-documentation/documentation/controller-properties.md)
  * [テスト済み構成](legacy-documentation/documentation/tested-configurations.md)

# JSON

## JSONファイル

JSONとは「JavaScript Object Notation」の略で、設定データや文字列データの表現に広く使われているフォーマットです。詳しくは[こちら](https://www.w3schools.com/js/js\_json\_intro.asp)をご覧ください。

## swerveディレクトリの構成

[`swervedrive.json`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/deploy/swerve/swervedrive.json) で `"modules": [ "frontleft.json", "frontright.json", "backleft.json", "backright.json"]` と定義した場合、スワーブドライブを生成するためのswerveディレクトリの構成は以下のようにする必要があります。

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

`modules` フォルダ、`controllerproperties.json`、`physicalproperties.json`、`pidfproperties.json`、`swervedrive.json` ファイルはスワーブドライブの構築に必要です。それぞれのファイルにはスワーブドライブの属性に対応する固有のフィールドがあり、物理的なものと抽象的なものがあります。

#### 各JSONをクリックして設定オプションを確認する

* `swervedrive.json`
* `controllerproperties.json`
* `pidfpropreties.json`
* `physicalproperties.json`
* `backleft.json`、`backright.json`、`frontleft.json`、`frontright.json`

# Rojo セットアップ

!!! note "実機未検証"
    公式 v7 ドキュメントに沿った Windows 向け手順です。導入・同期の通し検証は未実施です。

## 1. Rojo CLI を用意する

[公式リリース](https://github.com/rojo-rbx/rojo/releases)から Windows と CPU に合う v7 のバイナリを取得し、展開先をユーザーの PATH に追加します。PowerShell を開き直し、使用する版を確認します。

~~~powershell
rojo --version
~~~

チームでツールの版をそろえる場合は、公式の [Rokit を使う手順](https://rojo.space/docs/v7/getting-started/installation/)も選べます。

## 2. Studio プラグインを導入する

~~~powershell
rojo plugin install
~~~

Roblox Studio を開き直し、プラグイン欄で Rojo が見えることを確認します。導入方法の詳細は[公式のインストール手順](https://rojo.space/docs/v7/getting-started/installation/)を参照してください。

## 3. 新しいテストプロジェクトを同期する

~~~powershell
rojo init roblox-demo
Set-Location roblox-demo
rojo serve
~~~

Studio で新規のテスト用プレースを開き、Rojo プラグインの接続画面で `rojo serve` が表示したホストとポートを指定します。既定の接続先は通常 `localhost:34872` です。同期内容を確認して接続し、Explorer にプロジェクト定義どおりのインスタンスが現れるか確認します。続きは[開発ワークフロー](workflow.md)へ進んでください。[公式の新規ゲーム作成手順](https://rojo.space/docs/v7/getting-started/new-game/)

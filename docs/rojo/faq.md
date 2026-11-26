# Rojo FAQ

以下は公式 v7 ドキュメントをもとにした確認項目です。今回の実機検証は未実施です。

## rojo が見つからない

`Get-Command rojo` と `rojo --version` で CLI を確認します。PATH を追加したら PowerShell を開き直します。VS Code 拡張から使える場合でも、ターミナルから呼ぶ場合は CLI の導入が必要です。

## Studio から接続できない

`rojo serve` が起動中か確認し、出力にあるホストとポートをプラグイン側で指定します。プラグインの版も確認します。CLI とプラグインの準備は[セットアップ](setup.md)を参照してください。

## 保存したファイルが Studio に出てこない

サーバーを起動したディレクトリと `default.project.json` の `$path` を確認します。`.server.luau`、`.client.luau` などのファイル名によって同期後のスクリプトの種類が変わります。[プロジェクト構成](project-structure.md)

## Studio 側で変更した内容を Git に戻せる？

このワークフローではファイルを編集し、Studio に同期します。Rojo の同期対象の Luau は外部エディタで管理してください。既存プレースに導入する場合は、コピーで同期対象を確認してから進めます。[公式の移行ガイド](https://rojo.space/docs/v7/getting-started/porting-an-existing-game/)

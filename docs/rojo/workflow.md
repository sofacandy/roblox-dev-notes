# Rojo 開発ワークフロー

以下の Codex → Rojo → Studio の手順は、今回の Windows 環境での通し検証が未実施です。

1. [セットアップ](setup.md)に沿って `rojo serve` を起動し、テスト用プレースから接続します。
2. ファイルを編集します。Codex に依頼する場合は、対象ファイル、ほしい挙動、Studio での確認方法を指定します。
3. Studio の Explorer で同期先を確認し、**Play** を押して **Output** を確認します。次の編集は Play を停止してから試すと、編集時と実行時を見分けやすくなります。
4. 差分を確認し、`src/` とプロジェクト設定を Git にコミットします。Studio 上で管理する配置やアセットは、プロジェクトの保存方針に沿って別途保存します。

## 同期を確認する最小例

[構成例](project-structure.md)を使う場合、`src/server/Hello.server.luau` に次を保存します。

~~~lua
print("Rojo server sync OK")
~~~

同期後に Play し、Studio の Output に文字列が出ることを確認します。

## プレースファイルを生成する

~~~powershell
rojo build -o build.rbxlx
~~~

生成したファイルを Studio で開くと、プロジェクト定義からビルドされた内容を確認できます。公開中のプレースへの反映は Studio 側の公開操作で行います。[Rojo の推奨ワークフロー](https://rojo.space/docs/v7/workflows/)

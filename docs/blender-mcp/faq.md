# Windows トラブルシューティング・FAQ

## 01. 設定画面で Blender MCP が見つからない

ローカルの stdio サーバーとして、[セットアップ](setup.md)の `[mcp_servers.blender]` を登録します。編集した設定が有効になるよう、アプリを開き直して新規チャットで確認します。

## 02. blender-1.0.3.mcpb はどう使う？

MCP クライアント向けバンドルです。このガイドでは Codex が uvx を使うので、バンドルの読み込みは不要です。Blender 側の待ち受けを準備する工程は使用する版の案内を確認してください。

## 03. uvx の実行後に ERROR と表示される

今回、Git からのビルドと依存のインストールは進みました。その後 `ERROR` が出ましたが、詳細行が記録されておらず原因は未特定です。Blender の起動、待ち受けの所有プロセス、ホストとポート、MCP の起動ログを順に確認します。

## 04. 手動起動したら止まったように見える

stdio サーバーは標準入力を待ち続ける場合があります。診断用の手動起動は `Ctrl+C` で終了し、Codex に起動を任せて `/mcp` で確認します。

## 05. localhost で TCP の警告が出る

今回、`::1:9876` は失敗し、`127.0.0.1:9876` は成功しました。IPv4 側の待ち受けが使えたため、`Test-NetConnection 127.0.0.1 -Port 9876` で再確認し、MCP の接続先も `BLENDER_MCP_HOST = "127.0.0.1"` にそろえます。

## 06. チャットに Blender MCP ツールが出ない

設定の読み込みとツール登録を確認します。CLI の `mcp list`、`mcp get blender`、対話中の `/mcp` を順に試し、デスクトップの新規ローカルチャットと比較します。TCP の成功とツール一覧は別々に確認できます。

## 07. Codex CLI が未インストール

デスクトップとの比較が必要なときに導入します。Node.js を入れ、PowerShell を開き直して `npm.cmd install -g @openai/codex@latest` を実行します。今回の記録では `codex.cmd --version` が 0.172.0 でした。

## 08. npm で PSSecurityException が発生する

今回、PowerShell が `npm.ps1` を選び、実行ポリシーでブロックされました。`npm.cmd` と `codex.cmd` を使うことで、実行ポリシーを変更せずに進められました。[コマンド集](commands.md)に導入と起動の例があります。

## 09. CLI 起動時に Hooks の信頼確認が出る

今回の画面には Review hooks、Trust all and continue、Continue without trusting が表示されました。接続の比較には、内容を確認していない Hooks を信頼せず、**Continue without trusting** を選びました。表示や選択肢の順番は版によって変わることがあります。

## 10. CLI で認識状況を確認したい

`codex.cmd mcp list` と `codex.cmd mcp get blender` で設定を確認し、対話中の `/mcp` でセッションの接続を見ます。実測は Blender **Connected / 26 tools** でした。シーン取得はツールを実際に呼び出して確認します。

## 11. CLI では見えるのにデスクトップでは見えない

CLI では `uvx` の名前指定で認識されました。デスクトップでは当初認識できず、絶対パスに変更後、動作したようだとの報告がありました。CLI とデスクトップで PATH の見え方が異なった可能性があります。起動ログによる原因の特定と最終ツール呼び出しの確認は未実施です。

`(Get-Command uvx.exe).Source` の結果を `command` に設定し、デスクトップを完全終了してからローカルの新規チャットでシーン取得を試します。

## 12. Roblox Studio MCP と同時に使える？

今回の CLI では Blender（26 tools）と Roblox_Studio（41 tools）が同時に認識されました。設定テーブルはサーバーごとに管理します。Blender から Studio にモデルを移すときは、エクスポートとインポートを行います。

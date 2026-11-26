# Windows セットアップ

Blender、Codex デスクトップ、uv、Git を用意します。CLI は接続の切り分けに使えます。対象は [Blender Lab の blender_mcp](https://projects.blender.org/lab/blender_mcp) です。

## 1. Blender 側の待ち受けを準備する

使用する Blender の版に対応した導入手順を、[Blender Lab リポジトリ](https://projects.blender.org/lab/blender_mcp)と[リリース一覧](https://projects.blender.org/lab/blender_mcp/releases)で確認してください。セットアップ時に取得した `blender-1.0.3.mcpb` は MCP クライアント向けのバンドルです。以下では Codex が uvx からサーバーを起動します。

今回の記録で確認できたのは Blender の起動と TCP 待ち受けです。Blender 側の導入ファイル、画面操作、対応バージョンの範囲は未確認です。待ち受けの設定が済んだら Blender を起動したままにし、ローカル接続先を確認します。

## 2. uvx の絶対パスを取得する

未導入なら [uv のインストール手順](https://docs.astral.sh/uv/getting-started/installation/)に従ってください。PowerShell を開き直して、以下を実行します。

~~~powershell
git --version
uvx --version
(Get-Command uvx.exe -ErrorAction Stop).Source
~~~

## 3. Codex に stdio サーバーを登録する

通常はユーザープロファイル配下の `.codex/config.toml` を編集します。`CODEX_HOME` を設定している場合はそのディレクトリを確認してください。同名のテーブルがあれば編集し、他の MCP 設定はそのまま使います。

~~~toml
[mcp_servers.blender]
enabled = true
command = 'C:\Users\YourName\.local\bin\uvx.exe'
args = ["--from", "git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp", "blender-mcp"]
startup_timeout_sec = 120
tool_timeout_sec = 120

[mcp_servers.blender.env]
BLENDER_MCP_HOST = "127.0.0.1"
BLENDER_MCP_PORT = "9876"
~~~

`command` は手順2の出力に置き換えます。TOML のシングルクォート内はパスのバックスラッシュを一つずつ書けます。取得元の Git リポジトリはリビジョンを固定していないため、将来同じツール数になるとは限りません。[Codex の MCP 設定](https://developers.openai.com/codex/mcp)

## 4. TCP、MCP、シーン情報の順に確認する

1. `Test-NetConnection 127.0.0.1 -Port 9876` で `TcpTestSucceeded` を確認します。待ち受けプロセスは[コマンド集](commands.md)で確認できます。
2. Codex デスクトップを完全に終了して開き直し、ローカルの新規チャットで Blender MCP を確認します。必要なら CLI の `/mcp` と比較します。
3. 下記を依頼して、実際にシーン情報が返るか確かめます。空のシーンで空の一覧が返るのは正常です。

~~~text
Blender MCP を使い、現在のシーンのオブジェクト名と種類を取得してください。
取得可能であれば Blender のバージョンも報告してください。
シーンは変更せず、失敗時はどの呼び出しで失敗したか教えてください。
~~~

TCP 成功は待ち受けへの到達を示します。`/mcp` の Connected は Codex と MCP サーバーの接続状況です。Blender との疎通はシーン取得で確認します。今回、デスクトップの最終ツール呼び出しログは未確認です。

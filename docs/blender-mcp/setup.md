# Windows で Blender を Codex につなぐ

Blender と Codex デスクトップ版を使います。コマンドは、Windows のスタートメニューで「PowerShell」と検索して開いた画面に貼り付け、Enter を押します。コード欄の右上からコピーできます。

!!! tip "AIに手伝ってもらう"
    下の文をコピーして、ふだん使っているAIチャットに貼り付けてください。今の画面に合わせて、次にすることを一つずつ聞けます。

    ~~~text
    Windows を使っています。Blender MCP を設定したいです。
    このページを読んで、初めての人向けに案内してください。
    https://sofacandy.github.io/roblox-dev-notes/blender-mcp/setup/

    まず、どこまでできているか私に聞いてください。
    次に開く画面や入力するコマンドを一つだけ教えてください。
    何が表示されたら成功かも教えて、私の返事を待ってください。
    ページに未確認と書いてあることは、そのまま教えてください。
    ページを開けなければ、必要な部分を貼るように言ってください。
    ~~~

## 1. Blender を接続できる状態にする

Blender 側には、Codex からの接続を受け付ける追加機能が必要です。このガイドで使うのは **Blender Lab の blender_mcp** です。[配布元](https://projects.blender.org/lab/blender_mcp)と[リリース一覧](https://projects.blender.org/lab/blender_mcp/releases)で、使っている Blender の版に合う案内を確認してください。

!!! note "ここは今回の記録だけでは案内できません"
    Blender 側でどのファイルを入れ、どのボタンを押したかは未確認です。`blender-1.0.3.mcpb` は MCP 対応アプリ向けの配布ファイルです。このガイドの Codex の設定では使いません。

Blender 側の準備ができたら、Blender を開いたまま PowerShell で次を実行します。

~~~powershell
Test-NetConnection 127.0.0.1 -Port 9876
~~~

`TcpTestSucceeded : True` と出たら次へ進みます。これは同じパソコン内の接続先に届いたという意味です。 `False` の場合は、Blender 側で接続の受け付けが始まっているか、ポート番号が `9876` かを確認します。

## 2. uv と Git を用意する

`uvx` は Codex が Blender との橋渡し役を起動するために使います。Git はその橋渡し役を配布元から取得するために使います。PowerShell で確認します。

~~~powershell
uvx --version
git --version
~~~

見つからない方だけ、[コマンド集のインストール欄](commands.md#uv-git)を見て入れてください。PowerShell を開き直し、両方のバージョンが表示されたら次を実行します。

~~~powershell
(Get-Command uvx.exe -ErrorAction Stop).Source
~~~

表示された `C:\` から `uvx.exe` までをコピーしておきます。これが次の手順で使う、ファイルの場所を最初から最後まで書いた「絶対パス」です。

## 3. メモ帳で Codex の設定を編集する

PowerShell で次を実行します。Codex の設定保存先を `CODEX_HOME` で変えている場合は、そのフォルダーの `config.toml` を開いてください。

~~~powershell
notepad "$env:USERPROFILE\.codex\config.toml"
~~~

開いたファイルに次を追加します。`[mcp_servers.blender]` がすでにあるときは、その部分を編集します。

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

`command` のシングルクォートの中だけを、手順2でコピーした通りに置き換えます。ほかの MCP の設定があれば残します。メモ帳で保存したら、Codex を完全に終了して開き直します。[Codex の MCP 設定資料](https://developers.openai.com/codex/mcp)

## 4. Codex に Blender の中を見てもらう

新しいローカルチャットを開いて、次の文を送ります。

~~~text
Blender MCP を使って、今のシーンにある物の名前と種類を教えてください。
分かれば Blender のバージョンも教えてください。
シーンは変更しないでください。失敗したら、どこで止まったか教えてください。
~~~

実際に開いているシーンと答えを見比べます。空のシーンなら一覧が空でも正常です。「ツールが見つからない」と言われたら [FAQ 06・11](faq.md) へ進みます。今回のデスクトップ版は、使えたようだとの報告までで、最後にシーンを読み取ったログは未確認です。

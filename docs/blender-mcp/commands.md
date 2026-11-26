# そのまま使えるコマンド集

Windows のスタートメニューから **PowerShell** を開きます。必要な欄だけ使ってください。

## uv と Git を入れる { #uv-git }

見つからない方だけ実行します。

~~~powershell
winget install --id astral-sh.uv -e
winget install --id Git.Git -e
~~~

PowerShell を開き直し、バージョンと `uvx.exe` の場所が出るか確認します。

~~~powershell
uv --version
uvx --version
git --version
(Get-Command uvx.exe -ErrorAction Stop).Source
~~~

## Blender に届くか調べる

Blender を開いたまま実行します。`TcpTestSucceeded : True` が目印です。

~~~powershell
Test-NetConnection 127.0.0.1 -Port 9876
~~~

誰がポート `9876` で接続を受け付けているか調べるときに使います。`ProcessName` に `blender` が出るか確認します。

~~~powershell
Get-NetTCPConnection -State Listen -LocalPort 9876 |
    ForEach-Object { Get-Process -Id $_.OwningProcess }
~~~

## ターミナル版 Codex で試す

Node.js がなければ `winget install --id OpenJS.NodeJS.LTS -e` を実行し、PowerShell を開き直します。次のコマンドは上から順に実行します。

~~~powershell
npm.cmd install -g @openai/codex@latest
codex.cmd --version
codex.cmd mcp list
codex.cmd mcp get blender
codex.cmd
~~~

最後の行で Codex が開いたら、その入力欄に `/mcp` と打って Enter を押します。対応する版では `/mcp verbose` で詳細を見られます。

## 起動エラーが出るときだけ、橋渡し役を手で起動する

普段は Codex が起動します。エラーを調べるときは次を PowerShell で実行します。画面が止まったように見えるときは、Codex からの入力を待っている場合があります。手動起動は `Ctrl+C` で終了できます。

~~~powershell
$uvxPath = (Get-Command uvx.exe -ErrorAction Stop).Source
$env:BLENDER_MCP_HOST = '127.0.0.1'
$env:BLENDER_MCP_PORT = '9876'
& $uvxPath --from "git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp" blender-mcp
~~~

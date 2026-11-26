# コマンド集

## uv と Git を導入・確認する

未導入のものだけインストールし、PowerShell を開き直します。

~~~powershell
winget install --id astral-sh.uv -e
winget install --id Git.Git -e
git --version
uv --version
uvx --version
(Get-Command uvx.exe -ErrorAction Stop).Source
~~~

既定の設定ファイルを開く例です。`CODEX_HOME` を変更している場合は読み替えます。

~~~powershell
notepad "$env:USERPROFILE\.codex\config.toml"
~~~

## Blender とポートを確認する

~~~powershell
Get-Process blender | Select-Object Id, Path
Get-NetTCPConnection -State Listen -LocalPort 9876 |
    Select-Object LocalAddress, LocalPort, OwningProcess
Get-NetTCPConnection -State Listen -LocalPort 9876 |
    ForEach-Object { Get-Process -Id $_.OwningProcess }
Test-NetConnection 127.0.0.1 -Port 9876
~~~

## CLI で登録と接続を確認する

Node.js が未導入の場合は `winget install --id OpenJS.NodeJS.LTS -e` を実行し、PowerShell を開き直します。

~~~powershell
npm.cmd install -g @openai/codex@latest
codex.cmd --version
codex.cmd mcp list
codex.cmd mcp get blender
codex.cmd
~~~

CLI を開いた後の入力欄で実行します。

~~~text
/mcp
~~~

利用中の CLI が対応していれば `/mcp verbose` でも詳細を確認できます。

## stdio を手動起動して診断する

通常は Codex が起動します。下記は起動エラーを調べるための手動実行例で、待機した場合は `Ctrl+C` で終了できます。

~~~powershell
$uvxPath = (Get-Command uvx.exe -ErrorAction Stop).Source
$env:BLENDER_MCP_HOST = '127.0.0.1'
$env:BLENDER_MCP_PORT = '9876'
& $uvxPath --from "git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp" blender-mcp
~~~

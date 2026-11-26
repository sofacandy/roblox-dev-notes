# Blender MCP

Windows で Blender Lab の MCP を Codex に接続する手順です。Codex は stdio で MCP サーバーを起動し、サーバーから Blender 側にはローカル TCP で接続します。

| 確認した段階 | Windows での結果 |
| --- | --- |
| Blender 5.3.0 の待ち受け | `127.0.0.1:9876` への TCP 接続に成功。IPv6 の `::1` は失敗 |
| Codex CLI 0.172.0 | `/mcp` で Blender が Connected、26 tools |
| デスクトップ | 当初はツール未認識。絶対パスに変更後、利用できたようだとの報告あり。最終ログは未確認 |

[セットアップ](setup.md) · [全12件のFAQ](faq.md) · [コマンド集](commands.md) · [活用例](examples.md)

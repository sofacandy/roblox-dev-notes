# Rojo プロジェクト構成

以下は今回の実機未検証の最小構成例です。`rojo init` が生成するテンプレートを使う場合は、生成された `default.project.json` を確認してください。

~~~text
roblox-demo/
├── default.project.json
└── src/
    ├── client/
    │   └── Hello.client.luau
    ├── server/
    │   └── Hello.server.luau
    └── shared/
        └── Greeting.luau
~~~

`default.project.json` は同期先のサービスと、プロジェクトファイルから見た相対パスを定義します。

~~~json
{
  "name": "RobloxDemo",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "Shared": { "$path": "src/shared" }
    },
    "ServerScriptService": {
      "Server": { "$path": "src/server" }
    },
    "StarterPlayer": {
      "StarterPlayerScripts": {
        "Client": { "$path": "src/client" }
      }
    }
  }
}
~~~

| ファイル | Studio の同期先 | 種類 |
| --- | --- | --- |
| `src/server/Hello.server.luau` | `ServerScriptService/Server/Hello` | Script |
| `src/client/Hello.client.luau` | `StarterPlayer/StarterPlayerScripts/Client/Hello` | LocalScript |
| `src/shared/Greeting.luau` | `ReplicatedStorage/Shared/Greeting` | ModuleScript |

`ReplicatedStorage` のモジュールはクライアントからも見えるため、サーバー専用コードは `ServerScriptService` 側に置きます。同期対象の判断には `tree` と `$path` を確認してください。[Rojo v7 Project Format](https://rojo.space/docs/v7/project-format/) · [Sync Details](https://rojo.space/docs/v7/sync-details/)

## ファイルを作成する

この例を使う場合は、上のツリーどおりにフォルダーと3つのファイルを作成します。サーバーでは  [同期確認用のコード](workflow.md) を使えます。クライアントから共有モジュールを参照する例は以下です。

```lua title="src/shared/Greeting.luau"
return "Rojo client sync OK"
```

```lua title="src/client/Hello.client.luau"
local shared = game:GetService("ReplicatedStorage"):WaitForChild("Shared")
local greeting = require(shared:WaitForChild("Greeting"))
print(greeting)
```

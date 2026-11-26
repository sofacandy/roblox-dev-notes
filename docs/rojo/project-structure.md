# ファイルは Studio のどこに出る？

このページは、[セットアップ](setup.md)でつないだあとに、自分でフォルダーを組み立てたい人向けです。以下は説明用の構成です。この Windows 環境では、この構成を Studio に同期するところまで試していません。

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

`default.project.json` は、パソコンのどのフォルダーを Studio のどこに表示するかを決めるファイルです。プロジェクトの一番上に置きます。`$path` に書く場所は、このファイルがあるフォルダーからたどります。

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

この例を使う場合は、上のツリーどおりにフォルダーと3つのファイルを作成します。`Hello.server.luau` には [Studio で試すためのコード](workflow.md)を書きます。残りの2つには、ファイル名を見ながら次の内容を書きます。

```lua title="src/shared/Greeting.luau"
return "Rojo client sync OK"
```

```lua title="src/client/Hello.client.luau"
local shared = game:GetService("ReplicatedStorage"):WaitForChild("Shared")
local greeting = require(shared:WaitForChild("Greeting"))
print(greeting)
```

# 実装ノート：2フォルダーの同期から、Rojo でゲームを組み立てるまで

私の Roblox 開発では、最初はサーバーとクライアントの2つのフォルダーだけを Rojo で Roblox Studio に反映していました。今は AI にも手伝ってもらいながら、Rojo で同期することを前提にゲーム全体を作っています。Studio で開いた直後は Baseplate（床になる板）がある状態です。そこから Rojo で同期して、ゲームを組み立てます。

| 時期 | Rojo で Studio に反映していた範囲 | 開発の進め方 |
| --- | --- | --- |
| 初期 | サーバー用・クライアント用の2フォルダー | AI はあまり使っていなかった |
| 移行中 | 管理する範囲を少しずつ拡大 | 開発を続けながら、Rojo に任せる範囲を増やした |
| 現在 | ゲームを組み立てるためのファイル | AI にも手伝ってもらい、Baseplate がある状態から Rojo で同期してゲームを組み立てる |

## AI と Rojo は何をしている？

AI はコードを書く作業や修正を手伝います。Rojo は、パソコン側のファイルを Studio の決められた場所に反映します。Studio では、反映されたコードを Play して動きを確かめます。

技術的に変わったのは、Rojo の設定でパソコン側のファイルと Studio 側の置き場所を対応づける範囲です。初期は2つのフォルダーから始め、その範囲を徐々に広げました。今は、Baseplate がある状態から同期して、ゲームを動かすところまでが普段の開発の流れです。

## Studio の Explorer で見ると、こう変わった

Rojo がコードを置く場所だけを抜き出しました。

### 最初の Obby：2か所にコードを直接置く

~~~text
ServerScriptService
└─ サーバー用のコード

StarterPlayer
└─ StarterPlayerScripts
   └─ クライアント用のコード
~~~

### 最近のプロジェクト：用途別に3つのフォルダーに分ける

~~~text
ServerScriptService
└─ Server
   └─ サーバー用のコード

StarterPlayer
└─ StarterPlayerScripts
   └─ Client
      └─ クライアント用のコード

ReplicatedStorage
└─ Shared
   └─ サーバーとクライアントの両方で使うコード
~~~

たとえば、両方で使うコードをパソコンの `src/shared` に保存すると、Studio の `ReplicatedStorage → Shared` に反映されます。

## 設定ファイルで確かめる

下の `default.project.json` を読むときは、まず `$path` の行を探してください。そこにパソコン側のフォルダーが書いてあります。その行を囲んでいる名前を外側から順にたどると、上の図の Studio 側の置き場所が分かります。

!!! note "Studio で手作業で追加した項目を残す"
    たとえば、Studio で `ServerScriptService` を開くと、Rojo が作った `Server` フォルダーがあります。テスト用に `TestScript` を手作業で作り、`Server` の隣に置いたとします。

    この設定では、Rojo が `src/server` の内容を `Server` に反映しても、隣の `TestScript` は残ります。`ServerScriptService` の `$ignoreUnknownInstances: true` が、「Rojo の設定にない項目も、この階層では残す」という意味だからです。

    `TestScript` を `Server` の**中**に置くと話が変わります。`Server` は `$path` でパソコンのフォルダーとつながっていて、親の設定は引き継ぎません。そのため、パソコン側にない `TestScript` は同期で削除されることがあります。[Rojo v7 の設定ファイル仕様](https://rojo.space/docs/v7/project-format/#instance-description)

### 初期の設定（Lotus-Pond-Obby）

~~~json
{
  "name": "Lotus-Pond-Obby",
  "tree": {
    "$className": "DataModel",
    "ServerScriptService": {
      "$path": "src/ServerScriptService"
    },
    "StarterPlayer": {
      "StarterPlayerScripts": {
        "$path": "src/StarterPlayer/StarterPlayerScripts"
      }
    }
  }
}
~~~

### 最近の設定（Game）

~~~json
{
  "name": "Game",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "$ignoreUnknownInstances": true,
      "Shared": {
        "$path": "src/shared"
      }
    },
    "ServerScriptService": {
      "$ignoreUnknownInstances": true,
      "Server": {
        "$path": "src/server"
      }
    },
    "StarterPlayer": {
      "$ignoreUnknownInstances": true,
      "StarterPlayerScripts": {
        "$ignoreUnknownInstances": true,
        "Client": {
          "$path": "src/client"
        }
      }
    }
  }
}
~~~

Rojo でファイルの置き場所を決める仕組みは、[ファイルは Studio のどこに出る？](project-structure.md)で説明しています。

[開発ワークフローに戻る](workflow.md)

# 実装ノート：Rojo で管理するコードを増やした話

私の Roblox 開発では、最初はサーバーとクライアントの2つのフォルダーだけを Rojo で Roblox Studio に反映していました。今は AI にも手伝ってもらいながら開発を進め、ゲームのコード全体を Rojo で Studio に反映しています。

| 時期 | Rojo で Studio に反映していた範囲 | 開発の進め方 |
| --- | --- | --- |
| 初期 | サーバー用・クライアント用の2フォルダー | AI はあまり使っていなかった |
| 移行中 | 管理する範囲を少しずつ拡大 | 開発を続けながら、Rojo に任せる範囲を増やした |
| 現在 | ゲームのコード全体 | AI にも手伝ってもらい、変更したコードを Rojo 経由で Studio に反映 |

## AI と Rojo は何をしている？

AI はコードを書く作業や修正を手伝います。Rojo は、パソコン側のファイルを Studio の決められた場所に反映します。Studio では、反映されたコードを Play して動きを確かめます。

技術的に変わったのは、Rojo の設定でパソコン側のファイルと Studio 側の置き場所を対応づける範囲です。初期は2つのフォルダーから始め、その範囲を徐々に広げました。ここでいう「コード全体」はゲームのスクリプトを指します。

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

!!! note "Studio 側にある項目を残す設定"
    新しい設定では、親の項目に `$ignoreUnknownInstances: true` を書いています。たとえば `ServerScriptService` では、設定に含まれていない `Server` の隣の項目を同期時に残します。`Server` の内側は別で、親の指定は引き継ぎません。既定値は、その項目に `$path` があると `false`、ないと `true` です。[Rojo v7 の設定ファイル仕様](https://rojo.space/docs/v7/project-format/#instance-description)

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

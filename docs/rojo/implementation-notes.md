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

## 実際の設定を比べる

初めて作った Obby と、最近のプロジェクトの `default.project.json` を比べます。`$path` はパソコン側のフォルダーです。Studio 側の置き場所は、その項目を設定のどこに書くかで決まります。

| コードの種類 | 最初の Obby での置き場所 | 最近のプロジェクトでの置き場所 |
| --- | --- | --- |
| サーバー用 | `ServerScriptService` の直下 | `ServerScriptService → Server` |
| クライアント用 | `StarterPlayer → StarterPlayerScripts` の直下 | `StarterPlayer → StarterPlayerScripts → Client` |
| 両方で使うもの | この設定に共通コード用の同期先はなし | `ReplicatedStorage → Shared` |

初期は、Studio のサービスやコンテナにフォルダーを直接つないでいました。最近は、その中に `Server`、`Client`、`Shared` という置き場所を分け、パソコン側でも `src/server`、`src/client`、`src/shared` に分けています。たとえば、サーバーとクライアントの両方で使うコードは `src/shared` に置くと、Studio の `ReplicatedStorage → Shared` に反映されます。

もう一つの違いは、`$ignoreUnknownInstances: true` を親の項目に明記したことです。たとえば `ServerScriptService` では、Rojo の設定に含まれていない `Server` の隣の項目を、同期時に残す指定です。Rojo v7 では、`$path` がある項目の既定値は `false`、ない項目は `true` です。この設定例で同期元を指定している `Server`、`Client`、`Shared` の内側まで、この指定が引き継がれるわけではありません。[Rojo v7 の設定ファイル仕様](https://rojo.space/docs/v7/project-format/#instance-description)

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

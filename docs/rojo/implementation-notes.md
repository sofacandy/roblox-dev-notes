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

## 設定ファイルでは、どこが変わる？

Rojo の `default.project.json` では、`$path` にパソコン側のフォルダーを指定します。その項目を設定のどこに書くかで、Studio 側の置き場所が決まります。

下は、サーバー用・クライアント用の2フォルダーから始めて、両方で使うコードも同期する場合の差分です。**仕組みを説明するための例**で、私のプロジェクトの実際の設定や、追加した順番を示すものではありません。行頭に `+` がある行が追加分です。

~~~diff
 {
   "tree": {
     "$className": "DataModel",
+    "ReplicatedStorage": {
+      "Shared": { "$path": "src/shared" }
+    },
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

この例では、パソコンの `src/shared` に保存したコードが、Studio の **Explorer → ReplicatedStorage → Shared** に出るようになります。サーバー用・クライアント用と同じように、共通コードも手元のファイルを編集して Studio に反映できます。

Rojo でファイルの置き場所を決める仕組みは、[ファイルは Studio のどこに出る？](project-structure.md)で説明しています。

[開発ワークフローに戻る](workflow.md)

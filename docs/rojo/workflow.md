# コードを1行書いて Studio で試す

[セットアップ](setup.md)で練習用の `roblox-demo` を作り、Studio から Rojo に接続したあとに行います。この Windows 環境では、ここまでの流れを通して試していません。

## 1. 練習用のコードを保存する

エディタで `roblox-demo` を開きます。`default.project.json` を開き、`ServerScriptService` の中にある `$path` を探します。その場所が、Studio でサーバー用のコードを置くフォルダーです。[このサイトの構成例](project-structure.md)では `src/server` です。

そのフォルダーに `Hello.server.luau` を作り、次の1行を貼って保存します。Codex に書いてもらう場合も、保存先と「Studio の Output にこの文字を出したい」と伝えます。

~~~lua
print("Rojo server sync OK")
~~~

## 2. Studio で実際に動かす

1. Studio の **Explorer（エクスプローラー）** で **ServerScriptService** を開き、その下に `Hello` があるか確認します。
2. **Play** を押します。**View（表示）→ Output（出力）** を開いて、`Rojo server sync OK` が出れば成功です。
3. **Stop** でテストを止めます。続けて編集する場合は、エディタで保存してからもう一度 Play します。

表示されない場合は [Rojo FAQ](faq.md) を参照してください。Git を使っている場合は、うまく動いたコードと `default.project.json` を変更履歴に残しておけます。

## Studio で開けるファイルを作る

[セットアップ](setup.md)と同じ場所に `rojo.exe` がある場合は、`roblox-demo` を開いた PowerShell で実行します。Rojo が `rojo` だけで呼べる環境では `rojo build -o build.rbxlx` でも実行できます。

~~~powershell
..\rojo.exe build -o build.rbxlx
~~~

`build.rbxlx` ができたら Studio で開けます。Roblox に公開する場合は Studio から公開します。

# Rojo で困ったときは

公式の Rojo v7 ガイドをもとにした確認方法です。この Windows 環境での実機確認は未実施です。

## PowerShell が「rojo が見つからない」と言う

エクスプローラーで `rojo.exe` があるフォルダーを開き、上のアドレス欄に `powershell` と入力します。開いた画面で `.\rojo.exe --version` を実行し、バージョンが出るか確認します。[セットアップ](setup.md)

## Studio からつながらない

Rojo を起動した PowerShell がまだ開いているか確認します。その画面に出たアドレスとポート番号を Studio の Rojo プラグインに入れます。プラグインが見つからなければ [セットアップの手順2](setup.md) に戻ります。

## 保存したのに Studio に出てこない

Studio の **Explorer** を開いて、探している場所を確認します。パソコン側では `default.project.json` の `$path` が指すフォルダーに保存したかを見ます。名前の最後が `.server.luau` か `.client.luau` かによって、Studio で作られるスクリプトの種類も変わります。[ファイルの置き場所](project-structure.md)

## Studio で書き換えたコードはパソコンのファイルにも戻る？

このガイドでは、パソコンのファイルを編集して Studio へ送ります。コードを修正するときはエディタでファイルを開いて保存します。すでに作っているゲームに Rojo を使う場合は、コピーした場所で試せます。[公式の移行ガイド](https://rojo.space/docs/v7/getting-started/porting-an-existing-game/)

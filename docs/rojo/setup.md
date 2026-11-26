# Windows で Rojo を始める

Rojo は、パソコンで動かす本体と、Roblox Studio に入れる追加機能の2つを使います。まずはテスト用に新しい場所を作ります。コマンドは、Windows のスタートメニューで「PowerShell」と検索して開いた画面に貼り付けます。

!!! note "試した範囲"
    公式の Rojo v7 ガイドをもとにした手順です。この Windows 環境では Studio での接続まで通して試していません。

!!! tip "AIに手伝ってもらう"
    下の文をコピーして、ふだん使っているAIチャットに貼り付けてください。今の画面に合わせて、次にすることを一つずつ聞けます。

    ~~~text
    Windows を使っています。Rojo を設定して、Roblox Studio でコードを試したいです。
    このページを読んで、初めての人向けに案内してください。
    https://sofacandy.github.io/roblox-dev-notes/rojo/setup/

    まず、どこまでできているか私に聞いてください。
    次に開く画面や入力するコマンドを一つだけ教えてください。
    何が表示されたら成功かも教えて、私の返事を待ってください。
    ページに未確認と書いてあることは、そのまま教えてください。
    ページを開けなければ、必要な部分を貼るように言ってください。
    ~~~

## 1. Rojo 本体を入れる

[Rojo の配布ページ](https://github.com/rojo-rbx/rojo/releases)で v7 のリリースを開き、**Assets** からパソコンに合う Windows 用の ZIP をダウンロードして展開します。一般的な Intel・AMD の64ビット PC では x86_64 版を選びます。

この案内では、`rojo.exe` があるフォルダーでコマンドを実行します。エクスプローラーでそのフォルダーを開き、上のアドレス欄に `powershell` と入力して Enter を押します。開いた画面で次を実行します。

~~~powershell
.\rojo.exe --version
~~~

`Rojo 7` で始まるバージョンが表示されたら次に進みます。

## 2. Studio に Rojo を入れる

同じ PowerShell で実行します。

~~~powershell
.\rojo.exe plugin install
~~~

Roblox Studio を開き直し、上の **Plugins（プラグイン）** 欄に Rojo があるか確認します。[公式のインストール手順](https://rojo.space/docs/v7/getting-started/installation/)

## 3. 練習用のフォルダーを作って Studio につなぐ

同じ PowerShell で次の3行を上から順に実行します。

~~~powershell
.\rojo.exe init roblox-demo
Set-Location roblox-demo
..\rojo.exe serve
~~~

`roblox-demo` が練習用のフォルダーです。画面にアドレスとポート番号が出たら、PowerShell を開いたままにします。

Studio で新しいテスト用の場所を開きます。**Plugins → Rojo** を開き、PowerShell に表示された接続先を使って接続します。通常は `localhost:34872` です。Studio に変更の確認画面が出たら、内容を見てから進みます。**Explorer（エクスプローラー）** にコードやフォルダーが表示されたら、[コードを1行書いて試す](workflow.md)へ進みます。[公式の新規ゲーム作成手順](https://rojo.space/docs/v7/getting-started/new-game/)

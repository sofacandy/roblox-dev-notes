# Roblox Dev Notes

Blender を Codex につなぐ方法と、Rojo で手元のコードを Roblox Studio に反映する方法をまとめた日本語サイトです。

## 自分のパソコンでサイトを開く

Python 3.12 を用意します。エクスプローラーで mkdocs.yml があるフォルダーを開き、アドレス欄に powershell と入力して Enter を押します。次のコマンドを上から1行ずつ実行します。

~~~powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m mkdocs build --strict
.\.venv\Scripts\python.exe -m mkdocs serve
~~~

起動後、ブラウザーで http://127.0.0.1:8000/roblox-dev-notes/ を開きます。実際の URL はターミナルの表示を確認してください。記事は `docs/` に置き、メニューへの追加は `mkdocs.yml` の `nav` を編集します。

## GitHub Pages に公開

1. GitHub で `sofacandy/roblox-dev-notes` を公開リポジトリとして新規作成します。下記コマンドを使う場合は README などの自動生成を選ばず、空の状態で作成します。
2. ZIP を展開し、`mkdocs.yml` のあるフォルダーで次を実行します。

   ~~~powershell
   git init -b main
   git add .
   git commit -m "Add Roblox Dev Notes"
   git remote add origin https://github.com/sofacandy/roblox-dev-notes.git
   git push -u origin main
   ~~~

   既存リポジトリに追加するときは、そのリポジトリをクローンし、既存ファイルと統合してからコミットします。別のアカウント名・リポジトリ名を使う場合は `site_url`、`repo_url`、`repo_name` と push 先を調整してください。

3. リポジトリの **Settings → Pages → Build and deployment → Source** で **GitHub Actions** を選びます。
4. **Actions → Deploy MkDocs to GitHub Pages → Run workflow** で `main` を選んで実行します。初回 push が Pages の設定より先でも、この操作で再実行できます。
5. `build` と `deploy` の成功を確認し、デプロイジョブに表示される URL を開きます。想定 URL は https://sofacandy.github.io/roblox-dev-notes/ です。以後は `main` の更新で自動公開し、プルリクエストではビルドを検証します。

[GitHub Pages の公開元の設定](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) · [GitHub Actions による公開](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)

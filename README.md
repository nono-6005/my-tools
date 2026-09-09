# my-tools

ブラウザだけで動く自作ツール置き場。GitHub Pages で公開して、PC・スマホの両方から使う。

サーバー・データベース・APIは使わない。

## 構成

```
my-tools/
├── index.html          ツール一覧
├── threads.html        デモThreads
├── json-storage.html   JSON保管庫
└── README.md
```

各HTMLはCSS・JavaScriptを内包した単体ファイル。ビルド不要。

## ツール

### デモThreads (`threads.html`)

Threadsへ投稿する前に、自分の投稿がスマホ版のTLでどう見えるか確認するプレビューツール。

- 単発投稿 / ツリー投稿（本数の上限なし）
- TLへ保存、編集、複製、削除
- TLリセット（誤操作防止の2回押し方式）
- 表示名 / ユーザーネーム / アイコン画像URL の設定
- JSON書き出し・読み込み

### JSON保管庫 (`json-storage.html`)

文章をタイトル・タグ付きで保存し、あとから検索して取り出すツール。

- 新規保存 / 編集 / 削除
- タイトル・本文・タグを対象にした絞り込み検索
- JSON書き出し・読み込み

## 保存の仕組み

データは **開いている端末のブラウザ内（localStorage）** に保存される。

| | |
|---|---|
| 保存先 | 端末・ブラウザ単位 |
| 自動同期 | **なし**（スマホで保存したものはPCには出てこない） |
| 移行方法 | JSON書き出し → 移行先で読み込み |
| 消えるとき | ブラウザのサイトデータを削除したとき |

同期が必要になったら、その時点でサーバーかクラウドDBの導入を検討する。今は入れない。

## GitHub Pages 公開手順

### 1. GitHubにリポジトリを作る

```powershell
gh auth login          # トークンが切れている場合
gh repo create my-tools --public --source=. --remote=origin --push
```

`gh` を使わない場合は、GitHub上で `my-tools` リポジトリを作ってから:

```powershell
git remote add origin https://github.com/<ユーザー名>/my-tools.git
git branch -M main
git push -u origin main
```

### 2. Pages を有効にする

リポジトリの **Settings → Pages** で:

- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`

保存して1〜2分待つと公開される。

### 3. アクセス

```
https://<ユーザー名>.github.io/my-tools/
```

スマホからは同じURLを開く。ホーム画面に追加しておくとアプリのように使える。

### 更新方法

```powershell
git add -A
git commit -m "変更内容"
git push
```

push すると数十秒〜数分で反映される。

## 方針

必要になるまで構成を複雑にしない。

現時点で入れないもの: バックエンドサーバー / データベース / REST API / ログイン / ユーザー管理 / Docker / ビルド環境 / React・Vue等のフレームワーク。

ファイルが大きくなって管理しづらくなった時点で、はじめてCSS・JSの分離を検討する。

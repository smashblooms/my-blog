# なんでもブログ

## 環境構築, 記事・メモ投稿ガイド

このリポジトリは、Jekyllを使用して構築された個人ブログのソースコードである。
以下に、このブログをローカル環境で再現し、カスタマイズするための手順をまとめる。

## 目次

- 前提条件
- セットアップ手順
- サイト構造の概要
- 記事・メモの作成方法
- ローカルサーバーの起動
- トラブルシューティング

## 前提条件

- Ruby
- RubyGems
- Bundler
- Jekyll
  （特定のバージョンが必要な場合は、`Gemfile` を確認のこと。）

## セットアップ手順

1.  **リポジトリのクローン:**
    ```bash
    git clone <リポジトリのURL>
    cd <リポジトリ名>
    ```

2.  **依存関係のインストール:**
    プロジェクトルートにある `Gemfile` に基づいて、必要なgemをインストールする。
    ```bash
    bundle install
    ```

3.  **設定ファイルの確認・編集 (`_config.yml`):**
    サイトのタイトル、説明、URL、`baseurl`（GitHub Pagesで公開する場合のリポジトリ名など）を各自の環境に合わせて編集する。
    ```yaml
    title: あなたのブログタイトル
    email: あなたのメールアドレス
    description: >-
      ブログの簡単な説明を記述。
    baseurl: "" # ローカルテスト時は空、GitHub Pagesでは "/リポジトリ名"
    url: "" # 公開時のURL
    github_username: あなたのGitHubユーザー名
    ```
    コレクション（`tech_memos`, `daily_memos`, `tech_articles`, `daily_articles`）のパーマリンク設定などもここで定義されている。

## サイト構造の概要

主要なファイルとディレクトリの役割は以下の通り。

- `_config.yml`: サイト全体の設定ファイル。
- `_includes/`: ナビゲーションメニューなど、サイトの部品として再利用されるHTMLスニペットを格納。
- `_layouts/`: ページの基本的なHTML構造を定義するレイアウトファイルを格納（`default.html`, `post.html`, `memo.html`）。
- `_tech_articles/`: 技術記事のMarkdownファイルを格納。
- `_daily_articles/`: 日常記事のMarkdownファイルを格納。
- `_tech_memos/`: 技術メモのMarkdownファイルを格納。
- `_daily_memos/`: 日常メモのMarkdownファイルを格納。
- `assets/`: CSS、画像、JavaScriptファイルなどを格納。
- `pages/`: 固定ページ（例: `about.html`）のMarkdownまたはHTMLファイルを格納。
- `index.html`: ホームページ。
- `tech-articles.html`, `daily-articles.html`, `tech-memos.html`, `daily-memos.html`: 各コレクションの一覧ページ。

## 記事・メモの作成方法

1.  適切なコレクションフォルダ（例: `_tech_articles` や `_daily_memos`）内に、Markdownファイル (`.md`) を作成する。
    ファイル名は `YYYY-MM-DD-your-title.md` のように日付を含めると管理しやすい（必須ではないが、Jekyllの慣習）。

2.  ファイルの先頭にフロントマターを記述する。最低限、`title` と `date` を含めることを推奨。`date` がJekyllビルド時の時刻より未来である場合、一覧ページには表示されるものの、詳細ページは生成されない（Not Foundとなる）。
    ```yaml
    ---
    title: "記事のタイトル"
    date: YYYY-MM-DD HH:MM:SS +/-TTTT # 例: 2025-06-03 10:00:00 +0900
    # categories: [カテゴリ1, カテゴリ2] # 任意
    # tags: [タグ1, タグ2] # 任意
    ---

    ここにMarkdown形式で本文を記述する。
    ```
    `_config.yml` の `defaults` 設定により、レイアウトは自動的に適用される（記事は `post`、メモは `memo`）。

## ローカルサーバーの起動

以下のコマンドで、ローカル環境でサイトをプレビュー可能。
```bash
bundle exec jekyll serve --livereload
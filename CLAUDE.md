# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

NonNonSheet（Unity向けマスターデータ管理ツール）のドキュメントサイト。DocFXを使って静的HTMLサイトを生成し、GitHub Pagesにデプロイする。

## ビルドコマンド

```bash
# DocFXをインストール（初回またはアップデート時）
dotnet tool update -g docfx

# ドキュメントをビルド（出力先: _site/）
docfx ./docfx.json

# ローカルプレビューサーバーを起動（オプション）
docfx serve _site
```

ビルド成果物は `_site/` に出力される（`.gitignore` 対象）。

## アーキテクチャ

**ツール**: DocFX（.NET 8.x）  
**デプロイ**: GitHub Actions → GitHub Pages（`main` ブランチへのpushで自動実行）

### ドキュメント構成

```
articles/
├── en/   # 英語ドキュメント
└── ja/   # 日本語ドキュメント
api/      # C#ソースから自動生成されるAPIリファレンス（編集不可）
pdf/      # PDF生成用カバーページ（cover.html）
images/   # スクリーンショット等の画像
```

各セクションのナビゲーションは `toc.yml` で管理されている（ルート・各言語ディレクトリ・api/ それぞれに存在）。

### APIリファレンスの生成元

`docfx.json` の `metadata` セクションで、リポジトリ外の C# プロジェクトを参照している：

```
../NonNonSheet/src/NonNonSheet.Unity/Packages/com.nonnon-works.nonnon-sheet/{Editor,Runtime}/**
```

ローカルでAPIリファレンスをビルドするには、このパスに NonNonSheet リポジトリが存在する必要がある。

## コンテンツ編集のルール

- **ドキュメント本文**: `articles/en/` または `articles/ja/` 内の `.md` ファイルを編集
- **ナビゲーション追加**: 対応する `toc.yml` にエントリを追加
- **API リファレンス** (`api/` 内の `.yml`): 自動生成ファイルのため手動編集しない
- 英語・日本語のコンテンツは対称になるよう維持する

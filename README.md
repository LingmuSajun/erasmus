# 📖 Erasmus — Claude Code 書籍自動執筆システム

> *デジデリウス・エラスムス（1466–1536）—— ルネサンス最大の知識人。批評精神と教養の象徴。*

Claude Codeのプロジェクトカスタマイズ機能を活用した、ノンフィクション書籍の自動執筆環境です。

## セットアップ

### 1. プロジェクトをコピー

```bash
# 任意のディレクトリにコピー
cp -r erasmus ~/my-book
cd ~/my-book
```

### 2. PDF変換ツールのインストール（任意）

PDF出力が必要な場合、以下のいずれかをインストール:

```bash
# Ubuntu/WSL2
sudo apt install pandoc texlive-luatex texlive-lang-japanese

# または
sudo apt install wkhtmltopdf pandoc
```

### 3. 設定ファイルを編集

**`book-config.yml`** を開き、以下を設定:

- `title`: 書籍タイトル
- `chapters.count`: 章数
- `word_counts`: 各パートの文字数

**`book-theme.md`** を開き、以下を記述:

- テーマ、ターゲット読者、トーン、口調、キーワードなど
- テンプレートの `（例：...）` をすべて実際の内容に置き換える

**`review-criteria.md`** を開き、以下を記述:

- レビューで重視する観点と優先度
- 許容すること、絶対に見逃さないこと

### 4. Claude Codeを起動

```bash
cd ~/my-book
claude
```

## 使い方

### 一括実行（おすすめ）

```bash
/book-auto
```

すべてのステップを自動で実行します。途中でセッションが切れても、再度 `/book-auto` で中断箇所から再開します。

### ステップごとに実行

```bash
/book-outline          # Step 1: アウトライン作成
/book-write all        # Step 2: 全章を順番に執筆
/book-write 3          #         第3章だけ執筆
/book-review all       # Step 3: 全章レビュー + 全体チェック
/book-review 3         #         第3章だけレビュー
/book-revise all       # Step 4: 指摘のある全章を修正
/book-re-review all    # Step 5: 再レビュー
/book-finalize         # Step 6: 最終ファイル生成
/book-status           # いつでも進捗確認
```

## 出力ファイル

| ファイル | 場所 | 説明 |
| --- | --- | --- |
| アウトライン | `output/outline.md` | 全章の構成（セクション構成含む） |
| 原稿 | `output/drafts/*.md` | 各章の原稿 |
| 章別レビュー | `output/reviews/review-round{N}-ch{章}.md` | 章ごとのレビュー指摘 |
| 全体チェック | `output/reviews/review-round{N}-overall.md` | 全体整合性チェック結果 |
| 進捗 | `progress.yml` | 執筆・レビューの進捗状態 |
| **最終MD** | `output/final/{タイトル}.md` | 統合済み最終原稿 |
| **最終PDF** | `output/final/{タイトル}.pdf` | PDF版 |

## カスタマイズ

- **レビュー基準の変更**: `review-criteria.md` を編集
- **レビュー回数の変更**: `book-config.yml` の `review.max_rounds` を変更
- **執筆スキルの調整**: `.claude/skills/nonfiction-writing/SKILL.md` を編集
- **レビュアーの性格変更**: `.claude/agents/reviewer.md` を編集
- **新しいコマンドの追加**: `.claude/commands/` に `.md` ファイルを追加

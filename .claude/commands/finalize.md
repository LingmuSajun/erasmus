最終成果物を生成してください。

## 前提条件

- `output/drafts/` に完成した原稿が存在すること
- レビューで「完成」判定を受けていること（または最大回数到達）

## 手順

1. `book-config.yml` からタイトルと出力設定を読む
2. `output/drafts/` 内のすべての原稿ファイルを番号順に読み込む
3. 以下の構成で1つのMarkdownファイルに統合する
4. 統合したファイルを `output/final/{タイトル}.md` として保存する
5. PDF変換を実行し `output/final/{タイトル}.pdf` として保存する

## 統合フォーマット

```markdown
# {タイトル}

---

（はじめにの本文）

---

（第1章の本文）

---

（第2章の本文）

...

---

（おわりにの本文）
```

## 統合時の調整

- 各章の間にページ区切り（`---`）を挿入する
- 章末の「この章のポイント」はそのまま残す
- 章番号とタイトルのフォーマットを統一する（`# 第1章: タイトル` の形式）
- 全体を通して表記の揺れがないか最終チェックする（例: 「人工知能」と「AI」の混在）

## PDF変換

以下のコマンドでPDFを生成する。使用可能なツールに応じて切り替える:

### pandoc の場合
```bash
pandoc "output/final/{タイトル}.md" \
  -o "output/final/{タイトル}.pdf" \
  --pdf-engine=lualatex \
  -V documentclass=ltjsarticle \
  -V geometry:margin=2.5cm \
  -V fontsize=11pt \
  --toc \
  --toc-depth=2
```

### wkhtmltopdf の場合
まずMarkdownをHTMLに変換し、次にPDFに変換する:
```bash
pandoc "output/final/{タイトル}.md" -o "/tmp/book.html" --standalone --metadata title="{タイトル}"
wkhtmltopdf --encoding utf-8 "/tmp/book.html" "output/final/{タイトル}.pdf"
```

### いずれも使えない場合
PDFの生成をスキップし、Markdownファイルのみを最終成果物として案内する。
PDF変換ツールのインストール方法を提示する。

## 完了後

以下を表示する:
- 最終ファイルのパスとサイズ
- 総文字数
- 章ごとの文字数一覧
- レビューラウンド数
- 「執筆完了」メッセージ

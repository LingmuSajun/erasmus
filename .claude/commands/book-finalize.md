# 最終成果物の生成

最終成果物を生成してください。各ステップの開始・完了を都度ユーザーに報告しながら進めること。

## 前提条件

- `output/drafts/` に完成した原稿が存在すること
- レビューで「完成」判定を受けていること（または最大回数到達）

## 手順

### ステップ1: 設定読み込み

`book-config.yml` を読み、`title` を取得する。読み込んだら「タイトル: {タイトル}」と報告する。

### ステップ2: Markdownファイル統合（bashで実行）

原稿ファイルをAIのコンテキストに読み込まず、以下のbashスクリプトで直接結合する。

```bash
TITLE=$(grep '^title:' book-config.yml | sed 's/title: *"\(.*\)"/\1/')
OUTPUT="output/final/${TITLE}.md"
mkdir -p output/final

# タイトル行を書き出す
printf '# %s\n\n' "${TITLE}" > "${OUTPUT}"

# 各章ファイルを番号順に結合（見出しを1レベル下げ、セパレーター付き）
for f in $(ls output/drafts/*.md | sort); do
  printf '\n---\n\n' >> "${OUTPUT}"
  sed 's/^#\(#*\)/##\1/' "${f}" >> "${OUTPUT}"
  printf '\n' >> "${OUTPUT}"
done

echo "統合完了: ${OUTPUT}"
wc -m "${OUTPUT}"
```

実行後、完了メッセージとファイルサイズを報告する。

### ステップ3: PDF変換

`book-config.yml` の `pdf_engine` 設定に従い変換する。変換前に「PDF変換を開始します（エンジン: {エンジン名}）」と報告する。

#### wkhtmltopdf の場合

```bash
TITLE=$(grep '^title:' book-config.yml | sed 's/title: *"\(.*\)"/\1/')
pandoc "output/final/${TITLE}.md" -o "/tmp/book.html" \
  --standalone --metadata title="${TITLE}" \
  --metadata charset="utf-8"
wkhtmltopdf --encoding utf-8 "/tmp/book.html" "output/final/${TITLE}.pdf"
echo "PDF生成完了: output/final/${TITLE}.pdf"
```

#### pandoc の場合

```bash
TITLE=$(grep '^title:' book-config.yml | sed 's/title: *"\(.*\)"/\1/')
pandoc "output/final/${TITLE}.md" \
  -o "output/final/${TITLE}.pdf" \
  --pdf-engine=lualatex \
  -V documentclass=ltjsarticle \
  -V geometry:margin=2.5cm \
  -V fontsize=11pt \
  --toc --toc-depth=2
echo "PDF生成完了: output/final/${TITLE}.pdf"
```

#### いずれも使えない場合

PDFの生成をスキップし、Markdownファイルのみを最終成果物として案内する。
インストール方法: `sudo apt install wkhtmltopdf` または `sudo apt install pandoc texlive-full`

### ステップ4: 完了レポート

以下のbashで統計を収集して表示する:

```bash
TITLE=$(grep '^title:' book-config.yml | sed 's/title: *"\(.*\)"/\1/')
echo "=== 執筆完了レポート ==="
ls -lh "output/final/${TITLE}.md" "output/final/${TITLE}.pdf" 2>/dev/null
echo ""
echo "--- 章ごとの文字数 ---"
for f in $(ls output/drafts/*.md | sort); do
  count=$(wc -m < "${f}")
  echo "${f##*/}: ${count}字"
done
```

最後に「執筆完了」メッセージを表示する。

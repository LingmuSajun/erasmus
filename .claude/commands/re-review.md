# 再レビュー

修正後の原稿を再レビューしてください。

引数・動作は `/project:review` と同じ（章番号指定 / `all` / `overall`）。

## 前提条件

- `output/drafts/` に修正済みの原稿が存在すること
- `output/reviews/` に前ラウンドのレビューファイルが存在すること

## 手順

1. `progress.yml` を読み、現在のラウンド番号を確認する
2. `book-config.yml` のレビュー最大回数（`review.max_rounds`）を確認する
3. `review-criteria.md` を読み、レビュー基準を再確認する
4. `/project:review` と同じ2段階（章単位 → 全体整合性）でレビューする
5. 加えて、前回の指摘事項が適切に修正されているかを確認する
6. 修正により新たな問題が生じていないかを確認する
7. 結果を `output/reviews/review-round{N}-ch{章番号}.md` / `output/reviews/review-round{N}-overall.md` に保存する
8. `progress.yml` を更新する

## 判定基準

以下のすべてを満たす場合、「完成」と判定する:

- 前回の重要度「高」の指摘がすべて解消されている
- 重要度「中」の指摘の80%以上が解消されている
- 修正により新たな重大な問題が生じていない
- 全体の論理的一貫性が保たれている

## 最大レビュー回数に達した場合

- 残存する指摘事項を `output/reviews/remaining-issues.md` に記録する
- 「完成（条件付き）」と判定する
- ユーザーに残課題の一覧を表示し、`/project:finalize` を案内する

## 出力フォーマット

`/project:review` と同じフォーマットを使用する。
前回からの改善点は「✅ 解消済み」、未解消は「⚠️ 未解消」でマークする。

## 完了後

- 「完成」→ `/project:finalize` を案内
- 「修正が必要」→ `/project:revise` を案内
- 最大回数到達 → 残課題を表示の上 `/project:finalize` を案内

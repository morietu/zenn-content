---
title: "AI参拝ナビ開発日誌 #3: タグ選択をボタン押下で確定する検索UIに変更"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Nextjs", "Django", "TailwindCSS", "検索UI"]
published: true
---
## 🎯 今回のブランチ
`feature/search-confirm-button`

## 📝 やったこと
- 神社検索UIの挙動を改善
- タグをクリックしても即検索せず、検索ボタン押下でのみ検索確定するように変更
- `useEffect([selectedTags])` を削除し、`handleSearch` に検索処理を集約

## 🔍 実装ポイント

### フロントエンド (Next.js)
```tsx
// タグ＋名前検索をボタン押下で確定
const handleSearch = () => {
  fetchShrines({ name: query, tags: selectedTags })
}
修正前
タグを選択するたびに useEffect([selectedTags]) で API を呼んでいた

修正後
タグ選択は状態更新のみ

検索実行は「検索ボタンを押したとき」に限定

✅ 動作確認
タグを選んでも検索は走らない（選択状態だけ更新）

検索ボタンを押すと、入力済みの名前＋選択中のタグで検索が実行される

名前だけ／タグだけ／名前＋タグの組み合わせがOK

💡 学び
UI 仕様を「即時検索」から「確定検索」に変えるだけで、UX の印象が大きく変わる

状態管理と API 呼び出しを分離するのはシンプルで分かりやすい



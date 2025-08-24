---
title: "AI参拝ナビ開発日誌 #2: ご利益タグの複数選択フィルタを実装"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Nextjs", "Django", "TailwindCSS", "DRF"]
published: true
---
## 🎯 今回のブランチ
`feature/multi-tag-filter`

## 📝 やったこと
- 神社検索の「ご利益タグ」を **複数選択可能** にした
- フロントエンド：
  - `TagList` コンポーネントで複数タグを選択
  - 選択したタグをクエリに追加して API に送信
- バックエンド：
  - `ShrineViewSet` のフィルタを `get("tag")` → `getlist("tag")` に変更
  - `?tag=縁結び&tag=金運` の形式に対応
- エラーハンドリングを整理
  - 開発環境 → `console.error` で詳細ログ
  - 本番環境 → Sentry で収集する設計に変更

## 🔍 実装ポイント

### フロントエンド（React + Next.js）
```tsx
// 選択したタグを配列で管理
const [selectedTags, setSelectedTags] = useState<string[]>([])

// API 呼び出し時に複数クエリを付与
params.tags.forEach((t) => queryParams.append("tag", t))
バックエンド（Django + DRF）

# ShrineViewSet
tags = self.request.query_params.getlist("tag")
if tags:
    queryset = queryset.filter(goriyaku_tags__name__in=tags).distinct()
✅ 動作確認
タグを複数クリックすると、その条件で検索結果がフィルタされる

バックエンドを停止すると、画面には簡潔なエラーメッセージが出て
Console に詳細ログが表示される

💡 学び
DRF の getlist() を使うと簡単に「複数タグ対応」ができる

開発と本番でログの扱いを分ける設計を最初から入れておくと安心



---

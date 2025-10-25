# クイックスタート

5分で開発を始めるためのガイドです。

## 前提条件

- Node.js 18+
- PostgreSQL 12+ または Neon アカウント
- Git

## セットアップ（5分）

### 1. クローンとインストール（1分）

```bash
git clone https://github.com/FinShinjuku/timu2-RecipeRouter.git
cd timu2-RecipeRouter
npm install
```

### 2. 環境変数の設定（2分）

```bash
# .env ファイルを作成
cp .env.example .env
```

`.env` を編集して `DATABASE_URL` を設定：

**Neon を使う場合（推奨）:**
```bash
# https://neon.tech で無料アカウントを作成
# プロジェクトから接続文字列をコピー
DATABASE_URL=postgresql://user:password@ep-xxx.neon.tech/database
```

**ローカル PostgreSQL を使う場合:**
```bash
# PostgreSQL を起動
brew services start postgresql

# .env に設定
DATABASE_URL=postgresql://postgres:password@localhost:5432/minpaku_guard
```

### 3. データベースの初期化（1分）

```bash
npm run db:push
```

### 4. 開発サーバーの起動（1分）

```bash
npm run dev
```

### 5. ブラウザで確認

http://localhost:5173 をブラウザで開く

## 常用コマンド

```bash
# 開発サーバー起動
npm run dev

# TypeScript チェック
npm run check

# プロダクション ビルド
npm run build

# ビルト結果を実行
npm start

# データベースのスキーマ更新
npm run db:push
```

## 開発フロー

```bash
# 新しいブランチを作成
git checkout -b feature/your-feature

# コード編集
# ...

# 変更をコミット
git add .
git commit -m "Add your feature"

# GitHub にプッシュ
git push origin feature/your-feature

# プルリクエストを作成
gh pr create
```

## よくある問題

| 問題 | 解決策 |
|------|--------|
| DATABASE_URL が見つからない | `.env` ファイルを作成して `DATABASE_URL` を設定 |
| ポート 3000 が使用中 | `PORT=3001 npm run dev` で別のポートで実行 |
| PostgreSQL に接続できない | `brew services start postgresql` で起動 |
| npm install エラー | `npm cache clean --force` でキャッシュをクリア |

詳細は [DEVELOPMENT.md](./DEVELOPMENT.md) を参照してください。

---

サポートが必要？ [GitHub Issues](https://github.com/FinShinjuku/timu2-RecipeRouter/issues) で質問してください。

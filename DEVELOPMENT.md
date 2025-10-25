# ローカル開発環境セットアップガイド

## 概要

このドキュメントは、timu2-RecipeRouter (Minpaku Guard) をローカルマシンで開発するためのセットアップガイドです。

## 必要な環境

- **Node.js**: v18 以上
- **npm**: v11 以上
- **PostgreSQL**: v12 以上（ローカルまたはリモート）
- **Git**: 最新版

## ステップバイステップ セットアップ

### 1. リポジトリのクローン

```bash
git clone https://github.com/FinShinjuku/timu2-RecipeRouter.git
cd timu2-RecipeRouter
```

### 2. 依存パッケージのインストール

```bash
npm install
```

### 3. PostgreSQL のセットアップ

#### オプション A: Neon (クラウドベース - 推奨)

1. https://neon.tech にアクセスしてアカウントを作成
2. 新しいプロジェクトを作成
3. 接続文字列をコピー

#### オプション B: ローカル PostgreSQL

**macOS での起動方法:**

```bash
# Homebrewでインストール済みの場合
brew services start postgresql

# ターミナルで確認
psql --version
```

**ローカルデータベースの作成:**

```bash
# PostgreSQL に接続
psql postgres

# データベースを作成
CREATE DATABASE minpaku_guard;

# ユーザーを作成（オプション）
CREATE USER minpaku_user WITH PASSWORD 'your-secure-password';
GRANT ALL PRIVILEGES ON DATABASE minpaku_guard TO minpaku_user;

# 接続文字列
# postgresql://minpaku_user:your-secure-password@localhost:5432/minpaku_guard
```

### 4. 環境変数の設定

`.env` ファイルをプロジェクトルートに作成し、以下の内容を設定します：

```bash
# Database URL (Neon または ローカル PostgreSQL)
DATABASE_URL=postgresql://user:password@host:port/database

# サーバー設定
PORT=3000
NODE_ENV=development

# セッション秘密鍵（本番環境では変更必須）
SESSION_SECRET=dev-secret-key-please-change-in-production

# CORS設定
CORS_ORIGIN=http://localhost:5173

# ファイルアップロード
MAX_FILE_SIZE=5242880
UPLOAD_DIR=./attached_assets

# 外部API キー（オプション）
OPENAI_API_KEY=your-openai-api-key
DIFY_API_KEY=your-dify-api-key

# デバッグ設定
DEBUG=true
VERBOSE_LOGGING=true
```

#### 環境変数の例

**Neon の場合:**
```
DATABASE_URL=postgresql://neon_user:password@ep-*.neon.tech/neon_db
```

**ローカル PostgreSQL の場合:**
```
DATABASE_URL=postgresql://minpaku_user:your-password@localhost:5432/minpaku_guard
```

### 5. データベーススキーマの作成

```bash
# Drizzle Kit を使用してマイグレーションを実行
npm run db:push
```

このコマンドが失敗する場合は、以下をチェック：
- `DATABASE_URL` が正しく設定されているか
- PostgreSQL サーバーが実行中か
- ネットワーク接続が正常か

### 6. 開発サーバーの起動

```bash
# フロントエンド（Vite）とバックエンド（Express）を同時に起動
npm run dev
```

出力例：
```
  VITE v5.4.20  ready in XXX ms

  ➜  Local:   http://localhost:5173/
  ➜  press h to show help

Server running on port 3000
```

### 7. ブラウザでアクセス

http://localhost:5173 をブラウザで開くと、アプリケーションが表示されます。

## 開発作業フロー

### 新機能の開発

```bash
# 1. 新しいブランチを作成
git checkout -b feature/your-feature-name

# 2. コード変更
# ... 編集作業 ...

# 3. 変更をコミット
git add .
git commit -m "Add your feature description"

# 4. GitHub にプッシュ
git push origin feature/your-feature-name

# 5. プルリクエストを作成
gh pr create
```

### ビルドとテスト

```bash
# TypeScript チェック
npm run check

# プロダクション ビルド
npm run build

# ビルト結果を実行（テスト）
npm start
```

## よくあるトラブルシューティング

### エラー: DATABASE_URL が見つからない

```
Error: DATABASE_URL, ensure the database is provisioned
```

**解決策:**
1. `.env` ファイルが存在するか確認
2. `DATABASE_URL` が設定されているか確認
3. PostgreSQL サーバーが実行中か確認

```bash
# PostgreSQL サーバーの状態確認（macOS）
brew services list | grep postgresql

# 起動していない場合は起動
brew services start postgresql
```

### エラー: ポート 3000 が既に使用されている

```
Error: listen EADDRINUSE: address already in use :::3000
```

**解決策:**

```bash
# 別のポートで実行
PORT=3001 npm run dev

# または既存のプロセスを終了
lsof -ti:3000 | xargs kill -9
```

### エラー: npm install で失敗

```bash
# キャッシュをクリア
npm cache clean --force

# node_modules を削除して再インストール
rm -rf node_modules package-lock.json
npm install
```

### PostgreSQL に接続できない

**確認項目:**
- PostgreSQL サーバーが実行中か
- `DATABASE_URL` の形式が正しいか（例: `postgresql://user:password@localhost/dbname`）
- ファイアウォール設定
- ユーザー名とパスワードが正しいか

```bash
# 直接接続テスト
psql postgresql://user:password@host:port/database
```

## Docker を使用したローカル開発

Docker がインストール済みの場合、PostgreSQL をコンテナで実行できます：

```bash
# PostgreSQL コンテナを起動
docker run --name minpaku-postgres \
  -e POSTGRES_USER=minpaku_user \
  -e POSTGRES_PASSWORD=minpaku_password \
  -e POSTGRES_DB=minpaku_guard \
  -p 5432:5432 \
  -d postgres:15

# コンテナの状態確認
docker ps | grep minpaku-postgres

# 接続文字列
DATABASE_URL=postgresql://minpaku_user:minpaku_password@localhost:5432/minpaku_guard

# コンテナの停止
docker stop minpaku-postgres

# コンテナの削除
docker rm minpaku-postgres
```

## デバッグモード

開発中はデバッグモードを有効にすると便利です：

```bash
# .env に設定
DEBUG=true
VERBOSE_LOGGING=true

# ターミナルで実行
npm run dev
```

## パフォーマンス測定

```bash
# バンドルサイズの確認
npm run build

# TypeScript の型チェック
npm run check
```

## 外部 API キーの管理

### OpenAI API キー（顔認証・人数カウント用）

1. https://platform.openai.com/api-keys にアクセス
2. API キーを生成
3. `.env` に設定：
   ```
   OPENAI_API_KEY=sk-...
   ```

### Dify API キー（AI ワークフロー連携用）

1. Dify インスタンスにログイン
2. API キーを生成
3. `.env` に設定：
   ```
   DIFY_API_KEY=...
   ```

## コードスタイルと規約

このプロジェクトは以下の規約を遵守しています：

- **言語**: TypeScript
- **フォーマッタ**: Prettier（自動）
- **リンター**: ESLint（推奨）
- **命名規則**: camelCase（変数・関数）, PascalCase（コンポーネント・クラス）
- **ディレクトリ構造**: 機能ベースのモジュール分割

## 参考リンク

- [Vite ドキュメント](https://vitejs.dev/)
- [React ドキュメント](https://react.dev/)
- [Express.js ドキュメント](https://expressjs.com/)
- [Drizzle ORM ドキュメント](https://orm.drizzle.team/)
- [PostgreSQL ドキュメント](https://www.postgresql.org/docs/)
- [TypeScript ドキュメント](https://www.typescriptlang.org/docs/)

## サポートが必要な場合

- GitHub Issues で問題を報告
- GitHub Discussions で質問
- プルリクエストで機能提案

---

**作成日**: 2025年10月25日
**最終更新**: 2025年10月25日

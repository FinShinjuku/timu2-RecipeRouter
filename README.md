# timu2-RecipeRouter 🏠

> **バケーション レンタル管理ダッシュボード** - 物件管理を効率的にするための統合プラットフォーム

## 概要

timu2-RecipeRouter は、バケーションレンタル物件の管理を簡素化する、モダンで機能豊富な Web ベースのダッシュボード管理システムです。予約管理から遠隔デバイス制御、リアルタイムアラート監視までを一つのプラットフォームで実現します。

**開発環境**: Replit で作成された本格的な フルスタック Web アプリケーション

### 主な特徴

✨ **リアルタイム監視**
- ゲスト在室状況の自動検知
- 異常検知アラートの即座通知
- 入退室ログのリアルタイム追跡

🔐 **セキュリティ & 認証**
- 顔認証によるゲスト検証
- スマートロック統合
- セッションベースの認証管理

📊 **包括的な管理機能**
- 予約管理システム
- ゲスト情報管理
- デバイス制御パネル
- コミュニケーション履歴

🎨 **ユーザーフレンドリーな UI**
- Material Design 原則に基づく設計
- ダークモード対応
- モバイル最適化（iPhone 13 対応）
- レスポンシブレイアウト

## テックスタック

### フロントエンド
- **React 18** - UI フレームワーク
- **Vite** - ビルドツール
- **TailwindCSS** - スタイリング
- **Radix UI** - アクセシブルなコンポーネント
- **React Hook Form** - フォーム管理
- **TanStack React Query** - データ取得・キャッシング
- **Framer Motion** - アニメーション

### バックエンド
- **Express.js** - Web フレームワーク
- **Node.js** - ランタイム
- **Passport.js** - 認証ライブラリ
- **Drizzle ORM** - データベース抽象化層
- **PostgreSQL** - データベース（Neon）
- **WebSocket** - リアルタイム通信

### インフラ & ツール
- **TypeScript** - 型安全性
- **esbuild** - バンドラー
- **Drizzle Kit** - マイグレーション管理
- **Multer** - ファイルアップロード処理

## クイックスタート

### 前提条件
- Node.js 18+
- npm / yarn
- PostgreSQL データベース（オプション：Neon）

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/FinShinjuku/timu2-RecipeRouter.git
cd timu2-RecipeRouter

# 依存パッケージをインストール
npm install
```

### 開発

```bash
# 開発サーバーを起動（フロント + バック同時）
npm run dev

# ブラウザで http://localhost:5173 にアクセス
```

### ビルド

```bash
# プロダクション向けにビルド
npm run build

# ビルト結果を実行
npm run start
```

### データベース管理

```bash
# スキーマをデータベースに反映
npm run db:push

# スキーマをチェック
npm run check
```

## プロジェクト構造

```
.
├── client/                      # React フロントエンド
│   ├── src/
│   │   ├── components/         # UI コンポーネント
│   │   ├── pages/             # ページコンポーネント
│   │   ├── hooks/             # カスタムフック
│   │   ├── lib/               # ユーティリティ関数
│   │   └── App.tsx            # メインコンポーネント
│   └── index.html
├── server/                      # Express.js バックエンド
│   ├── routes/                 # API ルート
│   ├── middleware/             # ミドルウェア
│   ├── db/                     # データベーススキーマ
│   └── index.ts                # エントリポイント
├── shared/                      # 共有ユーティリティ
├── attached_assets/            # アップロード済みアセット
├── design_guidelines.md         # UI/UX ガイドライン
├── replit.md                    # Replit設定ドキュメント
├── vite.config.ts              # Vite設定
├── tsconfig.json               # TypeScript設定
├── tailwind.config.ts           # TailwindCSS設定
├── drizzle.config.ts           # Drizzle設定
├── components.json             # shadcn/ui設定
├── postcss.config.js           # PostCSS設定
└── package.json                # プロジェクト定義
```

## 主要機能

### 1. ダッシュボード
- 当日の到着・出発状況
- アクティブアラート一覧
- デバイス状態監視
- 予約概要統計

### 2. ゲスト管理
- ゲスト詳細情報表示
- 顔認証データ管理
- 身分証画像保管
- コミュニケーション履歴

### 3. アラート管理
- 異常検知の自動通知
- 重大度ベースのフィルタリング
- アナログ受信確認
- ゲストへの自動連絡

### 4. デバイス制御
- スマートロック制御
- ブレーカー管理
- 遠隔オン/オフ切り替え
- 操作ログ記録

### 5. コミュニケーション
- テンプレートメッセージ
- メール/SMS 送信
- 送信履歴追跡
- 自動メッセージ機能

## API ドキュメント

主要エンドポイント一覧：

```
# 認証
POST   /api/auth/login
POST   /api/auth/logout
POST   /api/auth/register

# ゲスト
GET    /api/guests
GET    /api/guests/:id
PUT    /api/guests/:id
POST   /api/guests/:id/verify

# 予約
GET    /api/bookings
POST   /api/bookings
PUT    /api/bookings/:id
DELETE /api/bookings/:id

# デバイス
GET    /api/devices
POST   /api/devices/:id/control
GET    /api/devices/:id/logs

# アラート
GET    /api/alerts
PATCH  /api/alerts/:id
```

## 環境変数設定

`.env` ファイルを作成して以下を設定します：

```bash
# データベース
DATABASE_URL=postgresql://...

# セッション
SESSION_SECRET=your-secret-key

# API設定
PORT=3000
NODE_ENV=development

# Replit（オプション）
REPLIT_DEPLOYMENT=true
```

## デザイン原則

本プロジェクトは **Utility-First Dashboard System** の設計原則に従っています：

- **操作効率重視** - 視覚的なフレアよりも情報階層の明確さ
- **情報設計** - 迅速な意思決定のための効果的なデータ表示
- **一貫性** - 日常的な使用にための信頼性のあるパターン
- **レスポンシブ** - デスクトップからモバイルまで完全対応

詳細は [design_guidelines.md](./design_guidelines.md) を参照してください。

## 開発ブランチ

- `main` - 本番環境対応リリース版
- `replit-agent` - 開発・実験用ブランチ

## スクリーンショット & デモ

[スクリーンショット・デモビデオはこちら]

## 利用技術の詳細情報

- [Replit Documentation](https://docs.replit.com/) - 開発環境
- [Vite Documentation](https://vitejs.dev/) - ビルドツール
- [TailwindCSS Documentation](https://tailwindcss.com/) - スタイリング
- [React Documentation](https://react.dev/) - UI フレームワーク
- [Express.js Documentation](https://expressjs.com/) - バックエンド

## ライセンス

MIT License - 詳細は [LICENSE](./LICENSE) を参照

## サポート & コントリビューション

バグ報告や機能提案は [GitHub Issues](https://github.com/FinShinjuku/timu2-RecipeRouter/issues) にお願いします。

プルリクエストも歓迎します！開発に参加する場合は以下をお願いします：

1. フォークしてフィーチャーブランチを作成（`git checkout -b feature/AmazingFeature`）
2. 変更をコミット（`git commit -m 'Add AmazingFeature'`）
3. ブランチにプッシュ（`git push origin feature/AmazingFeature`）
4. プルリクエストをオープン

## トラブルシューティング

### npm install でエラーが出た場合

```bash
# キャッシュをクリア
npm cache clean --force

# 再度インストール
npm install
```

### ポート 3000 が既に使用されている場合

```bash
# 別のポートで実行
PORT=3001 npm run dev
```

### データベース接続エラー

環境変数 `DATABASE_URL` が正しく設定されているか確認してください。

## 開発チーム

開発: Team 2 (timu2)
フレームワーク: Replit

---

**最後の更新**: 2025年10月25日
**バージョン**: 1.0.0
**ステータス**: Active Development

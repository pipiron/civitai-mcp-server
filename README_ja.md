# Civitai MCP Server

AI アシスタントが Civitai の膨大な AI モデル・クリエイター・生成画像コレクションにアクセスできる Model Context Protocol (MCP) サーバーです。お使いの MCP 対応 AI アシスタントから、モデルの閲覧・検索・発見をシームレスに行えます。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

## 機能

### 🔍 **モデル検索**
- **高度な検索**: タイプ・クリエイター・タグなど柔軟なフィルタでモデルを検索
- **カテゴリ閲覧**: モデルタイプ別（Checkpoint、LoRA、ControlNet 等）に探索
- **人気・トレンド**: 最もダウンロードされた・高評価のモデルを発見
- **最新モデル**: 新着モデルをリアルタイムで確認
- **ハッシュ検索**: ファイルハッシュによるモデル検証

### 👨‍💻 **クリエイター & コミュニティ**
- **クリエイタープロフィール**: モデル作者を閲覧・検索
- **クリエイター作品一覧**: 特定クリエイターの全モデルを確認
- **タグシステム**: Civitai の包括的なタグ体系でモデルを探索

### 🖼️ **生成画像**
- **画像ギャラリー**: 詳細なメタデータ付きで AI 生成画像を閲覧
- **生成パラメータ**: プロンプト・設定・モデル情報にアクセス
- **コミュニティショーケース**: コミュニティの作品を発見

### 📊 **モデル情報**
- **詳細スペック**: モデルの仕様・バージョン・ファイル情報を完全取得
- **バージョン履歴**: モデルの更新・改善を追跡
- **ダウンロード URL**: 認証対応のダウンロードリンクを直接取得
- **コンテンツ安全性**: pickle・ウイルススキャン結果にアクセス

## インストール

### 前提条件
- Node.js 18+
- npm または yarn
- Civitai API キー（任意。レートリミット向上のため推奨）

### クイックスタート

1. **リポジトリのクローン:**
```bash
git clone https://github.com/pipiron/civitai-mcp-server.git
cd civitai-mcp-server
```

2. **依存関係のインストール:**
```bash
npm install
```

3. **ビルド:**
```bash
npm run build
```

4. **API キーの設定（任意）:**
```bash
export CIVITAI_API_KEY="your_api_key_here"
```

5. **サーバー起動:**
```bash
npm start
```

### Civitai API キーの取得

1. [Civitai アカウント設定](https://civitai.com/user/account) にアクセス
2. Civitai アカウントにログイン
3. 新しい API キーを生成
4. キーをコピーして環境変数に設定

## 設定

### MCP クライアントの設定

#### Claude Desktop
`claude_desktop_config.json` に追記:
```json
{
  "mcpServers": {
    "civitai": {
      "command": "node",
      "args": ["/path/to/civitai-mcp-server/dist/index.js"],
      "env": {
        "CIVITAI_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

#### Claude Code
```bash
claude mcp add civitai -e CIVITAI_API_KEY=your_api_key_here -- node /path/to/civitai-mcp-server/dist/index.js
```

#### その他の MCP クライアント
クライアントのドキュメントに従い、以下の設定を使用:
- **コマンド**: `node /path/to/civitai-mcp-server/dist/index.js`
- **トランスポート**: stdio
- **環境変数**: `CIVITAI_API_KEY=your_key`

## 使用例

### 基本的なモデル検索
```
SDXL LoRA のアニメ関連モデルを検索:
- ツール: search_models
- Query: "anime"
- Types: ["LORA"]
- BaseModels: ["SDXL 1.0"]
```

### 人気モデルの取得
```
今週最もダウンロードされたモデルを取得:
- ツール: get_popular_models
- Period: "Week"
- Limit: 10
```

### モデル詳細の取得
```
特定モデルの詳細情報を取得:
- ツール: get_model
- ModelId: 12345
```

### 生成画像の閲覧
```
最新の AI 生成画像を閲覧:
- ツール: browse_images
- Sort: "Newest"
- Limit: 50
- NSFW: "None"
```

## 利用可能なツール

| ツール | 説明 | 主なパラメータ |
|--------|------|--------------|
| `search_models` | フィルタ付きモデル検索 | `query`, `types`, `sort`, `baseModels` |
| `get_model` | モデル詳細情報の取得 | `modelId` |
| `get_model_version` | バージョン詳細の取得 | `modelVersionId` |
| `get_model_version_by_hash` | ハッシュによるモデル検索 | `hash` |
| `browse_images` | 生成画像の閲覧 | `sort`, `period`, `modelId` |
| `get_creators` | クリエイター検索 | `query`, `limit` |
| `get_tags` | タグの閲覧 | `query`, `limit` |
| `get_popular_models` | 人気モデルの取得 | `period`, `limit` |
| `get_latest_models` | 最新モデルの取得 | `limit` |
| `get_top_rated_models` | 高評価モデルの取得 | `period`, `limit` |
| `search_models_by_tag` | タグ指定でモデル検索 | `tag`, `sort` |
| `search_models_by_creator` | クリエイター指定で検索 | `username`, `sort` |
| `get_models_by_type` | モデルタイプでフィルタ | `type`, `sort` |
| `get_download_url` | ダウンロード URL の取得 | `modelVersionId` |

## API リファレンス

### モデルタイプ
- `Checkpoint` - Stable Diffusion フルモデル
- `LORA` - Low-Rank Adaptation モデル
- `LoCon` - Local Conditioning LoRA
- `DoRA` - DoRA モデル
- `TextualInversion` - Embedding モデル
- `Hypernetwork` - Hypernetwork モデル
- `ControlNet` - ControlNet モデル
- `AestheticGradient` - Aesthetic Gradient モデル
- `VAE` - VAE モデル
- `Upscaler` - アップスケーラー
- `Poses` - ポーズモデル

### ソート順
- `Highest Rated` - コミュニティの高評価順
- `Most Downloaded` - ダウンロード数順
- `Newest` - 新着順

### 期間
- `AllTime` - 全期間
- `Year` - 過去 12 ヶ月
- `Month` - 過去 30 日
- `Week` - 過去 7 日
- `Day` - 過去 24 時間

## API カバレッジ

Civitai API v1 の主要エンドポイントをすべて実装:

- ✅ `/api/v1/models` - モデルの一覧・検索
- ✅ `/api/v1/models/:id` - 特定モデルの取得
- ✅ `/api/v1/model-versions/:id` - モデルバージョンの取得
- ✅ `/api/v1/model-versions/by-hash/:hash` - ハッシュによるバージョン取得
- ✅ `/api/v1/images` - 画像の閲覧
- ✅ `/api/v1/creators` - クリエイター一覧
- ✅ `/api/v1/tags` - タグ一覧
- ✅ 認証対応のダウンロード URL

## コンテンツフィルタリング

Civitai のコンテンツフィルタリングシステムに対応:
- NSFW コンテンツレベル（None / Soft / Mature / X）
- 商用利用権限
- モデルライセンスオプション

## エラーハンドリング

以下のエラーを包括的に処理:
- API レートリミット
- ネットワーク接続の問題
- 無効なパラメータ
- 認証エラー
- データバリデーション

## 開発

### プロジェクト構成
```
civitai-mcp-server/
├── src/
│   ├── index.ts          # メインサーバー実装
│   ├── civitai-client.ts # Civitai API クライアント
│   └── types.ts          # TypeScript 型定義
├── dist/                 # コンパイル済み JavaScript
├── tests/                # テストファイル
└── docs/                 # 追加ドキュメント
```

### ソースからのビルド
```bash
# 依存関係のインストール
npm install

# TypeScript ビルド
npm run build

# テスト実行
npm test

# ホットリロードで開発
npm run dev
```

## 制限事項

- **レートリミット**: Civitai API にはレートリミットがあります。API キーを設定することで上限が緩和されます。
- **NSFW コンテンツ**: アカウント設定によって一部コンテンツがフィルタされる場合があります。
- **モデルの可用性**: 一部モデルは一時的に利用不可または認証が必要な場合があります。

## トラブルシューティング

### よくある問題

**サーバーが起動しない:**
- Node.js 18+ がインストールされているか確認
- 依存関係がインストールされているか確認（`npm install`）
- ビルドが正常に完了しているか確認（`npm run build`）

**API レートリミット:**
- Civitai API キーを取得して `CIVITAI_API_KEY` 環境変数に設定
- リクエスト頻度を下げる

**モデルが見つからない:**
- モデル ID が正しいか確認
- モデルが削除または非公開になっていないか確認
- 検索クエリのスペルを確認

## ライセンス

MIT License - 詳細は LICENSE ファイルを参照

## 関連プロジェクト

- [Model Context Protocol](https://github.com/modelcontextprotocol/specification) - MCP 仕様
- [Civitai](https://civitai.com) - AI モデル共有プラットフォーム
- [MCP Servers](https://github.com/modelcontextprotocol/servers) - 公式 MCP サーバー実装

## サポート

- 🐛 **バグ報告**: [GitHub Issues](https://github.com/pipiron/civitai-mcp-server/issues)
- 💡 **機能リクエスト**: [GitHub Discussions](https://github.com/pipiron/civitai-mcp-server/discussions)
- 📚 **ドキュメント**: [Civitai API リファレンス](https://github.com/civitai/civitai/wiki/REST-API-Reference)
- 🔧 **MCP ドキュメント**: [Model Context Protocol](https://modelcontextprotocol.io/)

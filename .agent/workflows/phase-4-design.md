---
description: Phase 4 設計 - 基本設計・DB設計・画面設計・API設計などを作成する
---

# Phase 4: 設計ワークフロー

## 目的・概要

このフェーズでは、Phase 1〜3 で確定した要件・技術・ガイドラインに基づいて、各種設計書を作成し `docs/design/` 配下に文書化する。

設計書は実装の「設計図」となる。職人（CC）が**迷いなく実装できる詳細度**を目指す。

---

## 担当者

| 役割 | 担当 |
|------|------|
| 主導 | AG（建築家） |
| 承認 | User（施主） |

---

## 前提条件

- [ ] Phase 3 が完了していること
- [ ] `docs/guidelines/coding.md` が存在すること
- [ ] `PRODUCT.md` の Current Phase が Phase 4 であること

---

## ディレクトリ構成

```
docs/design/
├── index.md              # 設計書全体の目次・参照マップ
├── basic-design.md       # 基本設計（必須）
├── db/
│   ├── index.md          # テーブル一覧、ER図
│   ├── [table].md        # 各テーブル詳細
│   └── ...
├── api/
│   ├── index.md          # エンドポイント一覧
│   ├── [resource].md     # 各リソースのAPI詳細
│   └── ...
└── screens/
    ├── index.md          # 画面一覧、遷移図
    ├── _assets/          # 画像アセット（ロゴ、アイコン等）
    │   ├── logo.png
    │   ├── icon-search.svg
    │   └── ...
    ├── _components/      # 共通コンポーネント
    │   ├── index.md
    │   ├── header/
    │   │   ├── design.md
    │   │   └── mockup.html
    │   ├── footer/
    │   └── ...
    ├── [screen]/
    │   ├── design.md     # 画面仕様（アニメーション・リンク含む）
    │   └── mockup.html   # デザインモック（静的）
    └── ...
```
```

---

## Step 1: 必要な設計書の判定

`docs/requirements.md` の内容に基づいて、必要な設計書を特定する：

| 設計書 | 条件 |
|--------|------|
| 基本設計書 (`basic-design.md`) | **必須** |
| DB設計書 (`db/`) | データベース使用時 |
| 画面設計書 (`screens/`) | UI有りの場合 |
| API設計書 (`api/`) | API有りの場合 |

> ユーザーに「以下の設計書を作成します」と確認を取る

---

## Step 2: 基本設計書の作成

`docs/design/basic-design.md` を作成する。

### 必須項目

```markdown
# 基本設計書

## 1. システム概要
プロジェクトの目的と解決する課題の要約

## 2. アーキテクチャ
- 技術構成（tech-stack.md からの参照）
- システム構成図（Mermaid推奨）

## 3. 主要コンポーネントと責務
| コンポーネント | 責務 |
|--------------|------|
| フロントエンド | UI表示、ユーザー操作 |
| API | ビジネスロジック |
| DB | データ永続化 |

## 4. データフロー
主要なユースケースのデータフロー図
```

---

## Step 3: DB設計（DB使用時）

### 3.1 index.md の作成

```markdown
# データベース設計

## テーブル一覧

| テーブル | 説明 | 詳細 |
|---------|------|------|
| users | ユーザー情報 | [users.md](./users.md) |
| recipes | レシピ情報 | [recipes.md](./recipes.md) |

## ER図

（Mermaid で記述）
```

### 3.2 各テーブルの詳細

`db/[table].md` を作成：

```markdown
# [テーブル名] テーブル

## 概要
このテーブルの目的

## 関連
- API: [関連APIへのリンク]
- 画面: [関連画面へのリンク]

## カラム定義

| カラム | 型 | NULL | 制約 | 説明 |
|--------|-----|------|------|------|
| id | UUID | NO | PK | 主キー |
| name | VARCHAR(100) | NO | - | 名前 |
| created_at | TIMESTAMP | NO | DEFAULT NOW() | 作成日時 |

## インデックス

| 名前 | カラム | 種類 |
|------|--------|------|
| idx_name | name | BTREE |

## リレーション

| 対象 | 関係 | 外部キー |
|------|------|---------|
| ingredients | 1:N | recipe_id |
```

---

## Step 4: API設計（API有りの場合）

### 4.1 index.md の作成

```markdown
# API設計

## 認証方式
Bearer Token / Cookie / なし

## エンドポイント一覧

| メソッド | パス | 説明 | 詳細 |
|---------|------|------|------|
| GET | /api/recipes | レシピ一覧取得 | [recipes.md](./recipes.md#get-apirecipes) |
| GET | /api/recipes/{id} | レシピ詳細取得 | [recipes.md](./recipes.md#get-apirecipesid) |
| POST | /api/recipes | レシピ作成 | [recipes.md](./recipes.md#post-apirecipes) |
```

### 4.2 各リソースの詳細

`api/[resource].md` を作成：

```markdown
# /api/[resource]

## 関連
- DB: [関連テーブルへのリンク]
- 画面: [関連画面へのリンク]

---

## GET /api/[resource]

### 概要
一覧取得

### 認可
誰がアクセス可能か

### リクエスト

| パラメータ | 位置 | 型 | 必須 | 説明 |
|-----------|------|-----|------|------|
| limit | query | number | No | 取得件数（デフォルト20） |

### レスポンス

```json
{
  "items": [
    { "id": "xxx", "name": "xxx" }
  ],
  "total": 100
}
```

### エラー

| ステータス | 条件 | レスポンス |
|-----------|------|-----------|
| 401 | 未認証 | `{ "error": "Unauthorized" }` |
| 500 | サーバーエラー | `{ "error": "Internal Server Error" }` |
```

---

## Step 5: 画面設計（UI有りの場合）

画面設計はAGが仕様を言語化し、**CCがHTMLベースのデザインモックを生成**する。

### 5.0 配色と必須アセットの準備

デザインモック作成前に、以下を決定：

1. **配色の決定**
   - ブランドカラー（メイン、サブ）
   - 背景色、テキスト色
   - design.mdに配色を記載

2. **必須アセットの生成**（Nano Banana）
   - ロゴ
   - 確定しているアイコン
   - 出力先: `docs/design/screens/_assets/`

> 追加アセットは、デザインモック確認後に必要に応じて生成

### 5.1 共通コンポーネントを先に作成

`screens/_components/` 配下に共通要素を定義：

```
_components/
├── index.md          # 共通コンポーネント一覧
├── header/
│   ├── design.md     # 仕様
│   └── mockup.html
├── footer/
│   ├── design.md
│   └── mockup.html
└── ...
```

#### 作成手順

1. **AG: design.md を作成**
2. **AG: CCにデザインモック生成を依頼**
3. **CC: mockup.html を生成**
4. **ユーザー: 確認・フィードバック**

### 5.2 画面一覧の作成

`screens/index.md`:

```markdown
# 画面設計

## 画面一覧

| 画面 | 対象ユーザー | 概要 | 詳細 |
|------|-------------|------|------|
| ホーム | 全員 | トップページ | [home/](./home/) |
| レシピ一覧 | 全員 | レシピ検索 | [recipe-list/](./recipe-list/) |

## 画面遷移図

（Mermaid で記述）

## 参照マップ

| 画面 | 使用API | 使用テーブル |
|------|--------|-------------|
| recipe-list | GET /api/recipes | recipes |
| recipe-detail | GET /api/recipes/{id} | recipes, ingredients |
```

### 5.3 各画面の詳細

`screens/[screen]/design.md`:

```markdown
# [画面名]

## 概要
この画面の目的

## 配色
- メインカラー: #XXXXXX
- サブカラー: #XXXXXX
- 背景: #XXXXXX

## 関連
- API: [使用するAPIへのリンク]
- DB: [参照するテーブルへのリンク]

## レイアウト構成
- ヘッダー: [headerコンポーネントを配置]
- メイン: レシピカードのグリッド表示（3列）
- 各カード: サムネイル、レシピ名、作成者
- フッター: [footerコンポーネントを配置]

## デザインモック
[mockup.html](./mockup.html) 参照（静的ビジュアル）

## 表示項目

| 項目 | データ元 | 備考 |
|------|---------|------|
| レシピ名 | recipes.name | - |
| 作成者 | users.name | - |

## 操作・インタラクション

| 操作 | 動作 | 遷移先 |
|------|------|--------|
| カードクリック | 詳細画面へ遷移 | /recipes/{id} |
| お気に入りボタン | お気に入り登録 | - |

## アニメーション

| 要素 | アニメーション | タイミング |
|------|----------------|----------|
| カード | ホバー時に拡大 | 0.2s ease |
| フェードイン | ページ読み込み時 | 0.3s |

## 状態遷移

| 状態 | 条件 | 表示 |
|------|------|------|
| ローディング | データ取得中 | スケルトン |
| 表示 | データ取得成功 | コンテンツ |
| エラー | 取得失敗 | エラーメッセージ |
| 空 | データ0件 | 空状態メッセージ |
```

> ※ design.mdにはアニメーション・リンクの仕様を記載。mockup.htmlは静的ビジュアルのみ。

### 5.4 CCへのデザインモック生成依頼

AGがdesign.mdを作成したら、CCにHTMLデザインモック生成を依頼する。

#### 依頼コマンド

```bash
claude -p "docs/design/screens/[screen]/design.md を読み込んで、
HTMLベースのデザインモックを生成してください。

## 生成ルール
- フロントエンドデザインを使用して
- 1ファイルでレスポンシブ対応（PC/スマホ）
- 日本語で表記
- design.mdの配色に従う（Figmaライクな本格デザイン）
- アニメーション・リンクは不要（静的ビジュアルのみ）
- 共通コンポーネントはdesign.mdの指示に従って配置
- アセットはdocs/design/screens/_assets/から参照

## 出力先
docs/design/screens/[screen]/mockup.html" --dangerously-skip-permissions
```

### 5.5 追加アセットの生成

デザインモック確認後、不足しているアセットをNano Bananaで生成：
- アイコン
- イラスト
- 背景画像等

出力先: `docs/design/screens/_assets/`

---

## HTMLデザインモック仕様

### 基本ルール

1.  **日本語で表記**
2.  **1ファイルでレスポンシブ対応**（メディアクエリ使用）
3.  **design.mdの配色に従う**（Figmaライクな本格デザイン）
4.  **アニメーション・リンクは不要**（静的ビジュアルのみ）
5.  **共通コンポーネントは配置を明示**

### ファイル構成

```
docs/design/screens/
├── _assets/           # 画像アセット
│   ├── logo.png
│   └── ...
├── _components/
│   ├── header/
│   │   ├── design.md
│   │   └── mockup.html
│   └── ...
├── home/
│   ├── design.md
│   └── mockup.html
└── ...
```

### 作成順序

1.  配色決定・必須アセット生成（AG: Nano Banana）
2.  共通コンポーネント作成（AG→CC）
3.  各画面作成（AG→CC）
4.  追加アセット生成（必要に応じて）

### オーバーレイ要素の管理

| 要素 | 管理場所 |
|------|---------|
| 共通（ヘッダー、ハンバーガーメニュー等） | `_components/` |
| 画面固有（確認ダイアログ等） | その画面のディレクトリ |

---

## 相互参照ルール

### ルール1: 命名の統一

| カテゴリ | 命名例 |
|---------|-------|
| DBテーブル | `recipes` |
| APIパス | `/api/recipes` |
| 画面ディレクトリ | `screens/recipe-list/` |
| ファイル名 | `recipes.md` |

### ルール2: 各ファイル冒頭に「関連」セクション

```markdown
## 関連
- API: [GET /api/recipes](../api/recipes.md)
- DB: [recipes](../db/recipes.md)
```

### ルール3: index.md に参照マップを記載

設計書ルート `docs/design/index.md` に全体の参照関係を記載

---

## CCレビュー（Claude Code による検証）

各設計書作成後、Claude Code でレビューを実施する。

### 実行コマンド

```bash
claude -p "以下のファイルをレビューしてください。
対象: docs/design/[対象ファイル]

レビュー観点:
1. 誤字脱字がないか
2. 曖昧な表現がないか（実装時に迷う記述）
3. 相互参照が正しく設定されているか
4. 必要な情報が漏れていないか

問題があれば具体的に指摘してください。"
```

---

## 完了条件

### 必須条件
- [ ] `docs/design/index.md` が作成されている
- [ ] `docs/design/basic-design.md` が作成されている
- [ ] 選定した全ての設計書が作成されている
- [ ] CCレビューが完了している
- [ ] ユーザーが全ての設計書を承認している

### 追加確認
- [ ] 各設計書に「関連」セクションがある
- [ ] 参照マップが作成されている
- [ ] ワイヤーフレームが対応デバイスごとに存在する

---

## 次フェーズへの移行

1. `PRODUCT.md` の `Current Phase` を「Phase 5: 環境構築」に更新
2. `Status & Decisions` に設計完了を記録
3. Phase 5 ワークフロー（`/phase-5-environment`）を開始

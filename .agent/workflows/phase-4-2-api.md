---
description: Phase 4.2 API設計 - RESTful/GraphQL等のエンドポイント設計
---

# Phase 4.2: API設計ワークフロー

## 目的・概要

このフェーズでは、基本設計書とデータ設計書に基づいて**API設計書**を作成する。

---

## 担当者

| 役割 | 担当 |
|------|------|
| 主導 | AG（建築家） |
| 承認 | User（施主） |

---

## 前提条件

- [ ] Phase 4.0 が完了していること
- [ ] `docs/design/basic-design.md` が存在すること
- [ ] データ設計が必要な場合、Phase 4.1 が完了していること
- [ ] 基本設計でAPI設計が「必要」と判定されていること
- [ ] `PRODUCT.md` の Current Phase が Phase 4.2 であること

> **スキップ条件**: 基本設計でAPI設計が「不要」と判定された場合、このフェーズをスキップして次へ進む。

---

## Step 1: 関連設計の確認

以下を確認し、API設計の要件を把握する：

1. `docs/design/basic-design.md` - システム概要、データフロー
2. `docs/design/db/index.md` - テーブル構造
3. `docs/requirements.md` - API関連の要件
4. `docs/tech-stack.md` - 採用するAPI技術（REST/GraphQL等）

---

## Step 2: ユーザーとの対話

以下の観点でユーザーと対話しながらAPI設計を固める：

### 2.1 APIスタイル

- RESTful / GraphQL / gRPC / その他
- バージョニング方針

### 2.2 認証・認可

- 認証方式（JWT、セッション、OAuth等）
- 認可（ロールベース、リソースベース等）

### 2.3 エンドポイント設計

- リソースの洗い出し
- 各リソースに対する操作（CRUD）
- URL設計

### 2.4 共通仕様

- レスポンス形式
- エラーハンドリング
- ページネーション
- レート制限

---

## Step 3: Implementation Plan 作成

設計内容が固まったら、実装計画を作成してユーザー承認を得る。

```markdown
# Ph4.2 API設計 - 実装計画

## 設計概要
（対話で固まった内容のサマリー）

## エンドポイント一覧

| メソッド | パス | 説明 |
|---------|------|------|
| GET | /api/users | ユーザー一覧 |
| POST | /api/users | ユーザー作成 |
| ... | ... | ... |

## 出力ファイル構成

- `docs/design/api/index.md` - エンドポイント一覧、共通仕様
- `docs/design/api/[path]/[action].[method].md` - 各API詳細
```

---

## Step 4: API設計書の作成

ユーザー承認後、以下のファイルを作成する：

### 4.1 ファイル構成

エンドポイントに合わせたディレクトリ構成：

```
docs/design/api/
├── index.md                      # エンドポイント一覧、共通仕様
├── users/
│   ├── list.get.md               # GET /api/users
│   ├── create.post.md            # POST /api/users
│   └── [id]/
│       ├── get.get.md            # GET /api/users/{id}
│       ├── update.put.md         # PUT /api/users/{id}
│       └── delete.delete.md      # DELETE /api/users/{id}
├── products/
│   ├── list.get.md
│   └── ...
└── ...
```

### 4.2 `docs/design/api/index.md`

```markdown
# API設計

## 概要

- ベースURL: `/api/v1`
- スタイル: RESTful
- 認証: Bearer Token

## エンドポイント一覧

| メソッド | パス | 説明 | 詳細 |
|---------|------|------|------|
| GET | /api/users | ユーザー一覧 | [users/list.get.md](./users/list.get.md) |
| POST | /api/users | ユーザー作成 | [users/create.post.md](./users/create.post.md) |
| ... | ... | ... | ... |

## 共通仕様

### 認証

Bearer Token を Authorization ヘッダーに設定

### レスポンス形式

成功時:
```json
{
  "data": { ... },
  "meta": { "timestamp": "..." }
}
```

エラー時:
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "エラーメッセージ"
  }
}
```

### ページネーション

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 100,
    "total_pages": 5
  }
}
```

### HTTPステータスコード

| コード | 説明 |
|-------|------|
| 200 | 成功 |
| 201 | 作成成功 |
| 400 | リクエストエラー |
| 401 | 認証エラー |
| 403 | 認可エラー |
| 404 | リソース不存在 |
| 500 | サーバーエラー |
```

### 4.3 `docs/design/api/[path]/[action].[method].md`

各APIエンドポイントの詳細：

```markdown
# GET /api/users

## 概要

ユーザー一覧を取得する

## 関連

- DB: [users テーブル](../../db/users.md)
- 画面: [ユーザー一覧画面](../../screens/user-list/)

## 認可

- 認証: 必須
- 権限: admin, manager

## リクエスト

### ヘッダー

| 名前 | 必須 | 説明 |
|------|------|------|
| Authorization | Yes | Bearer {token} |

### クエリパラメータ

| 名前 | 型 | 必須 | デフォルト | 説明 |
|------|-----|------|-----------|------|
| page | number | No | 1 | ページ番号 |
| per_page | number | No | 20 | 1ページあたりの件数 |
| q | string | No | - | 検索クエリ |

## レスポンス

### 200 OK

```json
{
  "data": [
    {
      "id": "uuid",
      "name": "田中太郎",
      "email": "tanaka@example.com",
      "created_at": "2025-01-01T00:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 100,
    "total_pages": 5
  }
}
```

### エラー

| ステータス | コード | 条件 |
|-----------|--------|------|
| 401 | UNAUTHORIZED | 認証トークンが無効 |
| 403 | FORBIDDEN | 権限不足 |
```

---

## Step 5: CCレビュー

API設計書完成後、Claude Code にレビューを依頼する。

```bash
claude -p "以下のファイルをレビューしてください。
対象: docs/design/api/

レビュー観点:
1. 誤字脱字がないか
2. エンドポイント設計に漏れがないか
3. リクエスト/レスポンス定義が実装可能か
4. データ設計書との整合性があるか
5. 基本設計書との乖離がないか
6. 要件定義書（docs/requirements.md）との乖離がないか

問題があれば具体的に指摘してください。"
```

---

## 完了条件

- [ ] `docs/design/api/index.md` が作成されている
- [ ] 全エンドポイントの詳細ファイルが作成されている
- [ ] 共通仕様（認証、エラー、ページネーション等）が記載されている
- [ ] CCレビューが完了している
- [ ] ユーザーがAPI設計書を承認している
- [ ] `docs/design/index.md` が更新されている

---

## 次フェーズへの移行

1. `PRODUCT.md` の `Current Phase` を次のフェーズに更新
   - 画面設計が必要 → 「Phase 4.3: 画面設計」
   - 画面設計不要 → 「Phase 5: 環境構築」
2. `Status & Decisions` にAPI設計完了を記録
3. 次のワークフローを開始

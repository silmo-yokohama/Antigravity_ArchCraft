# 🏛️ ArchCraft - AI協調開発フレームワーク

## 目的

ArchCraftは、**2つのAI（AG：建築家 / CC：職人）の役割分離と相互レビュー**によって、個人開発でも高品質なソフトウェアを構築するためのフレームワークである。

### 解決する課題

| 課題 | ArchCraftの解決策 |
|------|------------------|
| AIに曖昧な指示を出すと、意図しない実装になる | 詳細な設計書・実装計画を作成してから実装 |
| 要件の抜け漏れに後から気づく | 段階的な要件定義・設計フェーズで詳細化 |
| AIの出力を鵜呑みにしてしまう | 相互レビューで品質を担保 |
| プロジェクトの状態が分からなくなる | PRODUCT.md で唯一の真実（SSoT）を管理 |

### 基本原則

1. **役割分離の原則**: AGは設計に専念し、CCは実装に専念する
2. **相互レビューの原則**: 各AIは相手の成果物を検証する義務を持つ
3. **ユーザー主権の原則**: 最終決定権は常にユーザーにある
4. **設計優先の原則**: 設計書なしに実装を開始しない

---

## 全体フロー

```mermaid
flowchart TB
    subgraph Phase0["Phase 0: 初期化"]
        P0[プロジェクト情報確認<br/>Jira/Git設定<br/>README作成]
    end
    
    subgraph Phase1["Phase 1: 要件定義"]
        P1[ユーザー対話<br/>機能/非機能要件整理<br/>requirements.md作成]
    end
    
    subgraph Phase2["Phase 2: 技術選定"]
        P2[要件→技術マッピング<br/>選択肢比較<br/>tech-stack.md作成]
    end
    
    subgraph Phase3["Phase 3: ガイドライン"]
        P3[コーディング規約<br/>命名規則・Git運用<br/>guidelines/*.md作成]
    end
    
    subgraph Phase4["Phase 4: 設計"]
        P40[4.0 基本設計<br/>必要な設計書決定]
        P41[4.1 データ設計<br/>DB/キャッシュ]
        P42[4.2 API設計<br/>エンドポイント]
        P43[4.3 画面設計<br/>共通仕様策定]
        P44[4.4 モック生成<br/>ユーザー→CC]
        
        P40 --> P41
        P41 --> P42
        P42 --> P43
        P43 --> P44
    end
    
    subgraph Phase5["Phase 5: 環境構築"]
        P5[タスク分割→Jira<br/>setup.md作成<br/>CCが環境構築]
    end
    
    subgraph Phase68["Phase 6-8: 実装・検証"]
        P6[AGが実装計画作成<br/>CCがTDD実装<br/>AGがコードレビュー]
    end
    
    Phase0 --> Phase1
    Phase1 --> Phase2
    Phase2 --> Phase3
    Phase3 --> Phase4
    Phase4 --> Phase5
    Phase5 --> Phase68
```

---

## フェーズ一覧

### Phase 0-3: 準備フェーズ（AG主導）

| Phase | 名称 | 成果物 | ワークフロー |
|-------|------|--------|-------------|
| 0 | 初期化 | PRODUCT.md, README.md | [phase-0-init.md](.agent/workflows/phase-0-init.md) |
| 1 | 要件定義 | docs/requirements.md | [phase-1-requirements.md](.agent/workflows/phase-1-requirements.md) |
| 2 | 技術選定 | docs/tech-stack.md | [phase-2-tech-stack.md](.agent/workflows/phase-2-tech-stack.md) |
| 3 | ガイドライン | docs/guidelines/* | [phase-3-guidelines.md](.agent/workflows/phase-3-guidelines.md) |

### Phase 4: 設計フェーズ（AG主導、4.4のみユーザー主導）

| サブフェーズ | 名称 | 成果物 | ワークフロー |
|-------------|------|--------|-------------|
| 4.0 | 基本設計 | docs/design/basic-design.md | [phase-4-0-basic.md](.agent/workflows/phase-4-0-basic.md) |
| 4.1 | データ設計 | docs/design/db/* | [phase-4-1-data.md](.agent/workflows/phase-4-1-data.md) |
| 4.2 | API設計 | docs/design/api/* | [phase-4-2-api.md](.agent/workflows/phase-4-2-api.md) |
| 4.3 | 画面設計プランニング | docs/design/screens/index.md | [phase-4-3-screens-planning.md](.agent/workflows/phase-4-3-screens-planning.md) |
| 4.4 | 画面モック生成 | docs/design/screens/html/* | [phase-4-4-screens-mockup.md](.agent/workflows/phase-4-4-screens-mockup.md) |

> **Note**: Phase 4.4 は**ユーザー主導**でCCに依頼する特殊なフェーズ

### Phase 5-8: 実装フェーズ（AG+CC協調）

| Phase | 名称 | 成果物 | ワークフロー |
|-------|------|--------|-------------|
| 5 | 環境構築 | docs/setup.md, Jiraタスク | [phase-5-environment.md](.agent/workflows/phase-5-environment.md) |
| 6-8 | 実装・検証 | src/*, tests/* | [phase-6-implementation.md](.agent/workflows/phase-6-implementation.md) |
| - | 完了・維持 | CI/CD, ドキュメント | [completion.md](.agent/workflows/completion.md) |

---

## 各フェーズの共通フロー

```
1. 前提条件チェック
2. ユーザーと対話しながら内容を固める
3. Implementation Plan 作成 → ユーザー承認
4. 成果物作成
5. CCレビュー
6. ユーザー最終承認
7. PRODUCT.md 更新 → 次フェーズへ
```

---

## ディレクトリ構成

```
project/
├── PRODUCT.md           # プロジェクトの唯一の真実（SSoT）
├── ArchCraft.md         # このファイル（フレームワーク説明）
├── CLAUDE.md            # CC（職人）向け指示書
├── GEMINI.md            # AG（建築家）向け指示書
├── README.md            # プロジェクト説明
├── .agent/
│   └── workflows/       # フェーズ別ワークフロー
│       ├── phase-0-init.md
│       ├── phase-1-requirements.md
│       ├── phase-2-tech-stack.md
│       ├── phase-3-guidelines.md
│       ├── phase-4-0-basic.md
│       ├── phase-4-1-data.md
│       ├── phase-4-2-api.md
│       ├── phase-4-3-screens-planning.md
│       ├── phase-4-4-screens-mockup.md
│       ├── phase-5-environment.md
│       ├── phase-6-implementation.md
│       └── completion.md
├── docs/
│   ├── requirements.md      # 要件定義書（Phase 1）
│   ├── tech-stack.md        # 技術選定書（Phase 2）
│   ├── setup.md             # 環境構築手順（Phase 5）
│   ├── guidelines/          # ガイドライン（Phase 3）
│   │   ├── coding.md
│   │   └── ...
│   ├── design/              # 設計書（Phase 4）
│   │   ├── index.md         # 設計書インデックス・参照マップ
│   │   ├── basic-design.md
│   │   ├── db/
│   │   ├── api/
│   │   └── screens/
│   │       ├── index.md     # 画面共通設計
│   │       ├── _assets/     # 画像アセット
│   │       └── html/        # HTMLモック
│   ├── plans/               # 実装計画（Phase 6-8）
│   │   └── [タスクID]-plan.md
│   └── opinions/            # 意見書（CC→ユーザー）
├── src/                     # ソースコード
└── tests/                   # テストコード
```

---

## How To: ArchCraftの使い方

### 1. プロジェクト開始

```
ユーザー: /phase-0-init
AG: プロジェクトを初期化します...
```

AGに `/phase-0-init` コマンドを実行させると、ワークフローに沿って初期化が行われる。

### 2. 各フェーズの進め方

```
ユーザー: /phase-1-requirements
AG: 要件定義を開始します。いくつか質問させてください...
```

- 各フェーズはワークフロー（`.agent/workflows/`）に沿って進行
- AGがユーザーに質問しながら詳細化
- 成果物作成後、CCにレビューを依頼
- ユーザー承認後、次フェーズへ

### 3. 設計フェーズ（Phase 4）

Phase 4 は5つのサブフェーズに分かれている：

```
/phase-4-0-basic    → 基本設計・必要な設計書決定
/phase-4-1-data     → データ設計（必要時）
/phase-4-2-api      → API設計（必要時）
/phase-4-3-screens-planning → 画面設計プランニング（必要時）
/phase-4-4-screens-mockup   → ユーザーがCCにモック生成依頼
```

> **重要**: Phase 4.4 はユーザー主導。AGは関与せず、ユーザーがCCに直接依頼する。

### 4. 実装フェーズ（Phase 6-8）

```
ユーザー: /phase-6-implementation
AG: Jiraから次のタスクを確認します...
    [タスク名]の実装計画を作成しました。CCに実装を依頼してよいですか？
ユーザー: OK
AG: （CCに実装計画を渡す）
CC: TDD形式で実装します...
AG: コードレビューします...
```

- AGが実装計画を作成
- CCがTDD形式で実装
- AGがコードレビュー
- 承認後コミット

### 5. 進捗確認

`PRODUCT.md` を確認すれば、現在のフェーズ・進捗・決定事項が分かる。

---

## 対立解消プロトコル

CCが設計に異論を唱える場合:

1. `docs/opinions/opinion_from_claude.md` を作成
2. 作業を一時停止（Blocked）
3. ユーザーの仲裁を待つ
4. 決定事項を `PRODUCT.md` に記録

### AI間通信プロトコル（間接議論フロー）

AI同士の直接の競合やコンテキストの混乱を防ぐため、議論は**間接方式**で行う。

```mermaid
sequenceDiagram
    participant CC as Claude Code（職人）
    participant User as ユーザー（施主）
    participant AG as Antigravity（建築家）

    CC->>CC: 設計上の懸念を検出
    CC->>User: docs/opinions/opinion_from_claude.md を生成し作業停止
    User->>AG: 「職人からこのような指摘があるが、設計を修正すべきか？」
    AG->>User: 回答を作成
    User->>User: 最終決定を下す
    User->>CC: 決定事項を伝達
    Note over User: PRODUCT.md に背景を記録
```

---

## 開発・安全基準

| 項目 | 基準 |
|------|------|
| **開発プロセス** | ウォーターフォール形式による段階的承認（Phase 0〜8） |
| **実装パターン** | テスト駆動開発（TDD）の徹底 |
| **出力言語** | 全てのドキュメント、タスク出力は**日本語**とする |
| **設計優先** | 設計書なしに実装を開始しない |
| **安全措置** | 破壊的コマンドやファイル削除には必ずユーザーの承認を求める |

---

## フェーズ判定ロジック

各フェーズの完了は、成果物の生成によって判定される。

| Phase | 名称 | 完了条件（成果物） |
|-------|------|------------------|
| 0 | 初期化 | `PRODUCT.md`, `README.md` 生成完了 |
| 1 | 要件定義 | `docs/requirements.md` 生成完了 |
| 2 | 技術選定 | `docs/tech-stack.md` 生成完了 |
| 3 | ガイドライン | `docs/guidelines/coding.md` 生成完了 |
| 4.0 | 基本設計 | `docs/design/basic-design.md` 生成完了 |
| 4.1 | データ設計 | `docs/design/db/index.md` 生成完了 |
| 4.2 | API設計 | `docs/design/api/index.md` 生成完了 |
| 4.3 | 画面設計 | `docs/design/screens/index.md` 生成完了 |
| 4.4 | モック生成 | `docs/design/screens/html/` 生成完了 |
| 5 | 環境構築 | `docs/setup.md` 生成完了、Jiraタスク登録完了 |
| 6-8 | 実装・検証 | `PRODUCT.md` の phase 値で管理 |

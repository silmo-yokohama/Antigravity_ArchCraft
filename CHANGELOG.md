# Changelog

All notable changes to this project will be documented in this file.

## [v0.0.1] - 2026-01-15

### 🎉 Initial Release

ArchCraft AI駆動開発フレームワークの初期リリース。

### ✨ Features

- **フレームワーク基盤**
  - `ArchCraft.md`: 運用ルール（フレームワーク憲法）
  - `PRODUCT.md`: プロジェクトの唯一の真実（SSoT）テンプレート
  - `GEMINI.md`: 建築家（AG）への指示書
  - `CLAUDE.md`: 職人（CC）への指示書

- **ワークフロー（8フェーズ）**
  - Phase 0: プロジェクト初期化
  - Phase 1: 要件定義（ユーザー対話形式）
  - Phase 2: 技術選定
  - Phase 3: ガイドライン策定
  - Phase 4: 設計（DB/API/画面/デザインモック）
  - Phase 5: 環境構築・タスク分割
  - Phase 6-8: 実装・検証・デプロイ

- **設計ドキュメント構成**
  - 分割ファイル構成（db/, api/, screens/）
  - 相互参照ルール
  - HTMLベースのデザインモック（CC生成）
  - Nano Bananaによる画像アセット生成

- **品質管理**
  - AI間相互レビューシステム
  - 対立解消プロトコル
  - TDD徹底

### 📝 Templates

- `docs/_templates/adr.md`: アーキテクチャ決定記録
- `docs/_templates/implementation-plan.md`: 実装計画
- `docs/_templates/opinion.md`: 意見書
- `docs/_templates/parent-task-template.md`: 親タスク
- `docs/_templates/subtask-template.md`: サブタスク

# 🏛️ ArchCraft - AI駆動開発フレームワーク

**ArchCraft**は、AIエディタ「Antigravity（AG）」を**建築家**、AIエージェント「Claude Code（CC）」を**職人**として定義し、人間が**施主**としてプロジェクトを統治する次世代の開発フレームワークです。

## 🎯 特徴

- **役割分離**: 設計（AG）と実装（CC）を明確に分離
- **相互レビュー**: AI同士がチェックし合い品質を担保
- **段階的承認**: ウォーターフォール形式で各フェーズを確実に完了
- **TDD**: テスト駆動開発の徹底

## 📁 ファイル構成

| ファイル | 説明 |
|---------|------|
| `ArchCraft.md` | フレームワーク運用ルール（憲法） |
| `PRODUCT.md` | プロジェクトの唯一の真実（SSoT） |
| `GEMINI.md` | 建築家（AG）への指示書 |
| `CLAUDE.md` | 職人（CC）への指示書 |
| `.agent/workflows/` | 各フェーズのワークフロー |

## 🚀 使い方

### 1. このリポジトリをクローン

```bash
git clone https://github.com/YOUR_USERNAME/Antigravity_ArchCraft.git my-project
cd my-project
```

### 2. プロジェクトを初期化

1. AGセッションを開始
2. `PRODUCT.md` のプロジェクト名・概要を記入
3. `/phase-0-init` を実行

### 3. 各フェーズを順番に進める

| Phase | ワークフロー | 成果物 |
|-------|-------------|--------|
| 0 | `/phase-0-init` | PRODUCT.md, README.md |
| 1 | `/phase-1-requirements` | docs/requirements.md |
| 2 | `/phase-2-tech-stack` | docs/tech-stack.md |
| 3 | `/phase-3-guidelines` | docs/guidelines/*.md |
| 4 | `/phase-4-design` | docs/design/*.md, mockup.html |
| 5 | `/phase-5-environment` | docs/setup.md, Jiraタスク |
| 6-8 | `/phase-6-implementation` | src/*, tests/* |

## 📋 前提条件

- **Antigravity（AG）**: Gemini 2.5 Pro搭載のAIエディタ
- **Claude Code（CC）**: ターミナルで動作するAIエージェント
- **Jira**: タスク管理（オプション）

## 📄 ライセンス

MIT License

---

Made with 🏛️ ArchCraft

# Implementation Tasks: {{FEATURE_NAME}}

## Overview
{{OVERVIEW_DESCRIPTION}}

この実装は段階的なアプローチを採用し、基盤構築から機能実装、テストまでを論理的な順序で進める。

## Task List

### 1. プロジェクト基盤構築
プロジェクトの初期設定と基本インフラストラクチャを構築する。

対応要件: (該当する要件番号をカンマ区切りで記載)

- [ ] **1.1 サブタスクタイトル**
  - 実装する内容の説明（何を作るか、なぜ必要か）
  - 主要な実装ポイント（高レベルのみ）
  - 他コンポーネントとの統合ポイント

- [ ] **1.2 サブタスクタイトル (P)**
  - 並行開発可能なタスクには (P) マーカーを付与
  - 独立して実装・テスト可能な内容

### 2. 主要機能の実装
{{MAJOR_TASK_DESCRIPTION}}

対応要件: (該当する要件番号)

- [ ] **2.1 サブタスクタイトル**
  - 機能の実装内容
  - 統合と検証のポイント

- [ ] **2.2 サブタスクタイトル**
  - 前のタスクに依存する場合は (P) マーカーなし
  - 実装の詳細と期待される動作

### 3. テストと検証
全ての要件が満たされていることを検証する。

対応要件: All

- [ ] **3.1 ユニットテスト**
  - 個別コンポーネントのテスト実装
  - カバレッジ目標と重要なテストケース

- [ ] **3.2 統合テスト**
  - コンポーネント間の連携テスト
  - エンドツーエンドシナリオの検証

- [ ]* **3.3 オプショナルなテストカバレッジ拡張**
  - MVP後に追加可能なテストケース
  - パフォーマンステストやエッジケース
  - `- [ ]*` マーカーはMVP後に延期可能なタスクを示す

---

## Notes

- **(P) マーカー**: 並行開発可能なタスク（依存関係がない）
- **推定時間**: 各サブタスクは1-3時間を想定
- **統合**: 全てのタスクは既存システムと統合され、孤立した実装を避ける
- **要件カバレッジ**: 全要件が少なくとも1つのタスクにマップされている

---

*生成日時: {{TIMESTAMP}}*

## Template Usage Instructions (Remove before final output)

### Placeholder Replacement
- `{{FEATURE_NAME}}`: Feature name from spec.json
- `{{OVERVIEW_DESCRIPTION}}`: Brief summary of implementation scope
- `{{MAJOR_TASK_DESCRIPTION}}`: Description of major task group
- `{{TIMESTAMP}}`: Current ISO 8601 timestamp

### Task Structure Guidelines

1. **Major Tasks**: High-level milestones (e.g., "1. プロジェクト基盤構築")
   - Include brief description of the group's goal
   - List covered requirements: `対応要件: 1, 2, 3`

2. **Sub-tasks**: Specific implementation work (e.g., "1.1 プロジェクト初期化")
   - Describe what to build and why
   - Include integration points
   - Add `(P)` marker if task can be developed in parallel

3. **Parallel Markers `(P)`**:
   - Mark tasks with no dependencies on other pending tasks
   - Only use when task can truly be developed independently
   - Omit in sequential mode (`--sequential` flag)

4. **Optional Test Tasks `- [ ]*`**:
   - Mark test coverage tasks that can be deferred post-MVP
   - Only use for tests that strictly cover acceptance criteria already satisfied by implementation
   - Core implementation tasks should NEVER use `- [ ]*`

5. **Requirements Coverage**:
   - Use format: `対応要件: 1, 2, 3` (numeric IDs only, comma-separated)
   - Do NOT use: "対応要件: 1 (ユーザー認証), 2 (メッセージ送信)" ❌
   - Do NOT use: "対応要件: Requirement 1, Requirement 2" ❌

6. **Collapsing Single Subtasks**:
   - If a major task has only one sub-task, promote it to major task level
   - Avoid unnecessary nesting for clarity

### Generation Checklist

Before finalizing tasks.md, verify:

- [ ] All requirements from requirements.md are covered
- [ ] Each sub-task is sized 1-3 hours
- [ ] Task descriptions use natural language (what/why, not how)
- [ ] Maximum 2-level hierarchy (no 1.1.1)
- [ ] Sequential numbering with no gaps
- [ ] Logical progression (foundation → features → testing)
- [ ] Integration points are explicit
- [ ] Parallel tasks marked with `(P)` (unless sequential mode)
- [ ] Optional test tasks marked with `- [ ]*` (only when appropriate)
- [ ] Requirements format: `対応要件: 1, 2, 3` (no descriptions)

**Remove this section before final output.**

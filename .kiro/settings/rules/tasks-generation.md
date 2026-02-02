# Task Generation Rules

## Core Principles

### 1. Natural Language Descriptions
- Describe **what to build and its purpose**, not **how to build it**
- Focus on capabilities and outcomes, not implementation details
- Use clear, action-oriented language
- Avoid code structure references unless essential for clarity

**Good Example**:
```markdown
### 1.2 ユーザー認証機能の実装
ユーザー名のバリデーション、重複チェック、ログイン/ログアウト処理を実装し、認証状態をSocket.IO接続と関連付ける。
```

**Bad Example**:
```markdown
### 1.2 AuthHandler クラスの実装
AuthHandler クラスを作成し、handleLogin、handleLogout、handleDisconnect メソッドを実装する。
```

### 2. Complete Requirements Coverage
- **Every requirement** from requirements.md MUST map to at least one task
- Include requirement IDs in task descriptions: "対応要件: 1, 2, 3"
- When documenting requirement coverage, list numeric requirement IDs only (comma-separated) without descriptive suffixes, parentheses, translations, or free-form labels
- Verify all non-functional requirements are addressed

### 3. Task Sizing
- Each sub-task should take **1-3 hours** for an experienced developer
- If a task is larger, break it into smaller sub-tasks
- If a task is smaller, combine with related work
- Major tasks can be larger (sum of sub-tasks)

### 4. Maximum 2-Level Hierarchy
- **Major Tasks**: High-level milestones (e.g., "1. プロジェクト基盤構築")
- **Sub-tasks**: Specific implementation work (e.g., "1.1 プロジェクト初期化")
- **No deeper nesting**: Avoid 1.1.1, 1.1.2, etc.

### 5. Sequential Numbering
- Major tasks: 1, 2, 3, 4...
- Sub-tasks: 1.1, 1.2, 1.3... (under major task 1)
- **Never repeat numbers**: Each task ID must be unique
- **Never skip numbers**: Increment sequentially

### 6. Task Progression and Dependencies
- Order tasks logically: foundation → features → integration → testing
- Earlier tasks should enable later tasks
- Document critical dependencies explicitly
- Group related work into cohesive major tasks

### 7. Integration Focus
- Every task must integrate with the existing system
- Avoid "orphaned" work that doesn't connect
- Test integration points explicitly
- Ensure end-to-end functionality

### 8. Testing Strategy
- Unit tests for individual components
- Integration tests for component interactions
- E2E tests for user scenarios
- Testing tasks can be separate or embedded in implementation tasks

### 9. Parallel vs Sequential Tasks
- Mark tasks that can be developed independently with `(P)` in the task title
- Only mark tasks that truly have no dependencies on other tasks
- Use parallel markers to enable concurrent development
- When `--sequential` mode is used, omit all `(P)` markers

### 10. Collapse Single-Subtask Structures
- If a major task contains only one sub-task, promote the sub-task to a major task
- Avoid unnecessary hierarchy for single items
- Keep task structure flat and scannable
- Exception: If logical grouping requires container (rare cases)

## Task Template Structure

```markdown
# Implementation Tasks: {feature-name}

## Overview
Brief summary of the implementation scope and approach.

## Task List

### 1. Major Task Title
Brief description of the major task's goal and scope.

対応要件: 1, 2, 3

- [ ] **1.1 Sub-task Title**
  - Description of what to build and why
  - Key implementation points (high-level only)
  - Integration points with other components

- [ ] **1.2 Sub-task Title**
  - Description and purpose
  - Integration considerations

### 2. Major Task Title (P)
This major task can be developed in parallel with task 1.

対応要件: 4, 5

- [ ] **2.1 Sub-task Title (P)**
  - Independent work that doesn't block or depend on other tasks

- [ ] **2.2 Sub-task Title**
  - This sub-task depends on 2.1 completing first

### 3. Testing and Validation
Testing tasks to verify all requirements are met.

対応要件: All

- [ ] **3.1 Unit Tests**
  - Test individual components

- [ ] **3.2 Integration Tests**
  - Test component interactions

- [ ] **3.3 E2E Tests**
  - Test user scenarios end-to-end
```

## Requirements Coverage Documentation

When documenting which requirements a task addresses:

**Correct Format**:
```markdown
対応要件: 1, 2, 3
```

**Incorrect Formats** (DO NOT USE):
```markdown
対応要件: 1 (ユーザー認証), 2 (メッセージ送信)  // ❌ No descriptions
対応要件: Requirement 1, Requirement 2  // ❌ No "Requirement" prefix
対応要件: 要件1, 要件2  // ❌ No Japanese prefixes
対応要件: 1, 2, and 3  // ❌ Use commas only, no "and"
```

Always use: `対応要件: 1, 2, 3` (numeric IDs only, comma-separated)

## Common Anti-Patterns to Avoid

### ❌ Anti-Pattern 1: Code-Centric Descriptions
```markdown
- [ ] **1.1 MessageStore クラスの作成**
  - `addMessage()` メソッドを実装
  - `getMessages()` メソッドを実装
  - `MAX_MESSAGES` 定数を定義
```

### ✅ Correct Approach: Capability-Centric
```markdown
- [ ] **1.1 メッセージストレージの実装**
  - メッセージの追加と取得機能を実装し、最新100件のみを保持する仕組みを構築する
```

---

### ❌ Anti-Pattern 2: Tasks Too Large
```markdown
- [ ] **1.1 チャット機能全体の実装**
  - ユーザー認証、メッセージ送信、受信、ユーザー一覧を全て実装
```

### ✅ Correct Approach: Properly Sized
```markdown
- [ ] **1.1 ユーザー認証機能の実装** (1-2 hours)
- [ ] **1.2 メッセージ送信機能の実装** (2-3 hours)
- [ ] **1.3 メッセージ受信と表示機能の実装** (2-3 hours)
- [ ] **1.4 ユーザー一覧機能の実装** (1-2 hours)
```

---

### ❌ Anti-Pattern 3: Missing Integration
```markdown
- [ ] **1.1 React コンポーネントの作成**
  - LoginPage を作成
  - ChatPage を作成
  - (統合方法が不明)
```

### ✅ Correct Approach: Integration Included
```markdown
- [ ] **1.1 ログインページの実装**
  - ユーザー名入力フォームを作成し、Socket.IO 接続と認証処理を統合する
```

---

### ❌ Anti-Pattern 4: Orphaned Tasks
```markdown
- [ ] **1.1 ユーティリティ関数の作成**
  - 日付フォーマット関数を作成
  - (どこで使うか不明)
```

### ✅ Correct Approach: Purpose-Driven
```markdown
- [ ] **1.1 メッセージ表示機能の実装**
  - メッセージ一覧を時系列順に表示し、送信時刻を人間が読める形式でフォーマットする
```

---

### ❌ Anti-Pattern 5: Incomplete Requirements Coverage
```markdown
## Task List
(要件 1, 2, 3 のみカバー、要件 4, 5 が欠落)
```

### ✅ Correct Approach: Full Coverage
```markdown
## Task List
(全要件 1-5 および非機能要件をカバー)
```

## Task Generation Checklist

Before finalizing tasks, verify:

- [ ] All requirements from requirements.md are covered
- [ ] Each sub-task is 1-3 hours in size
- [ ] Task descriptions use natural language (capabilities, not code)
- [ ] Maximum 2-level hierarchy (major tasks + sub-tasks)
- [ ] Sequential numbering with no gaps or repeats
- [ ] Logical task progression (foundation → features → testing)
- [ ] Integration points are explicit
- [ ] Testing strategy is included
- [ ] Parallel tasks are marked with `(P)` (unless sequential mode)
- [ ] No single-subtask major tasks (collapsed to flat structure)
- [ ] Requirement IDs use format: "対応要件: 1, 2, 3" (numeric only, no descriptions)

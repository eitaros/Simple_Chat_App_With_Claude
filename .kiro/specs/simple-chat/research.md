# Research & Discovery Log: simple-chat

## Summary
シンプルなチャットアプリケーションの技術設計のための包括的な調査を実施。リアルタイム通信技術、モダンなフロントエンド/バックエンドスタック、セキュリティベストプラクティスを中心に評価。新規機能としてフル調査プロセスを適用。

## Research Topics

### Topic 1: リアルタイム通信アーキテクチャ
**Question**: チャットアプリケーションに最適なリアルタイム通信パターンとプロトコルは何か？

**Sources**:
- [How To Design A Real Time Chat Application](https://javatechonline.com/how-to-design-a-real-time-chat-application/)
- [WebSocket architecture best practices](https://ably.com/topic/websocket-architecture-best-practices)
- [Building Real-Time AI Chat: Infrastructure for WebSockets](https://render.com/articles/real-time-ai-chat-websockets-infrastructure)

**Findings**:
- WebSocketは双方向通信の基盤として最適（100ms以内のメッセージ配信が可能）
- Serverfulアーキテクチャが推奨される（永続的な接続の維持が必要）
- Pub/Subパターンによる水平スケーリングが有効
- フォールバックメカニズム（HTTP Long Polling、SSE）の実装が重要
- 接続管理が最も重要な要素（アクティブ接続の追跡、メッセージルーティング、クリーンアップ）

**Implications for Design**:
- Client-Server + Pub/Subパターンをベースアーキテクチャとして採用
- WebSocketを主要通信プロトコルとして選定
- 接続状態管理とクリーンアップロジックを明示的に設計

---

### Topic 2: Socket.IO vs ネイティブWebSocket
**Question**: Socket.IOとネイティブWebSocketのどちらを選択すべきか？

**Sources**:
- [Socket.IO Documentation v4](https://socket.io/docs/v4/)
- [WebSocket vs Socket.IO: Performance & Use Case Guide](https://ably.com/topic/socketio-vs-websocket)
- [Socket.IO vs WebSocket: Comprehensive Comparison](https://www.videosdk.live/developer-hub/websocket/socketio-vs-websocket)

**Findings**:
- **ネイティブWebSocket**: 低レイテンシ、低オーバーヘッド、パフォーマンス重視のアプリに最適
- **Socket.IO**: 自動再接続、フォールバック、ブロードキャスト、Room機能などの高レベル機能を提供
- Socket.IOのバンドルサイズ: 10.4 KB (minified + gzipped)
- Socket.IOの主要機能:
  - 自動再接続（exponential back-off付き）
  - Acknowledgements（タイムアウト付き応答確認）
  - Broadcasting（全体またはRoom単位）
  - パケットバッファリング（切断時の自動キューイング）
  - Namespaces（ロジック分離）
  - HTTP Long-Pollingフォールバック
- 最近の開発者の見解: "ws = raw speed, socket.io = resilience"

**Implications for Design**:
- シンプルなチャットアプリには**Socket.IO**を採用
- 理由: 自動再接続、ブロードキャスト、Room機能が要件（ユーザー一覧更新、メッセージ配信、自動再接続）と完全一致
- パフォーマンス要件（1秒以内配信、10同時接続）は十分満たせる

---

### Topic 3: フロントエンドスタック選定
**Question**: 2026年のモダンなReactベースのチャットアプリに最適な状態管理とライブラリは？

**Sources**:
- [React, Next.js, Node.js: Full-Stack Innovations in 2026](https://kanhasoft.com/blog/react-next-js-node-js-the-full%E2%80%91stack-innovations-changing-software-in-2026/)
- [State Management in React (2026): Best Practices](https://www.c-sharpcorner.com/article/state-management-in-react-2026-best-practices-tools-real-world-patterns/)
- [The Best State Management Strategies in React 19](https://medium.com/@roman_j/the-best-state-management-strategies-in-react-19-bb51f64775c6)

**Findings**:
- React 18/19 + TypeScriptがスタンダード
- 状態管理のベストプラクティス（2026）:
  - ローカル状態: `useState` / `useReducer`
  - 環境レベル状態（theme、auth、locale）: Context API
  - 軽量な共有状態: Zustand（30%+ YoY成長、40%+のプロジェクトで採用）
  - サーバーデータ: React Query/TanStack Query（80%のサーバーデータ管理をカバー）
- Contextは低頻度更新データに限定すべき（高頻度更新は全Consumerが再レンダリングされる問題）
- React 18の並行モード、useTransitionでパフォーマンス向上
- Zustandは選択的購読により40-70%の再レンダリング削減

**Implications for Design**:
- フロントエンド: React 18+ + TypeScript
- 状態管理:
  - メッセージ、ユーザー一覧: ローカル状態（`useState`）
  - 現在のユーザー情報: Context API
  - 複雑な状態ロジックが必要な場合: `useReducer` or Zustand（必要に応じて）
- Socket.IOクライアント統合: React hooksでラップ

---

### Topic 4: セキュリティ - XSS対策
**Question**: Node.js/ExpressでXSS攻撃を防ぐベストプラクティスは？

**Sources**:
- [OWASP Nodejs Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)
- [Security Best Practices for Express](https://expressjs.com/en/advanced/best-practice-security.html)
- [express-xss-sanitizer](https://www.npmjs.com/package/express-xss-sanitizer)

**Findings**:
- 全てのユーザー入力を敵対的とみなす原則
- 入力サニタイゼーションライブラリ:
  - `express-xss-sanitizer`: Express 4.x/5.x middleware（req.body, req.query, req.headers, req.paramsをサニタイズ）
  - `validator.js`: 入力バリデーション
  - `express-validator`: 詳細なバリデーションポリシー
  - `xss`: XSS専用パッケージ
- セキュリティヘッダー: `helmet`パッケージでCSP実装
- ベストプラクティス:
  - 事前定義スキーマに対してユーザー入力を検証
  - ホワイトリスト/ブラックリストによるフィルタリング
  - 危険な文字を安全な等価物に変換
  - 出力時にエンコード

**Implications for Design**:
- バックエンド: `express-validator`でメッセージとユーザー名を検証
- `helmet`ミドルウェアでセキュリティヘッダー設定
- メッセージ入力: 最大長制限、HTMLタグのエスケープ
- フロントエンド: DOMPurifyまたはReactの自動エスケープに依存

## Technology Decisions

### Decision 1: リアルタイム通信ライブラリ
**Options Considered**:
1. ネイティブWebSocket - Pros: 低オーバーヘッド、最高パフォーマンス / Cons: 手動実装（再接続、フォールバック、ブロードキャスト）
2. Socket.IO - Pros: 自動再接続、Room、Broadcasting、フォールバック機能 / Cons: 若干のオーバーヘッド（10.4 KB）

**Selected**: Socket.IO v4

**Rationale**:
- 要件（自動再接続、リアルタイムユーザー一覧、全ユーザーへのブロードキャスト）がSocket.IOの機能セットと完全一致
- 開発速度と信頼性を優先（battle-tested）
- パフォーマンス要件（1秒以内、10同時接続）は十分満たせる
- ネイティブWebSocketの手動実装は不要な複雑性を導入

**Verification**:
- Version: Socket.IO v4.x (latest stable)
- Known issues: なし（安定版）
- Migration path: N/A（新規プロジェクト）

---

### Decision 2: フロントエンドフレームワークと状態管理
**Options Considered**:
1. React + TypeScript + useState - Pros: シンプル、標準的 / Cons: 複雑な状態管理には限界
2. React + TypeScript + Zustand - Pros: 軽量、選択的購読 / Cons: 追加ライブラリ
3. React + TypeScript + Redux Toolkit - Pros: エンタープライズ向け / Cons: 過剰（シンプルなチャットには）

**Selected**: React 18+ + TypeScript + useState + Context API

**Rationale**:
- シンプルなチャットアプリには`useState`で十分
- 現在のユーザー情報のみContext APIで管理
- Zustandは将来的な拡張時に追加可能
- TypeScript強制で型安全性確保

---

### Decision 3: バックエンドフレームワーク
**Options Considered**:
1. Express.js - Pros: 成熟、エコシステム豊富、シンプル / Cons: ボイラープレート
2. Fastify - Pros: 高速、モダン / Cons: Socket.IO統合が若干複雑

**Selected**: Express.js + TypeScript

**Rationale**:
- Socket.IOとの統合が標準的でドキュメント豊富
- セキュリティミドルウェア（helmet、express-validator）が成熟
- チームの習熟度が高い（想定）
- パフォーマンス要件には十分

## Architecture Patterns Evaluated

### Pattern 1: Client-Server + Event-Driven (Pub/Sub)
**Description**: クライアントとサーバー間でWebSocket接続を確立し、イベント駆動でメッセージをブロードキャスト
**Pros**: リアルタイム性、シンプルな実装、Socket.IOのネイティブサポート
**Cons**: 単一サーバーの場合スケーリング限界あり（水平スケーリングにはRedis必要）
**Fit Assessment**: 要件（10同時接続）に対して完全に適合、将来的な拡張も可能

### Pattern 2: Microservices + Message Queue
**Description**: 認証、メッセージング、ユーザー管理を別サービスに分離
**Pros**: 高スケーラビリティ、独立デプロイ
**Cons**: 過剰な複雑性、運用コスト高
**Fit Assessment**: シンプルなチャットアプリには過剰

### Pattern 3: Serverless + WebSocket Gateway
**Description**: API Gateway + Lambda + DynamoDB Streams
**Pros**: 自動スケーリング、従量課金
**Cons**: WebSocketの永続接続に不適（タイムアウト、状態管理の複雑性）
**Fit Assessment**: リアルタイムチャットには不適

**Selected Pattern**: Client-Server + Event-Driven (Pub/Sub)

**Rationale**:
- Socket.IOのRoom機能とBroadcastingが要件と完全一致
- シンプルで保守しやすい
- 将来的にRedis追加で水平スケーリング可能
- 10同時接続の要件には単一サーバーで十分

## External Dependencies

### Dependency 1: Socket.IO
**Purpose**: リアルタイム双方向通信
**Version**: v4.x (latest stable, ~4.7.x)
**Documentation**: https://socket.io/docs/v4/
**Key Constraints**:
- クライアント/サーバー間のバージョン互換性必要
- バンドルサイズ 10.4 KB
- Engine.IOプロトコル依存

**Integration Points**:
- サーバー: Express.jsサーバーに統合（`http.Server`ラップ）
- クライアント: React hooksでラップして状態管理

---

### Dependency 2: Express.js
**Purpose**: HTTPサーバーとRESTエンドポイント（静的ファイル配信）
**Version**: v5.x or v4.x (latest stable)
**Key Constraints**:
- Socket.IOは`http.Server`インスタンスが必要（Express appをラップ）

---

### Dependency 3: express-validator
**Purpose**: 入力バリデーションとサニタイゼーション
**Version**: Latest stable (~7.x)
**Documentation**: https://express-validator.github.io/docs/
**Key Constraints**:
- ユーザー名: 1-20文字、英数字のみ
- メッセージ: 1-500文字、HTMLタグ除去

---

### Dependency 4: helmet
**Purpose**: セキュリティヘッダー設定（CSP、XSS対策）
**Version**: Latest stable (~7.x)
**Documentation**: https://helmetjs.github.io/

## Risks Identified

### Risk 1: 同時接続数スケーリング
**Description**: 単一サーバーでの同時接続数上限（10接続要件は満たすが、将来的な拡張）
**Likelihood**: 低（初期要件は10接続のみ）
**Impact**: 中（将来的なユーザー増加時）
**Mitigation Strategy**:
- 初期はシングルサーバーで実装
- 将来的にRedis Adapterを追加して水平スケーリング対応
- 接続数モニタリングを実装
**Source**: Architecture pattern research

---

### Risk 2: メッセージ履歴の永続化欠如
**Description**: メモリ上の100件制限により、サーバー再起動時に履歴が失われる
**Likelihood**: 高（サーバー再起動時）
**Impact**: 低（制約事項として明記済み）
**Mitigation Strategy**:
- 要件では永続化スコープ外と明記
- 将来的にデータベース追加可能（MongoDB、PostgreSQL）
- インメモリ実装をインターフェース化して後から差し替え可能にする
**Source**: Requirements constraints

---

### Risk 3: XSS攻撃
**Description**: ユーザー入力（メッセージ、ユーザー名）を通じたXSS攻撃
**Likelihood**: 中（ユーザー入力がある以上常にリスク）
**Impact**: 高（セキュリティ侵害）
**Mitigation Strategy**:
- サーバー側: express-validatorで入力検証、サニタイゼーション
- クライアント側: Reactの自動エスケープ（dangerouslySetInnerHTML使用禁止）
- helmet middleware でCSP設定
- メッセージ長制限、特殊文字のホワイトリスト
**Source**: OWASP Security Cheat Sheet

---

### Risk 4: 接続切断とメッセージロスト
**Description**: ネットワーク不安定時の接続切断とメッセージ送信失敗
**Likelihood**: 中（モバイル環境など）
**Impact**: 中（ユーザー体験低下）
**Mitigation Strategy**:
- Socket.IOの自動再接続機能活用（exponential back-off）
- Acknowledgements機能で送信確認
- クライアント側で送信中状態表示（スピナー）
- 再接続失敗時のエラーメッセージ表示
**Source**: Socket.IO documentation, requirements (availability)

## Design Boundaries & Parallelization

### Component Independence Analysis
**Independently Developable Components**:
- **フロントエンドUI**: インターフェース定義後、独立開発可能（依存: なし）
- **バックエンドAPI**: Socket.IOイベント定義後、独立開発可能（依存: なし）
- **メッセージストア**: インターフェース定義後、独立開発可能（依存: なし）

**Sequential Dependencies**:
- Socket.IOイベント契約定義が最優先（フロント/バックエンド共通）
- ユーザー認証ロジックがメッセージ送信に先行必要
- 統合テストはすべてのコンポーネント完成後

**Critical Path**: Socket.IOイベント定義 → 認証実装 → メッセージング実装 → 統合

### Integration Points
**External Boundaries**:
- **ユーザー - Webブラウザ**: HTTP/HTTPS、WebSocket (wss://)
- **クライアント - サーバー**: Socket.IO protocol (Engine.IO transport)

**Internal Boundaries**:
- **Presentation Layer (React) ↔ Transport Layer (Socket.IO Client)**: Custom React hooks
- **Transport Layer (Socket.IO Server) ↔ Business Logic (Message/User管理)**: Event handlers
- **Business Logic ↔ Data Layer (In-memory store)**: Repository interface

---

*Last Updated: 2026-02-02T00:00:00Z*

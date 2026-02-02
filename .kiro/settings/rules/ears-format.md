# EARS Format Guidelines

## Overview
EARS (Easy Approach to Requirements Syntax) provides structured patterns for writing clear, testable requirements.

## Basic Patterns

### 1. Ubiquitous Requirements (Always Active)
**Pattern**: `[Subject] shall [action]`
**Example**: `システムは全てのメッセージを保存すること`

### 2. Event-Driven Requirements
**Pattern**: `When [trigger], [Subject] shall [action]`
**Example**: `ユーザーが送信ボタンをクリックしたとき、システムはメッセージを送信すること`

### 3. State-Driven Requirements
**Pattern**: `While [state], [Subject] shall [action]`
**Example**: `ユーザーがログイン中の間、システムはチャット画面を表示すること`

### 4. Unwanted Behavior Requirements
**Pattern**: `If [condition], then [Subject] shall [action]`
**Example**: `接続が切断された場合、システムはエラーメッセージを表示すること`

### 5. Optional Feature Requirements
**Pattern**: `Where [feature is included], [Subject] shall [action]`
**Example**: `ファイル共有機能が含まれる場合、システムは画像ファイルをアップロードできること`

## Key Principles
- Use appropriate subject (システム/アプリケーション/サービス for software)
- Focus on observable, testable behavior
- Avoid implementation details
- Use consistent terminology
- Each requirement should be atomic and verifiable

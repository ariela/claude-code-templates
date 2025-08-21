---
description: 実装計画フェーズ - タスク生成・管理、進捗追跡、specs/tasks.md生成・更新
allowed-tools: Read, Write, Edit, mcp__context7__*, mcp__firecrawl__*, WebSearch, TodoWrite, Bash
---

**設計ドリブン開発ワークフロー - フェーズ3: 実装計画**

あなたは実装計画の専門家として、詳細設計書を基に実装可能なタスクリストを作成します：

## 実行手順

### 1. 前提確認・初期設定
```
1. action: "prerequisite_check"
   - specs/requirements.mdの存在・内容確認
   - specs/design.mdの存在・内容確認
   - 前フェーズ未完了の場合は該当コマンドの実行を促す

2. action: "setup_task_context"
   - タスクIDの生成（TASK-YYYYMMDD-HHMM）
   - 設計・要件との関連付け確立
   - 既存tasks.mdがある場合は更新・進捗継続モード
```

### 2. 設計分析・実装スコープ決定
```
1. action: "analyze_design_document"
   - 設計要素の実装複雑度分析
   - AI実行指示の抽出・整理
   - 技術的依存関係の分析

2. action: "determine_implementation_scope"
   - MVP（Minimum Viable Product）スコープの定義
   - 実装優先度の決定（High/Medium/Low）
   - 段階的リリース戦略の検討
```

### 3. タスク分解・構造化

#### 3.1 アーキテクチャ別タスク分解
```
1. action: "decompose_by_architecture_layer"
   - データ層タスク（DB、スキーマ、マイグレーション）
   - ビジネス層タスク（API、ビジネスロジック）
   - プレゼンテーション層タスク（UI、フロントエンド）
   - インフラ層タスク（デプロイ、CI/CD）

2. action: "identify_cross_cutting_tasks"
   - セキュリティ対応タスク
   - パフォーマンス最適化タスク
   - テスト実装タスク
   - ドキュメント作成タスク
```

#### 3.2 依存関係分析
```
1. action: "analyze_task_dependencies"
   - 技術的依存関係の特定
   - 論理的依存関係の特定
   - 並行実行可能タスクの特定

2. action: "create_dependency_graph"
   - タスク間依存関係の可視化
   - クリティカルパスの特定
   - ボトルネックの識別
```

### 4. タスク詳細化

#### 4.1 各タスクの構造化
各タスクに以下の情報を付与：

**基本情報**:
- タスクID（TASK-001形式）
- タスク名・説明
- 優先度（High/Medium/Low）
- 見積工数（時間単位）
- 担当者（開発者）

**トレーサビリティ**:
- 関連要件へのリンク（REQ-F001等）
- 関連設計へのリンク（DES-A001等）
- 影響するタスクのリスト

**実装詳細**:
- 実装内容の具体的説明
- コードサンプル・テンプレート
- AI実行指示の詳細化
- ファイル・ディレクトリ構造

**受入条件**:
- 機能的受入条件（動作確認項目）
- 品質的受入条件（パフォーマンス、セキュリティ）
- テスト条件

#### 4.2 AI実行指示の詳細化
設計書のAI実行指示をより具体的に：

```
例: データベーススキーマ実装タスク

AI実行指示:
1. action: "create_file"
   path: "database/schema.sql"
   content: "完全なDDL文（CREATE TABLE、INDEX、CONSTRAINT）"
   
2. action: "create_file" 
   path: "database/migrations/001_create_tables.sql"
   content: "マイグレーション用DDL（UP/DOWN）"
   
3. action: "create_file"
   path: "database/seeds/test_data.sql"
   content: "開発・テスト用初期データ"
   
4. action: "validate_schema"
   execute: "psql -f database/schema.sql"
   expected: "エラーなく実行完了"
```

### 5. 進捗管理システム設計

#### 5.1 ステータス管理
```
1. action: "define_task_status"
   - 🔲 未着手（Not Started）
   - ☑️ 進行中（In Progress）
   - ✅ 完了（Completed）
   
2. action: "setup_progress_tracking"
   - 進捗サマリー（完了率、残タスク数）
   - マイルストーン定義（MVP、フル機能等）
   - 工数実績管理
```

#### 5.2 リスク管理
```
1. action: "identify_implementation_risks"
   - 技術的リスク（新技術、複雑度）
   - スケジュールリスク（工数超過、依存関係）
   - 品質リスク（テスト不足、バグ混入）

2. action: "define_risk_mitigation"
   - リスク対策の具体化
   - 代替案の準備
   - エスカレーション基準
```

### 6. 実装プロセス定義

#### 6.1 開発プロセス
```
1. action: "define_development_workflow"
   - TDD（Test-Driven Development）適用
   - コードレビュープロセス
   - 継続的統合（CI）プロセス
   
2. action: "setup_quality_gates"
   - 単体テスト実行・カバレッジ確保
   - 静的解析・リンターチェック
   - セキュリティスキャン実行
```

#### 6.2 実装時の指示
```
1. action: "define_implementation_guidelines"
   - コーディング規約（言語別）
   - エラーハンドリング方針
   - ロギング・モニタリング方針
   
2. action: "setup_execution_context"
   - 実装順序の明確化
   - 並行作業可能箇所の特定
   - 統合テスト実施タイミング
```

### 7. タスク文書生成

#### 7.1 実装計画書作成
```
1. action: "generate_task_document"
   - テンプレート適用（.claude/templates/tasks-template.md使用）
   - タスクの優先度別整理
   - 依存関係グラフの埋め込み

2. action: "integrate_task_details"
   - 各タスクの詳細情報統合
   - AI実行指示の埋め込み
   - 進捗追跡テーブルの生成
```

#### 7.2 実装支援資料作成
```
1. action: "create_implementation_checklist"
   - タスク実行時のチェックリスト
   - 品質確認項目
   - デバッグ・トラブルシューティング指針

2. action: "setup_task_templates"
   - よく使用されるタスクパターンのテンプレート化
   - 実装・テストコードのボイラープレート
```

### 8. 進捗更新メカニズム

#### 8.1 進捗更新指示
実装時のタスク更新について：
```
AI実装時の進捗更新:
1. action: "update_task_status"
   task_id: "TASK-001" 
   status: "in_progress" # 実装開始時
   
2. action: "update_task_status"
   task_id: "TASK-001"
   status: "completed"   # 受入条件満足時
   completion_note: "実装詳細・注意点"
```

#### 8.2 品質保証チェック
```
1. action: "validate_task_completion"
   - 受入条件の全項目チェック
   - 関連タスクへの影響確認
   - 回帰テスト実行推奨

2. action: "update_progress_summary"
   - 全体進捗率の再計算
   - マイルストーン達成状況更新
   - 残工数・期間の再見積
```

### 9. タスク文書保存・整合性確認
```
1. action: "save_task_document"
   - path: "specs/tasks.md"
   - backup: 既存ファイルがある場合は.bakファイル作成
   - validation: 必須項目・フォーマット確認

2. action: "validate_task_consistency"
   - 要件・設計との整合性確認
   - タスク間の論理的整合性確認
   - 工数・期間の妥当性確認
```

## 対話ガイドライン

### タスク粒度の調整
1. **適切な粒度**: 1-2日で完了可能な単位
2. **測定可能性**: 進捗が明確に判断できる単位  
3. **独立性**: 他タスクへの依存を最小化
4. **テスト容易性**: 単独でテスト可能な単位

### 工数見積の精度向上
1. **過去実績参考**: 類似タスクの実績活用
2. **リスク考慮**: 不確実性に対するバッファ確保
3. **レビュー・テスト時間**: 実装時間の30-50%を追加
4. **学習コスト**: 新技術習得時間の考慮

### エラーハンドリング
- 工数超過リスク：タスク分割・優先度見直し
- 技術的困難：代替実装方法の検討
- 依存関係問題：実装順序の調整

## フェーズ完了条件
以下の条件を満たした時点でフェーズ完了：
- [ ] 全実装項目がタスク化されている
- [ ] 各タスクに適切な粒度・工数が設定されている
- [ ] タスク間依存関係が明確に定義されている
- [ ] AI実行指示が具体的に記述されている
- [ ] 進捗管理システムが準備されている
- [ ] 品質保証手順が定義されている
- [ ] 要件・設計との整合性が確認されている
- [ ] ユーザーが実装計画に合意している

**フェーズ完了時のメッセージ**:
```
🎯 実装計画フェーズが完了しました！

📋 生成されたファイル:
- specs/tasks.md (実装計画書)

🔄 次のステップ:
実装を開始するには、生成されたタスクリストを参照して実装作業を進めてください。

📝 実装計画の修正が必要な場合は、再度 `/tasks` コマンドを実行してください。
設計や要件の修正が必要な場合は `/design` または `/requirements` コマンドを実行してください。

💡 実装作業中は、tasks.mdの進捗ステータスを随時更新することを推奨します。
```

---

ユーザーの要求: $ARGUMENTS

まず要件定義書・詳細設計書の確認から開始し、実装可能なタスクリストの作成を進めてください。
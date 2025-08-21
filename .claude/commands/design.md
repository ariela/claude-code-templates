---
description: 詳細設計フェーズ - システムアーキテクチャ・DB・API・UI設計、図表生成、specs/design.md生成・更新
allowed-tools: Read, Write, Edit, mcp__context7__*, mcp__firecrawl__*, WebSearch, TodoWrite, Bash
---

**設計ドリブン開発ワークフロー - フェーズ2: 詳細設計**

あなたは詳細設計の専門家として、要件定義書を基に包括的な設計書を作成します：

## 実行手順

### 1. 前提確認・初期設定
```
1. action: "prerequisite_check"
   - specs/requirements.mdの存在・内容確認
   - 要件が未完了の場合は/requirementsの実行を促す
   - 設計コンテキストの把握

2. action: "setup_design_context"
   - 設計IDの生成（DES-YYYYMMDD-HHMM）
   - 要件定義との関連付け
   - 既存design.mdがある場合は更新モード
```

### 2. 要件分析・設計方針決定
```
1. action: "analyze_requirements"
   - 機能要件の設計への影響分析
   - 非機能要件の技術的制約抽出
   - 設計上のトレードオフ識別

2. action: "determine_architecture_pattern"
   - 要件に基づく適切なアーキテクチャパターン選択
   - 技術スタックの提案・確認
   - 設計原則の適用（SOLID、DRY等）
```

### 3. システムアーキテクチャ設計

#### 3.1 全体アーキテクチャ設計
```
1. action: "design_system_architecture"
   - レイヤー構成の設計（プレゼンテーション・ビジネス・データ層）
   - コンポーネント間の関係定義
   - データフロー設計

2. action: "generate_architecture_diagram"
   - Mermaidによる全体アーキテクチャ図生成
   - コンポーネント図の作成
   - 責務分離の明確化
```

#### 3.2 技術スタック選定
対話で以下を確認・決定：
- **フロントエンド**: React/Vue/Angular等
- **バックエンド**: Node.js/Python/Java等  
- **データベース**: PostgreSQL/MySQL/MongoDB等
- **インフラ**: AWS/Azure/GCP等

### 4. データベース設計

#### 4.1 データモデル設計
```
1. action: "design_data_model"
   - 要件からエンティティ抽出
   - エンティティ間関係の定義
   - 正規化の適用

2. action: "generate_er_diagram"
   - MermaidによるER図生成
   - テーブル定義（カラム、制約、インデックス）
   - データ辞書の作成
```

#### 4.2 パフォーマンス考慮
- インデックス戦略
- パーティショニング検討
- クエリ最適化方針

### 5. API設計

#### 5.1 RESTful API設計
```
1. action: "design_api_endpoints"
   - リソース設計（URL構造）
   - HTTPメソッド・ステータスコード定義
   - リクエスト・レスポンス形式設計

2. action: "generate_api_sequence_diagram"
   - 主要APIフローのシーケンス図生成
   - エラーハンドリングフローの設計
   - 認証・認可フローの設計
```

#### 5.2 API仕様書作成
- OpenAPI(Swagger)形式での仕様定義
- サンプルリクエスト・レスポンス
- エラーレスポンス定義

### 6. UI/UX設計

#### 6.1 画面設計
```
1. action: "design_screen_flow"
   - ユーザージャーニー分析
   - 画面遷移図の生成（Mermaid）
   - 主要画面のワイヤーフレーム定義

2. action: "design_component_structure"
   - UI コンポーネント階層設計
   - 状態管理設計（Redux/Vuex等）
   - レスポンシブデザイン方針
```

#### 6.2 UXパターン適用
- デザインパターンの適用
- アクセシビリティ考慮
- ユーザビリティ原則の適用

### 7. セキュリティ設計

#### 7.1 認証・認可設計
```
1. action: "design_security_architecture"
   - 認証方式選択（JWT/OAuth2.0等）
   - 認可システム設計（RBAC/ABAC）
   - セッション管理方針

2. action: "generate_security_sequence_diagram"
   - 認証フローのシーケンス図
   - 権限チェックフローの設計
```

#### 7.2 セキュリティ対策
- OWASP Top 10対策
- データ暗号化方針  
- セキュリティヘッダー設定
- 入力検証・サニタイゼーション

### 8. パフォーマンス設計

#### 8.1 最適化戦略
```
1. action: "design_performance_strategy"
   - キャッシュ戦略（Redis/CDN等）
   - 非同期処理設計
   - ロードバランシング方針

2. action: "define_performance_targets"
   - レスポンス時間目標
   - スループット目標
   - 同時接続数目標
```

### 9. AI実行指示の生成
各設計要素にAI実行可能な指示を付与：

```
例: データベーススキーマ実装
1. action: "create_schema_file"
   path: "database/schema.sql"
   tables: ["users", "products", "orders"]
   
2. action: "create_migration_files"
   pattern: "timestamp_description.sql"
   rollback: "enabled"
```

### 10. 設計書生成・更新
```
1. action: "generate_design_document"
   - テンプレート適用（.claude/templates/design-template.md使用）
   - 図表の埋め込み（Mermaid/PlantUML）
   - 一意IDの付与（DES-A001, DES-D001等）

2. action: "integrate_diagrams"
   - アーキテクチャ図の統合
   - ER図の統合
   - シーケンス図の統合

3. action: "save_design_document"
   - path: "specs/design.md"
   - backup: 既存ファイルがある場合は.bakファイル作成
```

### 11. 整合性チェック・品質確認
```
1. action: "validate_design_consistency"
   - 要件と設計の整合性確認
   - 設計要素間の一貫性チェック
   - 非機能要件の実現性確認

2. action: "check_design_quality"
   - SOLID原則の適用確認
   - セキュリティ要件の実装確認
   - パフォーマンス要件の実現性確認
```

### 12. 要件への影響確認
設計変更が要件に影響する場合：
```
1. action: "analyze_requirements_impact"
   - 変更が必要な要件項目の特定
   - 影響範囲の分析

2. action: "update_requirements_if_needed"
   - requirements.mdの該当箇所更新
   - 変更理由・影響の記録
```

## 対話ガイドライン

### 設計決定プロセス
1. **技術選択**: 要件に基づく最適な技術の提案・確認
2. **トレードオフ説明**: 設計選択の根拠・代替案の説明
3. **実装性確認**: 実装の複雑さ・期間の見積り
4. **拡張性検討**: 将来の拡張可能性の考慮

### 図表生成原則
- **Mermaid優先**: メンテナンス性を考慮
- **PlantUML併用**: Mermaidで表現困難な場合
- **図表説明**: 各図表に詳細説明を付与

### エラーハンドリング
- 技術的実現性の問題：代替案提示
- 要件との矛盾：要件定義との調整提案
- 設計要素間の矛盾：整合性を保つ修正案提示

## フェーズ完了条件
以下の条件を満たした時点でフェーズ完了：
- [ ] システムアーキテクチャが定義されている
- [ ] データベース設計が完了している
- [ ] API設計が完了している  
- [ ] UI/UX設計が完了している
- [ ] セキュリティ設計が完了している
- [ ] パフォーマンス設計が完了している
- [ ] 各設計要素にAI実行指示が付与されている
- [ ] 要件との整合性が確認されている
- [ ] ユーザーが設計内容に合意している

**フェーズ完了時のメッセージ**:
```
🎯 詳細設計フェーズが完了しました！

📋 生成されたファイル:
- specs/design.md (詳細設計書)

🔄 次のステップ:
実装計画フェーズに進むには以下のコマンドを実行してください：
`/tasks`

📝 設計の修正が必要な場合は、再度 `/design` コマンドを実行してください。
要件定義の修正が必要な場合は `/requirements` コマンドを実行してください。
```

---

ユーザーの要求: $ARGUMENTS

まず要件定義書の確認から開始し、要件を基にした包括的な詳細設計を進めてください。
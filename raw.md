# CLAUDE - Mezzio + Laminas開発指針

## 🎯 プロジェクト方針

PHP 8.4 + FrankenPHP 1.9.0環境でのMezzioベースアプリケーション開発。
疎結合アーキテクチャによる高性能・高保守性システムの構築を目指す。

## 🏗️ アーキテクチャ原則

- **疎結合設計**: サービス間直接依存禁止
- **共通ライブラリ戦略**: 共有コードは共通ライブラリのみ
- **PSR準拠必須**: 標準インターフェース活用
- **PHP 8.4機能活用**: プロパティフック必須使用

## 📋 技術スタック

### 必須ライブラリ

| 用途 | ライブラリ | バージョン | 選択理由 |
|------|-----------|-----------|----------|
| フレームワーク | `mezzio/mezzio` | ^3.19 | PSR準拠、軽量、拡張性 |
| ルーティング | `mezzio/mezzio-fastroute` | ^3.11 | 高速、キャッシュ対応 |
| DI Container | `laminas/laminas-servicemanager` | ^4.0 | PSR-11準拠 |
| HTTP | `laminas/laminas-diactoros` | ^3.3 | PSR-7/17準拠 |
| データベース | `doctrine/dbal` | ^4.3 | クエリビルダー、抽象化 |
| スケジューラ | `symfony/scheduler` | ^7.2 | cron代替、分散対応 |
| CLI | `symfony/console` | ^7.2 | コマンド管理 |
| テンプレート | `mezzio/mezzio-twig` | ^2.17 | セキュア、高機能 |
| バリデーション | `laminas/laminas-validator` | ^3.0 | 多機能、拡張可能 |
| フィルタリング | `laminas/laminas-filter` | ^2.37 | XSS対策必須 |
| キャッシュ | `laminas/laminas-cache` | ^3.13 | 多様なアダプター |
| ログ | `monolog/monolog` | ^3.8 | 構造化ログ対応 |
| API仕様 | `cebe/openapi` | ^1.8 | OpenAPI 3.1準拠 |

### 実行環境

- **PHP**: 8.4+ (プロパティフック必須)
- **Webサーバー**: FrankenPHP 1.9.0 (ワーカーモード)
- **データベース**: MySQL 8.0+ / PostgreSQL 15+

## 🚨 必須開発ルール

### 1. ライブラリ機能優先原則
**既存ライブラリ機能がある場合、自前実装は絶対禁止**

| 機能 | 必須使用ライブラリ | 禁止行為 |
|------|------------------|----------|
| データベースクエリ | Doctrine DBAL | 生SQL記述 |
| バリデーション | Laminas Validator | 独自バリデーター作成 |
| データフィルタリング | Laminas Filter | 独自フィルター作成 |
| タスクスケジューリング | Symfony Scheduler | cron直接使用 |
| キャッシュ処理 | Laminas Cache | 独自キャッシュ実装 |
| ログ出力 | Monolog | 独自ログ機能作成 |
| パスワードハッシュ | PHP標準関数 | laminas-crypt使用 |

### 2. PHP 8.4プロパティフック必須
- **getter/setterは全てプロパティフックで実装**
- 従来のgetName()/setName()メソッドは使用禁止
- 読み取り専用・計算プロパティを活用

### 3. PSR標準準拠
- PSR-7 (HTTP Message), PSR-11 (Container), PSR-15 (Middleware)
- PSR-16 (Simple Cache), PSR-17 (HTTP Factories)
- **PSR-12コーディング規約必須準拠**: 全コードはPSR-12準拠

### 4. 名前空間とuse文ルール
- **`use`キーワード積極使用必須**: 名前空間含みクラス名の直接記述禁止
- ファイル先頭でクラス・インターフェース・トレイト全てをimport
- 必要時にはエイリアス使用で名前衝突回避（例: `use Doctrine\DBAL\Connection as DBALConnection`）

## 📁 ディレクトリ構造

```
project/
└── sources/
    ├── common-layer/           # 共通ライブラリ群（フレームワーク使用禁止）
    │   └── src/                # namespace: \Earthquake\Common
    ├── services-scheduler/     # スケジューラサービス
    │   ├── config/
    │   │   ├── scheduler.php   # スケジューラー設定
    │   │   └── tasks.php       # タスク定義
    │   ├── src/                # namespace: \Earthquake\Services\Scheduler
    │   │   └── Scheduler/      # Symfony Scheduler実装
    │   │       ├── Task/       # タスククラス
    │   │       └── Event/      # イベントリスナー
    │   └── bin/
    │       └── scheduler       # 実行スクリプト
    ├── admin-service/          # 管理コンソールサービス
    │   ├── config/
    │   │   ├── container.php
    │   │   ├── routes.php
    │   │   └── csrf.php        # CSRF保護設定
    │   ├── public/
    │   │   ├── index.php
    │   │   └── assets/         # CSS, JS, 画像
    │   └── src/                # namespace: \Earthquake\Services\Admin
    │       ├── Handler/
    │       │   └── Form/       # フォームハンドリング
    │       └── templates/      # Twigテンプレート
    │           ├── layout/
    │           └── pages/
    └── services-api-*/         # REST APIサービス
        ├── config/
        │   ├── container.php   # DI設定
        │   ├── routes.php      # ルーティング設定
        │   └── middleware.php  # ミドルウェア設定
        ├── public/
        │   └── index.php       # エントリーポイント（ワーカーモード対応）
        └── src/                # namespace: \Earthquake\Services\Api\*
            ├── Handler/        # リクエストハンドラー
            ├── Middleware/     # カスタムミドルウェア
            └── Factory/        # Factory実装
```

## 🔧 実装指針

### 共通ライブラリ層
- フレームワーク(mezzio/mezzio)への依存禁止

### Repository層
- `Earthquake\Common\Repository\AbstractRepository`を継承
- `Doctrine\DBAL\Query\QueryBuilder`必須使用
- `Doctrine\DBAL\Connection::transactional()`でトランザクション処理
- ページネーション機能標準装備

### Handler層
- `Psr\Http\Server\RequestHandlerInterface`実装必須
- `Laminas\Validator\ValidatorChain`でバリデーション
- `Laminas\Diactoros\Response\JsonResponse`で構造化エラーレスポンス
- `Psr\Log\LoggerInterface`でセキュリティログ出力

### Entity層
- PHP 8.4プロパティフック活用
- 不変オブジェクト設計推奨
- toArray()プロパティで配列変換

### Factory層
- `Laminas\ServiceManager\Factory\FactoryInterface`実装
- `Psr\Container\ContainerInterface`使用
- 設定の外部化推進

## 🔒 セキュリティ要件

### パスワード処理
- **PASSWORD_ARGON2ID**必須使用（PHP標準定数）
- `laminas-crypt`使用禁止（廃止済み）
- `Earthquake\Common\Security\PasswordService`でパスワード強度チェック実装

### SQLインジェクション対策
- `Doctrine\DBAL\Query\QueryBuilder`でプリペアドステートメント必須
- 生SQL記述完全禁止
- `QueryBuilder::setParameter()`でパラメータバインディング徹底

### XSS対策
- `Laminas\Filter\StripTags`と`Laminas\Filter\StringTrim`使用必須
- `Twig\Environment`でテンプレートエスケープ設定
- ユーザー入力の完全フィルタリング

### CSRF対策
- 管理画面で`Mezzio\Csrf\CsrfMiddleware`必須
- `Mezzio\Csrf\CsrfGuardInterface`でトークン検証徹底

## 🚀 FrankenPHP最適化

### ワーカーモード設定
- `\frankenphp_handle_request()`関数使用
- リクエスト数制限設定（推奨：1000req）
- `gc_collect_cycles()`でガベージコレクション定期実行
- `memory_get_usage()`でメモリ使用量監視

### パフォーマンス設定
- OPcacheの最適化（`opcache.enable=1`）
- メモリ制限の適切な設定（`memory_limit`）
- `ignore_user_abort(true)`で長時間実行対応設定
- `Earthquake\Common\Performance\PerformanceMonitor`でメトリクス収集

## ⚠️ 廃止・代替コンポーネント

### 使用禁止ライブラリ

| 廃止ライブラリ | 廃止日 | 代替 | 緊急度 |
|---------------|--------|------|--------|
| `laminas-crypt` | 2024年7月 | PHP標準`password_*()`関数 | 🔴即座 |
| `laminas-api-tools` | セキュリティオンリー | `cebe/openapi` + Mezzio | 🟡2025年6月 |
| `laminas-mvc` | 2025年11月予定 | Mezzio | 🟡2025年9月 |

### Laminas Validator v3対応
- **破壊的変更**: v2の可変設定からv3の不変オブジェクト設計
- **対応必須**: `Laminas\Validator\EmailAddress`等の設定は初期化時に完全指定
- **移行期限**: 即座（v3.0リリース済み）

```php
// ❌ v2での可変設定（v3で削除済み）
$validator = new \Laminas\Validator\EmailAddress();
$validator->setOption('domain', 'example.com');

// ✅ v3での不変オブジェクト設計（必須対応）
$validator = new \Laminas\Validator\EmailAddress([
    'domain' => 'example.com'
]);
```

## 🚫 禁止事項

### 絶対禁止
- 生SQL記述・文字列結合クエリ
- スーパーグローバル変数直接使用
- 従来getter/setter実装
- 独自バリデーション・フィルタリング
- 独自キャッシュ・ログ実装
- laminas-crypt使用
- **名前空間含み完全修飾クラス名の直接記述**（`use`文必須）

### アンチパターン
- 巨大Handler・Service
- 直接データベース接続
- グローバル状態使用
- 責任の混在

## 📊 監視・ログ

### ログ設定
- **`Monolog\Logger`**構造化ログ必須
- `Monolog\Processor\PsrLogMessageProcessor`で機密情報自動マスキング
- 本番：`Monolog\Handler\RotatingFileHandler`
- 開発：`Monolog\Handler\StreamHandler`で標準出力

### セキュリティ監視
- `Earthquake\Common\Security\SecurityLogger`でログイン試行記録
- SQLインジェクション検知・自動ログ出力
- 不審活動アラート（`Psr\Log\LogLevel::CRITICAL`）
- 脅威レベル判定機能

### パフォーマンス監視
- 実行時間・メモリ使用量記録
- 閾値超過アラート（`Psr\Log\LogLevel::WARNING`）
- `Earthquake\Common\Performance\PerformanceMonitor`でFrankenPHPワーカーメトリクス

## 🧪 テスト要件

### 必須テスト
- **ユニットテスト**: Repository・Service層（`PHPUnit\Framework\TestCase`継承）
- **統合テスト**: Handler・API層（`Psr\Http\Message\ServerRequestInterface`モック使用）
- **セキュリティテスト**: SQLインジェクション・XSS（`Doctrine\DBAL\Exception\*`例外テスト）
- **パフォーマンステスト**: 負荷・メモリリーク（`memory_get_peak_usage()`監視）

### テストツール
- `PHPUnit\Framework\TestCase`: ユニット・統合テスト
- `PHPStan\Testing\TestCase`: 静的解析（Level 8推奨）
- `PHP_CodeSniffer\Standards`: コーディング規約チェック（PSR-12準拠）

## 🎯 開発推奨フロー

1. **設計**: インターフェース定義・DI設定
2. **Repository実装**: データアクセス層
3. **Service実装**: ビジネスロジック層
4. **Handler実装**: プレゼンテーション層
5. **テスト作成**: 各層のテストコード
6. **Factory作成**: DI設定・依存関係注入

---

**最終更新**: 2025年9月12日  
**PHP**: 8.4+ 必須  
**FrankenPHP**: 1.9.0+ 推奨

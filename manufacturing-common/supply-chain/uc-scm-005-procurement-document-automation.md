---
use_case_id: UC-SCM-005
title: 調達書類自動作成
category: Industry-specific
sub_category: Supply Chain
complexity_level: Lv2-3
implementation_types:
  - RAG
  - Automation
  - AI Tools
tags:
  - 文書自動化
  - 調達書類
  - 業務効率化
  - 標準化
  - コンプライアンス
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 書類テンプレート・標準文言・契約条項
  embedding_strategy: 文書構造とテンプレートのチャンキング
  chunk_size: 700
  top_k: 5
  similarity_threshold: 0.70
  sample_content: 発注書テンプレート、契約書テンプレート、標準取引条件、過去の優良事例
---

# 調達書類自動作成

## Overview

発注書、契約書、見積依頼書などの調達関連書類を自動生成するAIシステム。必要情報を入力するだけで、標準フォーマットに準拠した完全な書類を即座に作成します。過去の書類と標準文言をRAGで学習し、適切な表現と必要条項を自動的に含めます。作成時間を大幅に短縮し、記載漏れやミスを防止。書類の標準化とコンプライアンス強化を実現します。

## Business Value

- 時間削減: 書類作成時間を75%削減（1件40分→10分）
- コスト削減: 事務工数削減により年間約400万円のコスト削減
- 品質向上: 記載ミス・漏れゼロ、標準化された高品質書類
- その他の効果: 承認プロセスの迅速化、監査対応の容易化、コンプライアンス強化

## Required Components

- LLM（GPT-4またはClaude）
- ベクトルデータベース（テンプレート・標準文言格納）
- Difyワークフロー機能
- 文書生成機能（PDF、Word出力）
- 購買管理システムとの連携API
- 電子署名システム連携（オプション）

## Implementation Steps

1. 各種書類テンプレートをナレッジベースに登録
2. 標準取引条件、契約条項を登録
3. 過去の優良書類サンプルを登録
4. 入力フォームの作成
5. 生成ロジックをプロンプトで定義
6. サンプルで出力品質を検証
7. 承認ワークフローとの連携
8. 実運用開始、テンプレートの継続改善

## Sample Prompts

```
以下の情報から発注書を作成してください：

サプライヤー: {supplier_name}
品目: {item_list}
数量: {quantities}
単価: {unit_prices}
納期: {delivery_date}
納入場所: {delivery_location}

標準フォーマットに従い、以下を含めてください：
1. 発注番号（自動採番）
2. 取引条件（支払条件、納期条件等）
3. 品質要求事項
4. 検収条件
5. 特記事項

サプライヤーマスタから適切な取引条件を自動適用してください。
```

```
新規サプライヤー{supplier_name}との基本契約書を作成してください：

契約内容:
- 取引品目カテゴリー: {item_category}
- 契約期間: {contract_period}
- 支払条件: {payment_terms}
- 品質基準: {quality_standards}

標準契約書テンプレートをベースに、以下を含めてください：
1. 基本契約条項
2. 品質保証条項
3. 機密保持条項
4. 責任制限条項
5. その他必要条項

業界標準と社内規定に準拠した内容にしてください。
```

```
見積依頼書（RFQ）を作成してください：

案件: {project_name}
対象品目: {items}
必要仕様: {specifications}
希望納期: {target_delivery}
評価基準: {evaluation_criteria}

複数サプライヤーへの一括送付用に、以下を含めてください：
1. 案件概要
2. 詳細仕様
3. 見積条件（有効期限、提出期限等）
4. 評価方法
5. 質問受付方法

過去の同様案件を参考に、漏れのない内容にしてください。
```

## Data Requirements

- データの種類: 書類テンプレート、標準取引条件、契約条項、過去の書類サンプル
- データ量: 各書類種別のテンプレート、過去1年分の優良書類サンプル
- データ形式: Word、PDF、テキスト
- データ準備:
  1. テンプレートの標準化
  2. 標準文言の整備
  3. 契約条項の法務確認
  4. サプライヤーマスタとの連携

## Technical Considerations

- システム要件: LLM API（月間1,000リクエスト）、ベクトルDB（5,000エントリー）
- セキュリティ: 契約情報の機密保持、電子署名対応、アクセス権限管理
- パフォーマンス: 1件の書類生成3分以内
- 制限事項:
  - 特殊な契約条件は手動調整が必要
  - 法的効力は法務部門の確認が必要
  - 最終承認は権限者が実施
  - 高額・重要案件は個別レビュー推奨
- スケーラビリティ: 全書類種別、全部門への展開が容易

## Reference Links

- https://www.dify.ai/blog/procurement-document-automation
- https://contract-automation.com/templates
- https://purchasing-efficiency.com/digitalization

## Tags

- 文書自動化
- 調達書類
- 業務効率化
- 標準化
- コンプライアンス
- サプライチェーン
- 製造業
- 購買管理

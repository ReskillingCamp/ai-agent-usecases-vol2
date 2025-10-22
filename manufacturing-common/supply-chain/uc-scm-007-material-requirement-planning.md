---
use_case_id: UC-SCM-007
title: 資材所要量計画（MRP）
category: Industry-specific
sub_category: Supply Chain
complexity_level: Lv4-5
implementation_types:
  - AI Tools
  - RAG
  - Automation
tags:
  - MRP
  - 資材計画
  - 部品展開
  - 所要量計算
  - 発注計画
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: BOM・在庫情報・リードタイム
  embedding_strategy: 部品構成と所要量の階層チャンキング
  chunk_size: 600
  top_k: 8
  similarity_threshold: 0.75
  sample_content: 部品表（BOM）、在庫データ、リードタイム情報、過去の所要量実績
---

# 資材所要量計画（MRP）

## Overview

生産計画から必要な資材・部品の所要量を自動計算し、最適な発注計画を立案するAIシステム。製品のBOM（部品表）を展開し、必要な資材の種類、数量、必要時期を正確に算出します。現在在庫、発注残、リードタイムを考慮して、過不足のない発注計画を生成。AIが過去の実績から最適な発注タイミングと数量を学習し、欠品と過剰在庫の両方を防止します。

## Business Value

- 時間削減: MRP計算時間を80%削減（週12時間→2.4時間）
- コスト削減: 適正在庫維持により年間約3,500万円のコスト削減
- 品質向上: 部品欠品による生産停止ゼロ、在庫精度98%以上
- その他の効果: 生産計画変更への迅速な対応、キャッシュフロー改善、調達リードタイムの最適化

## Required Components

- LLM（GPT-4またはClaude）with 計算能力
- MRP計算エンジン
- ベクトルデータベース（BOM・在庫データ格納）
- Difyワークフロー機能
- 生産管理システムとの連携API
- 在庫管理システム連携

## Implementation Steps

1. 製品BOM（部品表）をナレッジベースに登録
2. 在庫データとの連携設定（リアルタイム取得）
3. リードタイム情報の整備
4. MRP計算ロジックの構築
5. 発注提案アルゴリズムの開発
6. サンプル生産計画でシミュレーション
7. アラート機能の設定（欠品リスク等）
8. 実運用開始、継続的な精度向上

## Sample Prompts

```
以下の生産計画に対するMRP計算を実施してください：

生産計画:
- 製品A: {quantity_a}個、納期{date_a}
- 製品B: {quantity_b}個、納期{date_b}
- 製品C: {quantity_c}個、納期{date_c}

各製品のBOMを展開し、以下を算出：
1. 部品別所要量（総所要量、正味所要量）
2. 発注必要数量（現在在庫と発注残を考慮）
3. 発注推奨日（リードタイムを逆算）
4. 在庫推移予測
5. 欠品リスク品目
6. 過剰在庫リスク品目

発注計画表として出力してください。
```

```
生産計画が以下のように変更されました。MRP計画を再計算してください：

変更内容:
- 製品Aの数量を{old_qty}→{new_qty}に変更
- 製品Dを追加（{qty_d}個、納期{date_d}）
- 製品Bの納期を{old_date}→{new_date}に前倒し

変更前後の比較と、追加発注が必要な品目をハイライトしてください。
緊急手配が必要な部品があれば警告を出してください。
```

```
部品{part_number}のリードタイムが{old_lead}日から{new_lead}日に変更されました。
この変更が今後の生産計画に与える影響を分析してください：

今後3ヶ月の生産計画: {future_plan}

影響分析:
- 影響を受ける製品
- 必要な対応（発注前倒し等）
- 在庫計画の変更
- リスク評価
```

## Data Requirements

- データの種類: 製品BOM、在庫データ、発注残、リードタイム、生産計画
- データ量: 全製品のBOM、全部品の在庫データ（リアルタイム）
- データ形式: CSV、Excel、ERPシステムデータ
- データ準備:
  1. BOMの正確性確認・更新
  2. 在庫精度の向上
  3. リードタイムの実績分析
  4. 安全在庫の設定

## Technical Considerations

- システム要件: LLM API（月間2,000リクエスト）、ベクトルDB（100,000エントリー）、計算リソース
- セキュリティ: BOM情報の機密保持、在庫データの正確性担保
- パフォーマンス: MRP計算10分以内（1,000部品規模）、緊急再計算3分以内
- 制限事項:
  - BOMの正確性に依存
  - 在庫データの遅延は計算精度に影響
  - 極端な生産変動には手動調整が必要
  - 代替部品の判断は人間が実施
- スケーラビリティ: 最大100製品、10,000部品まで効率的に計算可能

## Reference Links

- https://www.dify.ai/blog/mrp-automation
- https://production-planning.com/material-requirements
- https://erp-systems.com/mrp-optimization

## Tags

- MRP
- 資材計画
- 部品展開
- 所要量計算
- 発注計画
- サプライチェーン
- 製造業
- 生産管理

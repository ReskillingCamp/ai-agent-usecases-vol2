---
use_case_id: UC-SCM-004
title: 需要予測システム
category: Industry-specific
sub_category: Supply Chain
complexity_level: Lv4-5
implementation_types:
  - AI Tools
  - RAG
  - Automation
tags:
  - 需要予測
  - 予測分析
  - 生産計画
  - 在庫計画
  - 機械学習
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 販売実績・市場データ・外部要因
  embedding_strategy: 時系列データと影響要因の統合チャンキング
  chunk_size: 600
  top_k: 10
  similarity_threshold: 0.73
  sample_content: 販売実績、受注データ、市場トレンド、季節要因、販促計画
---

# 需要予測システム

## Overview

過去の販売実績と市場動向から将来の需要を高精度で予測するAIシステム。季節変動、トレンド、プロモーション効果、外部要因（天候、経済指標等）を考慮した多角的な予測を実施します。機械学習とRAGにより、複雑な需要パターンを学習し、予測精度を継続的に向上。生産計画、調達計画、在庫計画の基礎となる信頼性の高い予測を提供します。

## Business Value

- 時間削減: 予測作成時間を60%削減（週8時間→3.2時間）
- コスト削減: 予測精度向上により在庫削減・欠品削減で年間約4,000万円のコスト削減
- 品質向上: 予測精度10ポイント向上、機会損失30%削減
- その他の効果: 生産計画の安定化、サプライチェーン全体の最適化、意思決定の迅速化

## Required Components

- LLM（GPT-4またはClaude）with データ分析能力
- 機械学習モデル（時系列予測）
- ベクトルデータベース（販売データ・市場情報格納）
- Difyワークフロー機能
- 販売管理システムとの連携API
- 外部データソース連携（オプション）

## Implementation Steps

1. 過去の販売実績をナレッジベースに登録
2. 影響要因の特定（季節、プロモーション、外部要因等）
3. 予測モデルの構築・トレーニング
4. 予測ロジックをプロンプトで定義
5. サンプルデータで予測精度を検証
6. 複数予測手法の組み合わせ設定
7. 定期予測レポートの自動生成
8. 実運用開始、継続的な精度改善

## Sample Prompts

```
今後3ヶ月の製品別需要予測を作成してください：

製品リスト: {product_list}
過去2年の販売実績: {sales_history}
今後の販促計画: {promotion_plan}
外部要因: {external_factors}

各製品について以下を提示：
1. 月別需要予測（数量）
2. 予測信頼区間
3. 主な影響要因
4. 前年同期比
5. リスク要因
6. シナリオ分析（楽観/基準/悲観）

予測精度の評価指標も含めてください。
```

```
製品{product_name}の需要が急増/急減しています。
原因を分析し、今後の予測を修正してください：

最近の販売実績: {recent_sales}
過去の類似パターン: {similar_patterns}
現在の市場状況: {market_conditions}

分析内容:
- 変動の原因仮説
- 一時的か持続的かの判定
- 修正後の需要予測
- 生産・調達への影響
- 推奨アクション
```

```
新製品{new_product}の需要予測を作成してください：

類似製品の実績: {similar_product_history}
市場調査結果: {market_research}
価格設定: {pricing}
販売チャネル: {sales_channels}
マーケティング投資: {marketing_budget}

過去の新製品立ち上げ実績を参考に、以下を予測：
- 初年度販売予測（月別）
- 普及曲線の想定
- リスク要因
- 在庫計画への示唆
```

## Data Requirements

- データの種類: 販売実績、受注データ、販促計画、市場データ、外部要因データ
- データ量: 最低2年分の販売実績（季節商品は3年以上推奨）
- データ形式: CSV、Excel、販売管理システムデータ
- データ準備:
  1. 販売データの整理・補正
  2. 異常値の処理
  3. 影響要因の数値化
  4. 外部データの収集

## Technical Considerations

- システム要件: LLM API（月間1,500リクエスト）、ベクトルDB（50,000エントリー）、機械学習環境
- セキュリティ: 販売データの機密保持、アクセス権限管理
- パフォーマンス: 月次予測生成30分以内、緊急時の予測更新10分以内
- 制限事項:
  - 市場の構造的変化は予測困難
  - 新製品は類似品からの推定
  - 予測は確率的、実績との乖離は必ず発生
  - 最終判断は人間が実施
- スケーラビリティ: 最大1,000製品の同時予測可能

## Reference Links

- https://www.dify.ai/blog/demand-forecasting-ai
- https://supply-chain-analytics.com/forecasting
- https://predictive-planning.com/demand-prediction

## Tags

- 需要予測
- 予測分析
- 生産計画
- 在庫計画
- 機械学習
- サプライチェーン
- 製造業
- データ分析

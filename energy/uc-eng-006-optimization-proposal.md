---
use_case_id: UC-ENG-006
title: 省エネルギー最適化提案システム
category: Industry-specific
sub_category: Energy
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Tools
  - Automation
tags:
  - 省エネ提案
  - 最適化
  - ROI計算
  - 投資計画
  - 効果検証
version: 1.0.0
last_updated: 2025-01-22
rag_config:
  knowledge_base_type: 省エネ技術・設備仕様・補助金制度
  embedding_strategy: 技術分野別・投資規模別のチャンキング
  chunk_size: 800
  top_k: 10
  similarity_threshold: 0.76
  sample_content: 省エネ技術カタログ、導入事例、投資回収計算式
---

# 省エネルギー最適化提案システム

## Overview

顧客の施設・設備に最適な省エネ施策を提案し、投資対効果を試算するAIシステム。省エネ技術カタログ、導入事例、補助金制度をRAGで管理し、顧客のエネルギー使用状況に応じたカスタマイズ提案を自動生成します。従来の提案書作成業務を60%削減し、提案品質と説得力を向上させます。複数の施策を組み合わせた最適化シミュレーション、投資回収期間の試算、補助金活用提案により、顧客の意思決定を支援します。導入後の効果検証レポートも自動生成し、継続的な改善サイクルを実現します。

## Business Value

- 時間削減: 提案書作成時間を60%削減（1件8時間→3.2時間）、複数パターン試算が容易に
- コスト削減: 顧客の省エネ投資ROIが平均20%向上、提案成約率が35%向上
- 品質向上: 提案の網羅性・精度が向上、顧客満足度スコア15ポイント向上
- その他の効果: 営業効率化、技術者のノウハウ共有、補助金活用率80%向上

## Required Components

- LLM（GPT-4またはClaude）with function calling機能
- ベクトルデータベース（Pinecone, Qdrant等）
- シミュレーションエンジン（省エネ効果試算）
- Difyワークフロー機能
- 省エネ設備カタログデータベース
- 補助金情報API（自治体・国の制度）
- 提案書自動生成ツール（PowerPoint, Word等）

## Implementation Steps

1. 省エネ技術カタログ、導入事例、補助金制度情報をナレッジベースに登録
2. 顧客のエネルギー消費データ・施設情報を収集
3. 施策候補抽出ワークフローを構築（顧客条件に適合する技術を選定）
4. ROI計算ロジックを実装（投資額、削減効果、補助金を考慮）
5. 複数施策の組み合わせ最適化アルゴリズムを構築
6. 提案書テンプレートを作成（自動生成機能）
7. 効果検証レポート生成機能を実装
8. パイロット運用（既存顧客で試験提案）
9. 本格運用開始（営業活動への組み込み）

## Sample Prompts

```
以下の顧客に対する最適な省エネ提案を作成してください。
顧客情報:
- 業種: {industry}
- 施設規模: {facility_size}
- 年間エネルギーコスト: {annual_cost}円
- 現在の主要設備: {current_equipment}
- 予算上限: {budget_limit}円

要求事項:
1. 投資回収期間3年以内の施策を優先
2. 複数施策の組み合わせパターンを3案提示
3. 各案のROI、削減効果、実施優先順位を明示
4. 適用可能な補助金制度をリストアップ

経営層向けの提案書を作成してください。
```

```
既存顧客の省エネ施策導入効果を検証してください。
導入施策:
- 施策内容: {implemented_measures}
- 導入日: {implementation_date}
- 投資額: {investment_amount}円
- 想定削減効果: {expected_savings}円/年

検証データ:
- 導入前消費量: {before_consumption}
- 導入後消費量: {after_consumption}

実際の削減効果、想定との差異、差異の原因分析、追加改善提案を含むレポートを作成してください。
```

```
最新の省エネ技術動向を踏まえ、顧客向け提案内容をアップデートしてください。
対象顧客: {customer_segment}
新技術情報: {new_technology_info}
現行提案内容: {current_proposal}

新技術の適用可能性、追加メリット、投資対効果の変化、提案内容の改善案を提示してください。
```

## Data Requirements

- データの種類: 省エネ技術カタログ、設備仕様・価格情報、導入事例、補助金制度情報、顧客施設データ
- データ量: 省エネ技術（200種類以上）、導入事例（100件以上）、補助金制度（最新情報）
- データ形式: PDF（技術資料）、CSV（コストデータ）、JSON（補助金情報API）
- データ準備:
  1. 省エネ技術カタログの整備・更新
  2. 導入事例の成果データ収集
  3. 補助金制度の最新情報収集
  4. ROI計算式の標準化

## Technical Considerations

- システム要件: LLM API（月間20,000リクエスト）、シミュレーション計算能力
- セキュリティ: 顧客データの暗号化、機密情報保護、アクセス制御
- パフォーマンス: 提案生成を10分以内、複数パターン試算を同時実行可能
- 制限事項:
  - 試算は標準条件に基づく概算（詳細は個別設計が必要）
  - 補助金制度は変更される可能性あり（最新情報確認必須）
  - 投資判断は顧客自身が実施（AIは情報提供）
  - 特殊な施設・設備は専門家の個別検討が必要
- スケーラビリティ: 顧客数増加に対応、技術カタログの継続的拡充

## Reference Links

- https://www.energy.gov/eere/energy-efficiency
- https://www.iea.org/topics/energy-efficiency
- https://www.ashrae.org/technical-resources/bookstore/energy-efficiency-guides

## Tags

- 省エネ提案
- 最適化
- ROI計算
- 投資計画
- 効果検証
- RAG
- 補助金活用
- 営業支援

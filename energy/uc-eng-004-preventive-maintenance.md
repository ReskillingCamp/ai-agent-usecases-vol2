---
use_case_id: UC-ENG-004
title: 予防保全計画システム
category: Industry-specific
sub_category: Energy
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Tools
  - Automation
tags:
  - 予防保全
  - 設備診断
  - 故障予測
  - メンテナンス計画
  - CBM
version: 1.0.0
last_updated: 2025-01-22
rag_config:
  knowledge_base_type: 保全マニュアル・故障事例・メーカー推奨事項
  embedding_strategy: 設備タイプ別・故障モード別のチャンキング
  chunk_size: 750
  top_k: 12
  similarity_threshold: 0.74
  sample_content: 保全計画書、過去の故障事例、部品交換履歴
---

# 予防保全計画システム

## Overview

設備の状態監視データと過去の故障事例を分析し、最適な予防保全計画を立案するAIシステム。保全マニュアル、故障事例、メーカー推奨事項をRAGで管理し、設備ごとの最適な保全タイミングを予測します。従来の時間基準保全（TBM）から状態基準保全（CBM）へ移行し、保全コストを35%削減します。故障予兆の早期検知により、計画外停止を80%削減し、設備稼働率を向上させます。部品在庫の最適化、保全作業の平準化により、効率的なメンテナンス体制を構築します。

## Business Value

- 時間削減: 保全計画作成時間を70%削減（週12時間→3.6時間）、緊急対応時間を60%削減
- コスト削減: 保全コストを年間35%削減（約3,500万円）、部品在庫コストを25%削減
- 品質向上: 計画外停止を80%削減、設備稼働率が92%から97%に向上
- その他の効果: 設備寿命の15%延伸、保全作業の安全性向上、エネルギー効率5%改善

## Required Components

- LLM（GPT-4またはClaude）with function calling機能
- ベクトルデータベース（Pinecone, Qdrant等）
- 時系列データベース（InfluxDB等）
- 異常検知AI（Anomaly Detection）
- Difyワークフロー機能
- 設備状態監視システム（SCADA等）
- 保全管理システム（CMMS）連携
- 部品在庫管理システム連携

## Implementation Steps

1. 保全マニュアル、故障事例、メーカー推奨事項をナレッジベースに登録
2. 設備状態監視データを時系列DBに格納（センサーデータ、運転パラメータ）
3. 過去の故障・保全履歴をデータベース化（最低5年分）
4. 故障予測モデルを構築（機械学習または統計的手法）
5. 最適保全タイミング算出ワークフローを作成
6. 保全計画自動生成機能を実装（作業負荷平準化、部品調達考慮）
7. アラート・通知システムを構築
8. パイロット運用（重要設備で試験実施）
9. 本格運用開始（全設備への展開）

## Sample Prompts

```
設備Aの次回保全時期を推奨してください。
現状データ：
- 稼働時間: {operating_hours} 時間
- 振動レベル: {vibration_level}
- 温度: {temperature}
- 前回保全日: {last_maintenance_date}
- 過去の故障履歴: {failure_history}

推奨保全時期、根拠、実施すべき作業内容、必要部品リストを提示してください。
```

```
以下の設備異常アラートを分析し、対応優先度を判定してください。
アラート情報：
- 設備名: {equipment_name}
- 異常内容: {anomaly_description}
- 発生時刻: {occurrence_time}
- センサーデータ: {sensor_data}

緊急度レベル（高/中/低）、推定故障モード、推奨対応、放置した場合のリスクを提示してください。
過去の類似事例も参照してください。
```

```
次四半期の保全計画を最適化してください。
対象設備: {equipment_list}
制約条件:
- 保全要員: {available_staff}名/日
- 予算上限: {budget_limit}円
- 生産計画: {production_schedule}

設備別の保全スケジュール、作業内容、必要工数、概算コスト、部品発注タイミングを提示してください。
作業負荷を平準化してください。
```

## Data Requirements

- データの種類: 保全マニュアル、過去の故障・保全履歴、設備状態監視データ、部品仕様・在庫情報、メーカー技術資料
- データ量: 全設備5年分の保全履歴、リアルタイムセンサーデータ、故障事例データベース（100件以上）
- データ形式: PDF（マニュアル）、CSV（時系列データ）、JSON（API連携）、画像（故障写真）
- データ準備:
  1. 紙の保全記録をデジタル化
  2. センサーデータの収集・蓄積開始
  3. 故障事例を分類・構造化
  4. 部品情報をマスターデータ化

## Technical Considerations

- システム要件: LLM API（月間35,000リクエスト）、時系列DB（全設備5年分データ）、機械学習基盤
- セキュリティ: 設備情報の機密保護、改ざん防止、アクセス権限管理
- パフォーマンス: 故障予測を1分以内、保全計画生成を10分以内、リアルタイム監視対応
- 制限事項:
  - 新規導入設備は学習データ不足（初期は時間基準保全）
  - 予測精度は過去データの質に依存
  - 最終判断は保全責任者が実施
  - 特殊設備は専門家の見解が必要
- スケーラビリティ: 設備数増加に対応（5,000設備まで）、複数拠点への横展開可能

## Reference Links

- https://www.nist.gov/el/intelligent-systems-division-73500/production-systems-group/smart-manufacturing-operations
- https://www.api.org/oil-and-natural-gas/health-and-safety/refinery-and-plant-safety/process-safety/equipment-integrity
- https://www.sciencedirect.com/topics/engineering/predictive-maintenance

## Tags

- 予防保全
- 設備診断
- 故障予測
- メンテナンス計画
- CBM
- RAG
- 異常検知
- 稼働率向上

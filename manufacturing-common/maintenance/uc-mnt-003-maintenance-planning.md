---
use_case_id: UC-MNT-003
title: 保全計画最適化
category: Industry-specific
sub_category: Maintenance
complexity_level: Lv4-5
implementation_types:
  - AI Tools
  - RAG
  - Automation
tags:
  - 保全計画
  - スケジューリング
  - 最適化
  - リソース管理
  - 効率化
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 保全履歴・設備情報・リソースデータ
  embedding_strategy: 保全タスクとリソースの統合チャンキング
  chunk_size: 600
  top_k: 8
  similarity_threshold: 0.70
  sample_content: 保全計画表、過去の保全実績、設備台帳、要員スキル情報
---

# 保全計画最適化

## Overview

設備保全作業を効率的にスケジューリングし、リソースを最適配分するAIシステム。設備の重要度、保全周期、要員のスキルと空き状況、部品在庫などを考慮して、最適な保全計画を自動生成します。生産計画との調整により、生産への影響を最小化しながら必要な保全を確実に実施。過去の保全実績をRAGで学習し、作業時間の精度向上と効率的なリソース配分を実現します。

## Business Value

- 時間削減: 保全計画立案時間を65%削減（週4時間→1.4時間）
- コスト削減: 保全効率向上により年間約1,800万円のコスト削減
- 品質向上: 保全遅延ゼロ、設備稼働率向上、緊急保全30%削減
- その他の効果: 要員配置の最適化、残業削減、部品在庫の適正化

## Required Components

- LLM（GPT-4またはClaude）with スケジューリング機能
- 最適化アルゴリズム
- ベクトルデータベース（保全履歴・設備情報格納）
- Difyワークフロー機能
- 生産計画システムとの連携API
- カレンダー管理システム

## Implementation Steps

1. 設備台帳と保全基準をナレッジベースに登録
2. 過去の保全実績データを登録（作業時間、結果等）
3. 要員スキルマトリクスの整備
4. 生産計画システムとの連携設定
5. スケジューリングロジックをプロンプトで定義
6. サンプルデータで計画案を生成・検証
7. 自動通知機能の設定（保全予定、部品発注等）
8. 実運用開始、継続的な最適化

## Sample Prompts

```
今月の保全計画を作成してください：

対象設備: {equipment_list}
保全項目:
- 定期保全: {scheduled_maintenance_list}
- 法定点検: {regulatory_inspections}
- 予知保全: {predictive_maintenance_items}

制約条件:
- 利用可能要員: {available_personnel}
- 各要員のスキル: {skill_matrix}
- 生産計画: {production_schedule}
- 部品在庫状況: {parts_inventory}

以下を出力:
1. 日別保全スケジュール
2. 要員割り当て
3. 必要部品リスト
4. 生産への影響評価
5. リスク項目
```

```
緊急保全が発生しました。保全計画を調整してください：

緊急保全内容: {emergency_maintenance}
必要時間: {estimated_time}
優先度: 高

現在の保全計画: {current_schedule}

最小限の影響で計画を再調整し、以下を提示：
- 変更後のスケジュール
- 延期する保全項目（影響評価含む）
- 必要な追加リソース
- 生産計画への影響
```

```
来年度の年間保全計画を立案してください：

全設備の保全周期: {maintenance_cycles}
法定点検スケジュール: {regulatory_schedule}
工場休止日: {shutdown_days}

考慮事項:
- 同種設備の同時保全によるスキル効率化
- 生産繁忙期の回避
- 長期休暇期間の活用
- 部品発注リードタイム

月別計画と予算見積もりを提示してください。
```

## Data Requirements

- データの種類: 設備台帳、保全基準書、保全実績、要員情報、部品在庫
- データ量: 全設備の保全履歴（最低2年分）、要員スキルデータ
- データ形式: Excel、CSV、保全管理システムデータ
- データ準備:
  1. 設備ごとの保全周期整理
  2. 保全作業の標準時間設定
  3. 要員スキルの評価・登録
  4. 部品在庫との連携

## Technical Considerations

- システム要件: LLM API（月間800リクエスト）、ベクトルDB（30,000エントリー）
- セキュリティ: 保全情報の機密保持、アクセス権限管理
- パフォーマンス: 月次計画生成10分以内、緊急時の再計画3分以内
- 制限事項:
  - 予期せぬ設備故障は即時再計画が必要
  - 外部委託作業は調整に時間がかかる場合あり
  - 最終的な判断は保全責任者が実施
- スケーラビリティ: 最大300設備、50名の要員まで最適化可能

## Reference Links

- https://www.dify.ai/blog/maintenance-planning-optimization
- https://maintenance-management.com/scheduling
- https://production-planning.com/maintenance-coordination

## Tags

- 保全計画
- スケジューリング
- 最適化
- リソース管理
- 効率化
- 保全管理
- 製造業
- 設備管理

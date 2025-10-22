---
use_case_id: UC-MTG-004
title: 決定事項追跡システム
category: General Business
sub_category: Meeting Management
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Tools
  - Automation
tags:
  - 意思決定
  - トラッキング
  - ナレッジマネジメント
  - 会議管理
  - コンプライアンス
version: 1.0.0
last_updated: 2025-10-22
rag_config:
  knowledge_base_type: documents
  embedding_strategy: 決定事項ベースのチャンキング（会議議事録、決裁文書、ポリシー変更記録）
  chunk_size: 500
  top_k: 10
  similarity_threshold: 0.75
  sample_content: 過去の会議議事録、経営会議決定事項、プロジェクト承認記録、方針変更履歴
---

# 決定事項追跡システム

## Overview

過去の会議で決定された事項を一元管理し、検索可能にするシステム。AIが議事録から決定事項を自動抽出し、カテゴリー分類、タグ付け、関連文書とのリンク付けを行います。RAGを活用することで、「この件について過去にどのような決定があったか」「誰が承認したか」「その後の実施状況は」といった質問に即座に回答できます。エアウォーターグループの経営会議、部門会議、プロジェクト会議での決定事項を体系的に管理し、意思決定の一貫性と透明性を確保します。

## Business Value

- 時間削減: 過去の決定事項の検索時間を90%削減（30分→3分）
- コスト削減: 重複検討の防止により年間約250万円/部門のコスト削減
- 品質向上: 意思決定の一貫性向上、過去の決定との矛盾を防止
- その他の効果: コンプライアンス強化、監査対応の迅速化、ナレッジの継承

## Required Components

- LLM（GPT-4、Claude 3.5 Sonnet等）
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Dify RAG機能
- 議事録管理システム（SharePoint、Confluence等）との連携
- 決定事項追跡データベース（PostgreSQL、MongoDB等）
- 通知システム（メール、Slack等）

## Implementation Steps

1. 過去の議事録から決定事項を抽出・整理（過去2-3年分を推奨）
2. 決定事項の分類体系を設計（カテゴリー、タグ、重要度レベル）
3. ナレッジベースに決定事項を登録（RAG用のembedding作成）
4. 決定事項抽出プロンプトの作成（構造化データ出力）
5. 検索インターフェースの構築（自然言語での質問に対応）
6. 関連決定事項の自動リンク機能の実装
7. 実施状況追跡ワークフローの構築
8. ダッシュボードの作成（未実施決定事項のアラート等）
9. 運用開始とモニタリング

## Sample Prompts

```
以下の議事録から、すべての決定事項を抽出し、構造化データとして出力してください。

【出力フォーマット】
{
  "decisions": [
    {
      "decision_id": "自動採番",
      "decision_summary": "決定内容の要約（50文字以内）",
      "decision_details": "決定内容の詳細",
      "decision_maker": "決定者（役職・氏名）",
      "decision_date": "決定日（YYYY-MM-DD）",
      "category": "カテゴリー（予算/人事/戦略/業務プロセス等）",
      "impact_level": "影響度（高/中/低）",
      "implementation_deadline": "実施期限",
      "responsible_person": "実施責任者",
      "related_decisions": "関連する過去の決定事項ID"
    }
  ]
}

議事録:
{meeting_minutes}
```

```
「{キーワード}」に関連する過去の決定事項をすべて検索してください。

以下の情報を含めて回答してください：
- 決定日と会議名
- 決定内容の要約
- 決定者
- 実施状況（完了/進行中/未着手）
- 関連する後続の決定事項

検索結果は時系列順に並べてください。
```

```
この新しい提案について、過去の決定事項と矛盾や重複がないか確認してください。

【提案内容】
{proposal_details}

【確認事項】
1. 類似の提案が過去に承認または却下されていないか
2. 既存の方針やポリシーと矛盾していないか
3. 関連する過去の決定事項で参考になるものはあるか

過去の決定事項との関連性を分析し、推奨事項を提示してください。
```

## Data Requirements

- データの種類:
  - 過去の会議議事録（経営会議、部門会議、プロジェクト会議等）
  - 決裁文書、稟議書
  - ポリシー・規程変更記録
  - プロジェクト承認記録
- データ量: 過去2-3年分の議事録（500-1,000件）、主要な決定事項1,000-2,000件
- データ形式: Word、PDF、テキスト形式の議事録、構造化データ（JSON、CSV）
- データ準備:
  1. 過去の議事録をデジタル化（紙ベースの場合はスキャン・OCR）
  2. 決定事項の初期抽出（手動またはAI支援）
  3. カテゴリー・タグ体系の設計
  4. 重要度の評価基準の策定
  5. 実施状況の初期登録

## Technical Considerations

- システム要件:
  - LLM API（月間500-1,000リクエスト）
  - ベクトルDB（10,000-20,000エントリー）
  - 決定事項DB（リレーショナルまたはNoSQL）
- セキュリティ:
  - 機密決定事項のアクセス制御（役職・部門別権限）
  - 監査ログの記録（誰がいつ何を検索・参照したか）
  - 改ざん防止（決定事項の変更履歴を記録）
- パフォーマンス: 検索クエリに対して3秒以内に結果を返す
- 制限事項:
  - 非公式な口頭での決定は記録されない（議事録化が前提）
  - 決定事項の曖昧な表現は人間が明確化する必要あり
  - 実施状況の更新は手動または他システムとの連携が必要
- スケーラビリティ: 年間2,000件の新規決定事項追加に対応

## Reference Links

- https://www.dify.ai/blog/knowledge-management
- https://docs.anthropic.com/claude/docs/decision-tracking
- https://www.atlassian.com/software/confluence/guides/expand-confluence/decision-log

## Tags

- 意思決定
- トラッキング
- ナレッジマネジメント
- 会議管理
- コンプライアンス
- RAG
- 検索システム
- ガバナンス

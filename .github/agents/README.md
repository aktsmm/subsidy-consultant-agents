# 補助金コンサルティング エージェントシステム

補助金コンサルティング業務を支援するエージェントシステムです。

> **情報更新日**: 2026年2月8日

---

## 概要

中小企業向け補助金コンサルティング業務を効率化するためのエージェント群です。  
Orchestrator-Workers パターンで構成され、`subsidy-consultant` が司令塔として専門エージェントに作業を委譲します。

### 主な機能

- **対象者診断**: 業種・規模から補助金対象可否を判定
- **補助金選定**: 導入内容に応じた最適な補助金を提案
- **申請ガイド**: 申請フロー・必要書類・スケジュールを案内
- **最新情報取得**: 公式サイトから公募要領の更新を確認
- **回答記録・品質管理**: 回答の自動記録と品質チェック

---

## ディレクトリ構造

```
.github/
├── agents/                              # エージェント定義
│   ├── subsidy-consultant.agent.md     # オーケストレーター
│   ├── eligibility-checker.agent.md    # 対象者診断
│   ├── subsidy-selector.agent.md       # 補助金選定
│   ├── application-guide.agent.md      # 申請ガイド
│   ├── web-researcher.agent.md         # Webリサーチ
│   ├── schedule-tracker.agent.md       # スケジュール管理
│   ├── answer-recorder.agent.md        # 回答記録
│   └── answer-reviewer.agent.md        # 回答品質チェック
│
├── prompts/                             # プロンプトテンプレート
│   ├── customer-intake.prompt.md       # 顧客ヒアリング
│   ├── generate-proposal.prompt.md     # 提案書生成
│   ├── check-subsidy-updates.prompt.md # 更新チェック
│   ├── save-answer.prompt.md           # 回答保存
│   ├── review-session-export-md.prompt.md # セッションレビュー
│   ├── update-knowledge-base.prompt.md # ナレッジベース更新
│   └── create-customer-pptx.prompt.md  # 顧客向けPPTX作成
│
├── instructions/                        # ナレッジベース（instructions形式）
│   └── subsidy/
│       ├── _overview.instructions.md
│       ├── it-hojo/
│       ├── other-subsidies/
│       └── consulting/
│
└── assets/                              # テンプレート
    └── templates/
        ├── hearing-sheet.md
        ├── proposal-template.md
        └── documents-checklist.md
```

---

## エージェント一覧

| エージェント          | 役割                             | 呼び出し方             |
| --------------------- | -------------------------------- | ---------------------- |
| `subsidy-consultant`  | オーケストレーター（司令塔）     | `@subsidy-consultant`  |
| `eligibility-checker` | 対象者診断                       | `@eligibility-checker` |
| `subsidy-selector`    | 補助金選定                       | `@subsidy-selector`    |
| `application-guide`   | 申請ガイド                       | `@application-guide`   |
| `web-researcher`      | Webリサーチ                      | `@web-researcher`      |
| `schedule-tracker`    | スケジュール管理                 | `@schedule-tracker`    |
| `answer-recorder`     | 回答を Markdown で自動記録       | `@answer-recorder`     |
| `answer-reviewer`     | 記録済み回答の品質チェック・承認 | `@answer-reviewer`     |

### ワークフロー図

```
                    ┌─────────────────────┐
                    │ subsidy-consultant  │
                    │   (Orchestrator)    │
                    └──────────┬──────────┘
                               │ runSubagent
       ┌───────────┬───────────┼───────────┬───────────┬───────────┐
       ▼           ▼           ▼           ▼           ▼           ▼
┌─────────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
│ eligibility │ │ subsidy │ │  app    │ │   web   │ │schedule │ │ answer  │
│   checker   │ │ selector│ │  guide  │ │researcher│ │ tracker │ │recorder │
│ (対象者診断) │ │(補助金選定)│ │(申請ガイド)│ │(リサーチ) │ │(スケジュール)│ │ (記録)  │
└─────────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └────┬────┘
                                                                     │ handoff
                                                                     ▼
                                                              ┌─────────────┐
                                                              │   answer    │
                                                              │  reviewer   │
                                                              │ (品質チェック) │
                                                              └─────────────┘
```

---

## 使用例

### 1. 顧客の対象者診断

```
@eligibility-checker 製造業で従業員50人、資本金5000万円の企業は補助金対象になりますか？
```

### 2. 補助金の選定

```
@subsidy-selector 会計ソフトとPCを導入したいです。どの補助金がいいですか？
```

### 3. 申請フローの確認

```
@application-guide IT導入補助金のインボイス枠の申請フローを教えてください
```

### 4. 最新情報の確認

```
@web-researcher IT導入補助金の最新スケジュールを確認してください
```

### 5. 総合相談（オーケストレーター経由）

```
@subsidy-consultant 製造業の顧客が在庫管理システムを導入したいと言っています。補助金活用の提案をしてください。
```

### 6. 回答の記録と品質チェック

```
@answer-recorder 上記の回答を記録してください
@answer-reviewer Answer/2026-02/ の回答を品質チェックしてください
```

---

## プロンプト一覧

| プロンプト                 | 用途                                         |
| -------------------------- | -------------------------------------------- |
| `customer-intake`          | 新規顧客の初回ヒアリングを実施               |
| `generate-proposal`        | 顧客向け補助金活用提案書を生成               |
| `check-subsidy-updates`    | 補助金の公募要領・スケジュール更新をチェック |
| `save-answer`              | 回答の保存                                   |
| `review-session-export-md` | セッションエクスポートのレビュー             |
| `update-knowledge-base`    | ナレッジベースの最新情報更新                 |
| `create-customer-pptx`     | 顧客向けPPTX資料の作成                       |

---

## ナレッジベース

### 人間可読ドキュメント（`docs/`）

- [ナレッジベース索引](../../docs/README.md)
- [エージェント活用ガイド](../../docs/エージェント活用ガイド.md)

### instructions 形式（`.github/instructions/subsidy/`）

エージェントが直接参照する構造化ナレッジ：

| パス                        | 内容                                             |
| --------------------------- | ------------------------------------------------ |
| `_overview.instructions.md` | 全体概要                                         |
| `it-hojo/`                  | IT導入補助金                                     |
| `other-subsidies/`          | 他補助金（ものづくり、持続化、省力化、事業承継） |
| `consulting/`               | コンサル実務（対応フロー、書類チェック、FAQ）    |

---

## 関連データ

| リソース         | パス                        | 説明                       |
| ---------------- | --------------------------- | -------------------------- |
| 締切データベース | `data/deadlines.yaml`       | 全補助金の締切を一元管理   |
| 回答アーカイブ   | `Answer/`                   | 回答の自動記録・品質管理   |
| 顧客管理         | `clients/`                  | 顧客プロファイル・案件管理 |
| テンプレート     | `.github/assets/templates/` | ヒアリングシート・提案書   |

---

## 注意事項

1. **情報の鮮度**: ナレッジベースは2026年1月28日時点の情報です。最新情報は `@web-researcher` で確認してください。
2. **公式情報の確認**: 申請前に必ず公式サイトで最新の公募要領を確認してください。
3. **個別判断**: 複雑なケースは専門家（認定経営革新等支援機関等）に相談してください。

---

## 情報ソース

- [IT導入補助金](https://it-shien.smrj.go.jp/)
- [ものづくり補助金](https://portal.monodukuri-hojo.jp/)
- [小規模事業者持続化補助金](https://r6.jizokukahojokin.info/)
- [省力化投資補助金](https://shoryokuka.smrj.go.jp/)
- [ミラサポplus](https://mirasapo-plus.go.jp/)

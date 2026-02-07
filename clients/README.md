# 顧客・案件管理

このフォルダでは、補助金コンサルティングのクライアント情報と案件進捗を管理します。

## フォルダ構造

```
clients/
├── README.md              # このファイル
├── _template/            # 新規クライアント用テンプレート
│   ├── profile.md        # 顧客プロファイルテンプレート
│   └── consultation.md   # 相談記録テンプレート
│
└── {会社名または顧客ID}/
    ├── profile.md        # 顧客基本情報
    ├── consultations/    # 相談履歴
    │   └── YYYYMMDD-{トピック}.md
    └── applications/     # 申請案件
        └── {補助金名}-{年度}/
            ├── status.md
            ├── documents/
            └── timeline.md
```

## 使い方

### 1. 新規クライアント登録

```bash
# テンプレートをコピー
cp -r _template/ "株式会社ABC"

# profile.md を編集して基本情報を入力
```

### 2. 相談記録の追加

```bash
# consultations/ に日付形式でファイル作成
touch "株式会社ABC/consultations/20260208-IT導入補助金相談.md"
```

### 3. 申請案件の追加

```bash
# applications/ に補助金・年度別フォルダ作成
mkdir -p "株式会社ABC/applications/IT導入補助金-2026"
```

## エージェント連携

| エージェント         | 役割                                 |
| -------------------- | ------------------------------------ |
| `subsidy-consultant` | 相談内容を基に案件フォルダを自動生成 |
| `answer-recorder`    | 回答を consultations/ に自動記録     |
| `schedule-tracker`   | applications/ の締切を監視・アラート |

## 命名規則

### 会社フォルダ名

- 正式社名または顧客ID
- 例: `株式会社ABC`、`C001-山田商店`

### 相談記録ファイル名

- 形式: `YYYYMMDD-{トピック}.md`
- 例: `20260208-IT導入補助金対象診断.md`

### 申請案件フォルダ名

- 形式: `{補助金略称}-{年度}`
- 例: `IT導入補助金-2026`、`ものづくり-2026`

## 注意事項

- 個人情報・機密情報の取り扱いに注意
- 必要に応じて `.gitignore` に追加
- 定期的にバックアップを取得

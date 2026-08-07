# AI-Development-Team

Claude Cowork に読み込ませて自動稼働させるための「AIチーム運用リポジトリ」です。
Cowork はタスク開始時に必ずこのリポジトリを取得し、共通ルールと担当部門の役割定義に従って作業します。

## 運用サイクル

- **毎日**: Research 部門が情報収集を行い、結果を `03_Workspace/daily_research/` に日付ファイルとして保存する
- **毎週金曜**: Report 部門が1週間分の収集結果を集約し、週次レポートを `03_Workspace/weekly_reports/` に作成して報告する

## フォルダ構成

| フォルダ | 内容 |
|---|---|
| `00_Common_Rules/` | 全部門共通のルールと開発フロー。**すべてのタスクで最初に読むこと** |
| `01_Departments/` | 10部門それぞれの役割定義（`role.md`） |
| `02_Templates/` | 成果物のテンプレート。該当する成果物は必ずテンプレートに従う |
| `03_Workspace/` | 作業成果物の保存先（日次リサーチ、週次レポート、プロジェクト別成果物） |

## 部門一覧

| No | 部門 | 役割の概要 |
|---|---|---|
| 01 | Orchestrator | 全体統括。タスクを分解し、適切な部門に割り当てる |
| 02 | PM | プロジェクト管理。進捗・スコープ・優先順位の管理 |
| 03 | Research | 情報収集・市場調査（市場規模・競合・SWOT分析） |
| 04 | Architect | システム設計（要件定義・DB・API・UI・権限・ディレクトリ設計） |
| 05 | Business | 経営企画（価格・採算・ROI・Go/NoGo判定） |
| 06 | Report | 報告書作成。週次レポートの取りまとめ |
| 07 | Developer | Claude Code（実装専門） |
| 08 | QA | 品質保証・レビュー・テスト |
| 09 | Education | ユーザー専用の教育係（学習レポート・ロードマップ） |
| 10 | AI_Auditor | 成果物の最終監査。ルール遵守・正確性・リスクの確認 |

## Cowork でのタスク実行手順（Claude への指示）

1. このリポジトリの最新状態を取得する
2. `00_Common_Rules/common_rules.md` と `development_flow.md` を読む
3. タスク内容から担当部門を判断し、該当する `01_Departments/*/role.md` を読む
4. 成果物がある場合は `02_Templates/` の該当テンプレートに従って作成する
5. 成果物を `03_Workspace/` の適切な場所に保存し、変更をコミット・プッシュする
6. AI_Auditor の観点で自己チェックしてから、共通ルールの「出力ルール（9項目形式）」で完了報告する

## Cowork スケジュールタスク登録用プロンプト

### ① 毎日の情報収集タスク（毎日 朝に実行）

```
GitHubリポジトリ https://github.com/Yuito-9939/new-project-plan-team を取得し、
00_Common_Rules/ の共通ルール（稼働時間9:00〜16:00と引き継ぎルールを含む）と
01_Departments/03_Research/role.md を読んでください。
まず 03_Workspace/daily_research/progress.md（引き継ぎメモ）を読んで前日の続きを確認し、
role.md の手順に従って本日分の情報収集を実施してください。
16:00を過ぎた場合は作業を切り上げ、途中でも必ず保存してください。
結果を 02_Templates/research_template.md の形式で
03_Workspace/daily_research/YYYY-MM-DD.md として保存し、
progress.md を更新した上でリポジトリにコミット・プッシュしてください。
その週の調査・評価が出尽くしたと判断した場合は、金曜を待たず
早期報告ルール（共通ルール9）に従って週次レポートを作成してください。
```

### ② 週次レポートタスク（毎週金曜 夕方に実行）

```
GitHubリポジトリ https://github.com/Yuito-9939/new-project-plan-team を取得し、
00_Common_Rules/ の共通ルールと 01_Departments/06_Report/role.md を読んでください。
まず 03_Workspace/weekly_reports/ に当該週の早期報告がないか確認してください。
早期報告がある場合はその後の差分のみを追記し、ない場合は
03_Workspace/daily_research/ にある直近1週間分（月〜金）のファイルを読み込み、
【開発候補】タグの付いた案件を 06_Report/role.md の手順に従って評価した上で、
02_Templates/report_template.md の形式で週次レポートを作成して
03_Workspace/weekly_reports/YYYY-MM-DD_weekly.md として保存し、
リポジトリにコミット・プッシュしてください。
併せて、レポートの要約を完了報告として提示してください。
```

> **注意**: Cowork から GitHub へプッシュするには、Cowork 側で GitHub の認証（連携またはアクセストークン）が設定されている必要があります。読み取りのみの場合は、成果物をローカルフォルダに保存する運用に変更しても構いません。

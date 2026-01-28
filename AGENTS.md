<!-- skill-ninja-START -->

## Installed Skills

The following skills are available in this workspace.

| Skill                                                                    | When to Use                                                                                                                                                          |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [agentic-workflow-guide](.github/skills/agentic-workflow-guide/SKILL.md) | Use this skill when creating, reviewing, or updating agents and workflows:                                                                                           |
| [powerpoint-automation](.github/skills/powerpoint-automation/SKILL.md)   | AI-powered PPTX generation pipeline using Orchestrator-Workers pattern. Automatically extracts content, translates, applies templates, and performs quality reviews. |
| [skill-creator](.github/skills/skill-creator/SKILL.md)                   | This skill provides guidance for creating effective skills.                                                                                                          |
| [xlsx](.github/skills/xlsx/SKILL.md)                                     | Process Excel spreadsheets (.xlsx)                                                                                                                                   |

<!-- skill-ninja-END -->

---

## Subsidy Consulting Agents

補助金コンサルティング業務向けのエージェント一覧です。

| Agent                                                              | Role         | Description                                                              |
| ------------------------------------------------------------------ | ------------ | ------------------------------------------------------------------------ |
| [subsidy-consultant](.github/agents/subsidy-consultant.agent.md)   | Orchestrator | 補助金コンサルティングの司令塔。顧客対応を統括し、専門エージェントに委譲 |
| [eligibility-checker](.github/agents/eligibility-checker.agent.md) | Worker       | 補助金の対象者要件を診断。業種・規模から対象可否を判定                   |
| [subsidy-selector](.github/agents/subsidy-selector.agent.md)       | Worker       | 顧客のニーズに最適な補助金を選定・比較                                   |
| [application-guide](.github/agents/application-guide.agent.md)     | Worker       | 補助金申請の具体的なフローと必要書類を案内                               |
| [web-researcher](.github/agents/web-researcher.agent.md)           | Worker       | 補助金の最新情報をWeb検索で調査                                          |
| [schedule-tracker](.github/agents/schedule-tracker.agent.md)       | Worker       | 補助金の締切・スケジュールを管理                                         |
| [answer-recorder](.github/agents/answer-recorder.agent.md)         | Worker       | 回答をMarkdown形式で /Answer に自動記録                                  |
| [answer-reviewer](.github/agents/answer-reviewer.agent.md)         | Worker       | 記録された回答の品質チェック・自動承認                                   |

### Workflow Pattern

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
└─────────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └────┬────┘
                                                                     │ handoff
                                                                     ▼
                                                              ┌─────────────┐
                                                              │   answer    │
                                                              │  reviewer   │
                                                              └─────────────┘
```

### Usage

```
# オーケストレーターに相談（自動で適切なWorkerに委譲）
@subsidy-consultant 補助金について相談したい

# 直接Workerを呼び出し
@eligibility-checker 製造業・従業員50人で対象になりますか？
@subsidy-selector 会計ソフト導入に使える補助金は？
```

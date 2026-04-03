# Lark Private PM

> A Claude Code skill that turns Claude into your personal PM assistant, powered by Feishu/Lark Project MCP.

飞书项目私人PM助手 — 基于 Claude Code Skill + 飞书项目 MCP，让 Claude 成为你的个人项目经理。

## Features

| Feature | Description | Trigger |
|---------|-------------|---------|
| **Daily To-Do** | View today's tasks, overdue items, and actionable suggestions | `今日待办` `daily` `早上好` |
| **Weekly Summary** | Review completed work, in-progress items, and next-week outlook | `本周总结` `周报` `weekly summary` |
| **New Task Alert** | Check for recently assigned or created tasks | `有没有新任务` `新分配的任务` |
| **Smart Planning** | AI-powered daily/weekly work prioritization (P0-P3) | `帮我规划` `排个期` `work plan` |

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI or Desktop App
- Feishu Project MCP server connected (via `claude mcp add` or settings)
- A Feishu Project workspace with tasks assigned to you

## Installation

### 1. Add the Feishu Project MCP Server

```bash
claude mcp add feishu-project \
  --transport streamable-http \
  --url https://project.feishu.cn/api/mcp/v1/mcp/stream \
  --header "X-USER-KEY: <your-user-key>" \
  --header "X-PLUGIN-TOKEN: <your-plugin-token>"
```

> Get your `X-USER-KEY` and `X-PLUGIN-TOKEN` from the [Feishu Project MCP integration settings](https://project.feishu.cn/).

### 2. Install the Skill

Copy `SKILL.md` to your Claude Code skills directory:

```bash
# Create the skill directory
mkdir -p ~/.claude/skills/lark-private-pm

# Copy the skill file
cp SKILL.md ~/.claude/skills/lark-private-pm/SKILL.md
```

Or clone this repo directly:

```bash
git clone https://github.com/Eamonnn101/Lark-Private-PM.git
mkdir -p ~/.claude/skills/lark-private-pm
cp Lark-Private-PM/SKILL.md ~/.claude/skills/lark-private-pm/SKILL.md
```

### 3. Verify

Open Claude Code and type:

```
/mcp
```

You should see `feishu-project` connected. Then try:

```
今日待办
```

## Usage Examples

```
# Morning routine
今天有什么任务

# Friday review
帮我做本周总结

# Check for new assignments
有没有新任务

# Plan your day
帮我规划今天的工作

# Plan your week
这周怎么安排
```

## Sample Output

### Daily To-Do

```
## ☀️ 早安！今日工作概览 (2026-04-03)

### 🔴 已逾期 (0)
无逾期任务，很棒！

### 📋 进行中的待办 (2)
| 工作项 | 状态 | 排期 | 所属空间 |
|--------|------|------|----------|
| BOSS战BGM正式制作 | 未完成 | 03/24 ~ 04/13 | WorldX |
| 宁静之岛正式音乐制作 | 未完成 | 04/06 ~ 04/10 | WorldX |

### 💡 今日建议
- BOSS战BGM 已进入排期中段，建议持续推进
- 宁静之岛 04/06开始，可提前做前期准备
```

### Smart Planning

```
## 📅 今日工作规划 (2026-04-03)

### 🟢 本周推进 (P2)
1. BOSS战BGM — 04/13到期，剩余10天
   - 需求负责人: 秦迪 | 验收人: 秦迪、汪岩嵩
2. 宁静之岛音乐 — 04/06开始，3天准备期
   - 需求负责人: 张博文 | 验收人: 张博文

### ⚠️ 负荷评估
P0+P1 共 0 项，当前待办共 2 项，工作量适中。
```

## MQL Query Tips

When using `search_by_mql`, field names **must be in Chinese**:

| Field | Correct | Wrong |
|-------|---------|-------|
| Created time | `` `创建时间` `` | `` `created_at` `` |
| Priority | `` `优先级` `` | `` `priority` `` |
| Status | `` `状态` `` | `` `status` `` |
| Name | `` `name` `` | (this one works) |

## Automation (Optional)

You can set up scheduled triggers to run this skill automatically:

```bash
# Daily morning reminder at 8:00 AM
claude schedule create \
  --name "daily-todo" \
  --cron "0 8 * * 1-5" \
  --prompt "今日待办"

# Weekly summary on Friday at 5:00 PM
claude schedule create \
  --name "weekly-summary" \
  --cron "0 17 * * 5" \
  --prompt "本周总结"
```

## Architecture

```
Claude Code
    ├── Skill: lark-private-pm (SKILL.md)
    │   ├── Feature 1: Daily To-Do
    │   ├── Feature 2: Weekly Summary
    │   ├── Feature 3: New Task Alert
    │   └── Feature 4: Smart Planning
    │
    └── MCP: feishu-project (Streamable HTTP)
        ├── list_todo
        ├── search_by_mql
        ├── get_workitem_brief
        └── ... (50+ tools)
```

## License

MIT

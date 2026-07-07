---
name: agent-daily-installer
description: 安装并配置 agent-daily-auto 日报系统。克隆仓库 → 配 .env → 运行日报。当用户说「跑日报」「装日报」「部署日报」时使用。
tags: [daily, news, automation, pipeline]
keywords: [日报, daily, news, agent, pipeline]
---

# Agent Daily Installer

帮助你的 Hermes Agent 安装并运行 **agent-daily-auto**——一套自动抓取 AI 新闻、生成文章、部署到 Vercel 的日报系统。

## 这是什么

这个 skill 让你只需要对你的 Agent 说一句「跑日报」：

```
Agent 会自动完成：
  → git pull 最新代码
  → python3 fetch_news.py 抓取数据
  → AI 生成文章
  → python3 scripts/run_daily_update.py 编排
  → git push 部署到 Vercel
```

每天一条命令，日报自动上线。

## 代码仓库

https://github.com/AmaosAIGC/agent-daily-auto

## 演示站

https://www.agthub.tech/daily —— 每天在跑的活案例，从 2026 年 6 月至今未断过。

## 前置条件

```bash
python3 --version           # ≥ 3.10
node --version               # 任意版本
git --version
```

## 安装步骤

### Step 1: 克隆仓库

```bash
cd ~/projects
git clone https://github.com/AmaosAIGC/agent-daily-auto.git
cd agent-daily-auto
```

### Step 2: 配置 .env

```bash
cp .env.example .env
# 编辑 .env，至少设置 PROJECT_DIR
```

最小配置：
```env
PROJECT_DIR=/home/xxx/agent-daily-auto
```

### Step 3: 填充日报数据

```bash
cp daily-data.js.example daily-data.js
# 然后用 AI 生成首批文章，或手动编写
```

## 日常使用

当用户说「跑日报」时，按以下流程执行：

### 1️⃣ 抓取数据

```bash
cd /path/to/agent-daily-auto
python3 fetch_news.py > /tmp/news_data.json
```

### 2️⃣ 检查是否需要去重

```bash
python3 scripts/dedup_check.py --list
python3 scripts/dedup_check.py
```

### 3️⃣ AI 生成 18+ 篇文章

根据 `/tmp/news_data.json` 的真实数据，用 AI 撰写今日文章。

### 4️⃣ 编排 + 部署

```bash
export PROJECT_DIR=$(pwd)
python3 scripts/run_daily_update.py
```

脚本通过验证后，执行其打印的 git 命令。

## 注意事项

- 数据源偶尔报错（arXiv 等）属于正常波动，跳过不影响。
- JS 语法检查不过时先跑 `fix_body_quotes.py`。
- 第一个部署需要关联 Vercel 项目。

## 演示视频

（coming soon）

## License

MIT

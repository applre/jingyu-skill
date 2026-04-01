# jingyu-skill

Claude Code Skills 合集 — 产品经理 & 内容创作者的效率工具箱。

## Skills 列表

| Skill | 说明 | 触发方式 |
|-------|------|----------|
| [website-archiver](./website-archiver/) | 网站内容归档 | "帮我抓取这个网站"、"归档竞品官网" |
| [code-chinese-comments](./code-chinese-comments/) | 代码中文注释 | "给这段代码加中文注释"、"帮我注释一下" |
| [topic-collector](./topic-collector/) | AI 热点日报采集 | "开始今日选题"、"采集热点"、"今日AI热点" |

---

## website-archiver — 网站内容归档

**用途**：输入一个 URL，自动抓取网站所有页面，输出结构化 Markdown 文档 + 全页截图。适合竞品调研、产品分析。

**使用方式**：
```
帮我把 https://example.com 的网站内容整理一下
抓取 https://competitor.com 所有页面，整理成文档
```

**输出示例**：
```
competitor-2026-03-29/
├── 00-网站结构总览.md
├── 01-首页.md
├── 02-定价.md
├── 03-功能介绍.md
└── screenshots/
    ├── 01-首页.png
    └── 02-定价.png
```

**依赖**：Chrome + Remote Debugging（截图功能需要，非必须）

---

## code-chinese-comments — 代码中文注释

**用途**：为代码逐行添加中文注释，标注变量类型和含义。帮助非工程师（产品经理、运营）快速理解开源项目代码逻辑。

**使用方式**：
```
帮我给这个文件加上中文注释
注释一下这段代码，方便学习
```

**效果**：在代码关键位置添加中文注释，解释变量用途、函数逻辑、数据流向，不改变原始代码行为。

---

## topic-collector — AI 热点日报采集

**用途**：从 Product Hunt、Hacker News、技术博客等来源并行采集 AI 相关热点，整理成格式化日报写入 Apple Notes，同时在对话中输出 Markdown 版本。

**使用方式**：
```
开始今日选题
采集热点
看看今天有什么新闻
今日AI热点
```

**聚焦领域**：
1. Vibe Coding / Claude Code / Cursor 实践技巧
2. MCP Server / AI Agent 工作流
3. 模型厂商动态（Anthropic、OpenAI、Google、xAI）
4. AI 新产品（Product Hunt 上榜）
5. 社区热议（HN、Reddit）

**输出**：
- Apple Notes 中创建 `AI热点 MM/DD` 笔记（HTML 格式，含可点击链接）
- 对话中同步输出 Markdown 版本

**依赖**：WebSearch 工具、macOS（Apple Notes 写入需要 osascript）

---

## 安装

将 skill 目录复制到 `~/.claude/skills/` 下即可：

```bash
# 克隆仓库
git clone https://github.com/applre/jingyu-skill.git

# 复制需要的 skill
cp -r jingyu-skill/topic-collector ~/.claude/skills/
cp -r jingyu-skill/website-archiver ~/.claude/skills/
cp -r jingyu-skill/code-chinese-comments ~/.claude/skills/
```

## License

MIT

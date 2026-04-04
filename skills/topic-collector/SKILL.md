---
name: topic-collector
description: AI热点采集工具，采集后自动写入 Apple Notes。从 Product Hunt、Hacker News、博客等采集AI相关热点内容，整理成格式化日报存入 Notes。当用户说"开始今日选题"、"采集热点"、"看看今天有什么新闻"、"今日AI热点"、"每天早晨寻找全网热门资讯"时触发。聚焦领域：Vibe Coding、Claude Code、MCP、AI Agent、AI模型更新、AI新产品、海外热点。
---

# Topic Collector - AI热点采集

## 执行流程

1. **并行搜索**：同时执行 4 条 WebSearch
2. **整理内容**：去重、分类、提取要点和链接
3. **写入 Apple Notes**：用 AppleScript 写入 HTML 格式笔记

## 搜索关键词（并行执行）

```
1. Anthropic Claude OpenAI Gemini update {当月英文} {年份}
2. Claude Code MCP AI agent tips latest {当月英文} {年份}
3. Product Hunt AI tools trending today
4. Hacker News AI hot discussion {当月英文} {年份}
```

## 聚焦领域（优先级）

1. Vibe Coding / Claude Code / Cursor 实践技巧
2. MCP Server / AI Agent 工作流
3. 模型厂商动态（Anthropic、OpenAI、Google、xAI）
4. AI 新产品（Product Hunt 上榜）
5. 社区热议（HN、Reddit）

## 内容要求

- 每条必须有可点击链接
- 每类 2-3 条，总计不超过 10 条
- 优先 24 小时内内容
- 不采集纯学术论文（除非引发广泛讨论）

## 写入 Apple Notes

整理完内容后，用 AppleScript 写入 HTML 格式笔记。

**笔记标题格式**：`AI热点 MM/DD`（如已存在则先删除旧的）

**HTML 模板**：

```applescript
tell application "Notes"
    -- 删除同名旧笔记
    set oldNotes to every note whose name is "AI热点 MM/DD"
    repeat with n in oldNotes
        delete n
    end repeat

    set noteBody to "<h1>🗞 今日AI热点 MM/DD</h1>

<h2>🏢 模型厂商动态</h2>
<p><b>1. 标题</b><br>
要点：一句话<br>
来源：<a href='URL'>来源名</a></p>

<h2>🧑‍💻 Claude Code / Vibe Coding</h2>
<p><b>2. 标题</b><br>
要点：一句话<br>
来源：<a href='URL'>来源名</a></p>

<h2>⚙️ AI 新产品</h2>
<p><b>3. 产品名</b> — 一句话描述<br>
来源：<a href='URL'>Product Hunt</a></p>

<h2>💬 社区热议</h2>
<p><b>4. 标题</b><br>
要点：一句话<br>
来源：<a href='URL'>来源名</a></p>

<p>─────────────────<br>
🔥 今日关键信号：信号1 · 信号2 · 信号3</p>"

    make new note at folder "Notes" with properties {name:"AI热点 MM/DD", body:noteBody}
end tell
```

**注意**：
- 用 `osascript << 'EOF' ... EOF` 执行 AppleScript
- noteBody 中的单引号用 `'` 即可（heredoc 内无需转义）
- 写入成功后告知用户笔记标题

## 同步输出

写入 Notes 的同时，在对话中也输出相同内容（Markdown 格式），方便用户直接阅读，无需打开 Notes。

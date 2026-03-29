# jingyu-skill

给产品经理做竞品调研 / 产品分析用的 Claude Code Skills 合集。

## 使用场景

产品经理在做竞品分析、市场调研时，经常需要系统性地了解一个产品的官网内容——页面结构、功能介绍、定价方案、品牌定位等。手动逐页浏览和截图既耗时又容易遗漏。

这套 Skills 让你只需提供一个 URL，就能自动完成网站内容的完整归档，输出结构化的 Markdown 文档 + 全页截图，可直接用于竞品报告、内部分享或存入 Obsidian 等知识库。

## Skills 列表

| Skill | 说明 |
|-------|------|
| [website-archiver](./website-archiver/) | 网站内容归档：自动抓取网站所有页面，输出编号的 Markdown 文档 + 全页截图 |
| [code-chinese-comments](./code-chinese-comments/) | 代码中文注释：为每行代码添加中文学习注释，标注变量类型，帮助初学者理解代码 |

## 安装

将 skill 目录复制到 `~/.claude/skills/` 下即可：

```bash
# 克隆仓库
git clone https://github.com/applre/jingyu-skill.git

# 复制需要的 skill
cp -r jingyu-skill/website-archiver ~/.claude/skills/
```

## 使用示例

在 Claude Code 中直接说：

```
帮我把 https://example.com 的网站内容整理一下
```

或者更具体地：

```
抓取 https://competitor.com 所有页面，整理成文档放到 Obsidian Vault 里
```

### 输出示例

```
competitor-2026-03-29/
├── 00-网站结构总览.md
├── 01-首页.md
├── 02-定价.md
├── 03-功能介绍.md
├── 04-关于我们.md
└── screenshots/
    ├── 01-首页.png
    ├── 02-定价.png
    ├── 03-功能介绍.png
    └── 04-关于我们.png
```

## 依赖

- [Claude Code](https://claude.com/claude-code)
- Chrome + Remote Debugging（截图功能需要，非必须）

## License

MIT

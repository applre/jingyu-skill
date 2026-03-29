# jingyu-skill

给产品经理做竞品调研 / 产品分析用的 Claude Code Skills 合集。

## 使用场景

产品经理日常工作中经常需要：
- **竞品调研**：系统性了解竞品官网的页面结构、功能介绍、定价方案、品牌定位等
- **理解开源项目**：阅读开源代码了解技术实现，但代码对非工程师来说门槛较高

这套 Skills 帮你：
- 提供一个 URL，自动完成网站内容的完整归档，输出结构化的 Markdown 文档 + 全页截图
- 为开源代码逐行添加中文注释，标注变量类型和含义，不需要工程背景也能看懂代码逻辑

## Skills 列表

| Skill | 说明 |
|-------|------|
| [website-archiver](./website-archiver/) | 网站内容归档：自动抓取网站所有页面，输出编号的 Markdown 文档 + 全页截图 |
| [code-chinese-comments](./code-chinese-comments/) | 代码中文注释：为代码逐行添加中文注释，标注变量类型和含义，帮助产品经理快速理解开源项目代码 |

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

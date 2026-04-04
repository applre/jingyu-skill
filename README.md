# jingyu-skill

经过实测评测的 Claude Code Skills 精选集。每个 skill 附带评测报告、测试用例和实际输出。

## 评测总览

### 通用

| Category | Skill | 介绍 | 来源 | 评分 | 评测报告 |
|----------|-------|------|------|------|----------|
| info | [topic-collector](skills/topic-collector/) | AI 热点日报采集，从 PH/HN/博客等并行采集 | 自研 | 待评测 | [报告](reviews/topic-collector.md) |

### 职能

| Category | Skill | 介绍 | 来源 | 评分 | 评测报告 |
|----------|-------|------|------|------|----------|
| PM | [website-archiver](skills/website-archiver/) | 网站内容归档，输出 Markdown + 全页截图 | 自研 | 待评测 | [报告](reviews/website-archiver.md) |
| PM | [code-chinese-comments](skills/code-chinese-comments/) | 为代码添加中文学习注释，帮 PM 看懂开源项目 | 自研 | 待评测 | [报告](reviews/code-chinese-comments.md) |

## 安装

将 skill 目录复制到 `~/.claude/skills/` 下即可：

```bash
# 克隆仓库
git clone https://github.com/applre/jingyu-skill.git

# 复制需要的 skill
cp -r jingyu-skill/skills/website-archiver ~/.claude/skills/
```

## 仓库结构

```
jingyu-skill/
├── skills/          # skill 源码（可直接 cp 安装）
├── reviews/         # 评测报告（结论 + 多维度分析）
├── tests/           # 测试用例 + 实际输出（原始证据）
└── comparisons/     # 同类 skill 横向比较
```

## 评测标准

每个 skill 的评测包含：

- **多场景测试** — 基础场景、边界场景、复杂场景
- **实际输出记录** — prompt、完整日志、生成的产物
- **适用/不适用场景** — 明确告诉你什么时候该用、什么时候别用
- **横向比较** — 同类 skill 之间的对比

## 依赖

- [Claude Code](https://claude.ai/code)
- Chrome + Remote Debugging（部分 skill 需要）

## License

MIT

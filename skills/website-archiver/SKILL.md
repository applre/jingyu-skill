***

name: website-archiver
description: 系统化抓取网站的所有页面内容和结构，整理成编号的 markdown 文档+全页截图，统一放在一个带日期的文件夹下。当用户想要归档一个网站、抓取官网内容、整理某个产品/公司的网站资料、做竞品分析需要完整记录网站内容时使用。即使用户只说"帮我看看这个网站"或"把这个网站内容整理一下"，也应该触发此 skill。
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Site Archiver — 网站内容归档

将一个网站的所有页面系统化抓取，输出为结构化的 markdown 文档 + 全页截图，统一存放在带日期的文件夹中。

## 工作流程

### Phase 1: 发现网站结构

用 Jina + WebFetch 抓取首页，提取完整的网站地图：

```
WebFetch url="https://r.jina.ai/{domain}"
prompt="Extract the complete website structure: all navigation links, page sections, feature descriptions, and any other content. List every URL/link found on the page. Return the full content in markdown format."
```

从结果中提取：

* 主导航菜单及子项

* 页脚链接

* 所有可访问的子页面 URL

* 核心内容（tagline、功能描述、价值主张等）

Jina 的调用方式是在域名前加 `r.jina.ai/`，不保留原 URL 的 http/https 前缀。限 20 RPM。

### Phase 2: 并行抓取所有子页面

根据 Phase 1 发现的页面列表，用 WebFetch + Jina 并行抓取所有子页面。每批 4-5 个并行请求，避免触发限流。

对每个页面的 prompt 根据页面类型调整：

* 功能页："Extract all feature descriptions, capabilities, use cases. Return complete content."

* 定价页："Extract all pricing plans, features included in each plan, prices. Return complete content."

* 关于页："Extract all content about the company. Return complete content."

* 通用页："Extract all content. Return complete."

如果某个 URL 返回 404，记录下来但不中断流程，在总览文档中标注。

### Phase 3: 整理为编号的 markdown 文档

创建输出文件夹：`{目标路径}/{网站名}-{YYYY-MM-DD}/`

文件编号规则：

* `00-网站结构总览.md` — 网站地图、导航结构、文件索引（必须首先创建）

* `01-首页.md`

* `02-xx.md` ~ `0N-xx.md` — 按网站逻辑分组（功能、场景、公司、价格等）

* 内容较多的分类（如多个 use case）合并到一个文件中，用二级标题区分

每个文档的头部格式：

```markdown
# 页面标题

> 来源：https://完整URL

---
```

总览文档包含：

* 完整网站地图（树形结构）

* 导航结构（表格）

* 文件索引（文件名 → 内容说明的映射表）

* 404 页面标注

### Phase 4: 截图（可选）

截图依赖 Chrome DevTools MCP 工具（`mcp__chrome_devtools__*`）。

**启动前检查**：尝试调用 `mcp__chrome_devtools__list_pages`。如果工具不可用或连接失败，告知用户：

> "截图功能需要 Chrome DevTools MCP 连接。当前无法连接，文档已整理完成（不含截图）。如需截图，请确保 Chrome 已开启 Remote Debugging 后告诉我继续。"

然后跳过截图，完成文档交付。

**截图流程**（MCP 可用时）：

1. 创建 `screenshots/` 子文件夹
2. 用 `mcp__chrome_devtools__new_page` 打开页面（`background=true`, `timeout=30000`）
3. **截图前先滚动触发懒加载**——很多网站的图片、视频、动画使用懒加载，不滚动就不会渲染，全页截图会出现大片空白。用 `mcp__chrome_devtools__evaluate_script` 执行滚动脚本：

   ```javascript
   async () => {
     const delay = ms => new Promise(r => setTimeout(r, ms));
     const step = window.innerHeight;
     const maxScroll = document.body.scrollHeight;
     for (let y = 0; y < maxScroll; y += step) {
       window.scrollTo(0, y);
       await delay(300);
     }
     window.scrollTo(0, maxScroll);
     await delay(1000);
     window.scrollTo(0, 0);
     await delay(500);
     return { scrollHeight: document.body.scrollHeight, done: true };
   }
   ```
4. 用 `mcp__chrome_devtools__take_screenshot` 截取全页（`fullPage=true`）
5. 截图文件名与文档编号对应：`01-首页.png`、`02-xx.png`
6. 在对应文档头部插入截图引用：`![描述](screenshots/xx.png)`
7. 如果截图超时，在文档中标注"该页截图超时未能获取"，不中断流程
8. 完成后用 `mcp__chrome_devtools__close_page` 关闭所有自己创建的 tab

可以批量打开页面（每批 4-5 个），然后依次切换（需 `bringToFront=true`）、滚动、截图，提高效率。

### Phase 5: 交付

向用户展示最终文件结构和统计：

* 文件数量、截图数量

* 树形目录结构

* 如有 404 页面，列出

如果用户提到 Obsidian 或想存入 vault，将整个文件夹移动到 Obsidian Vault 的合适位置。

## 输出语言

* 默认跟随用户的语言偏好

* 文档用目标语言整理，关键英文原文以引用块保留

* 文件名使用中文或英文，与文档内容语言一致

## 注意事项

* Jina 适合文章、文档等以正文为核心的页面；对 SPA、需要 JS 渲染的动态内容可能提取不完整

* 如果 Jina 返回的内容明显不完整（如只有 404 或空内容），尝试不带 Jina 前缀直接 WebFetch

* 博客列表页可能需要动态加载，静态抓取可能只拿到部分文章，在文档中注明

* 截图是全页截图，长页面的 PNG 文件可能较大（数 MB）

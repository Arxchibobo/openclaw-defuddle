---
name: defuddle
description: "从网页中提取干净的 Markdown 内容。替代 web_fetch 用于标准网页 — 去除导航、广告、杂乱元素，减少 token 消耗。支持元数据提取（作者、发布日期、schema.org）。"
---

# Defuddle — 网页内容清洁提取

将任意网页转为干净的 Markdown，去除导航栏、广告、侧边栏等杂乱内容。

> **何时使用**: 需要从网页提取正文内容时，优先于 `web_fetch`。特别适合文章、博客、文档页面。
> **何时不用**: 需要交互（登录、点击）的页面用 browser 工具；纯 API 数据用 web_fetch。

## 安装位置

```
~/.npm-global/bin/defuddle
```

确保 PATH 包含: `export PATH=~/.npm-global/bin:$PATH`

## 核心命令

```bash
# 解析 URL → Markdown
defuddle parse https://example.com/article --md

# 解析 URL → JSON（含元数据）
defuddle parse https://example.com/article --json

# 解析本地 HTML 文件
defuddle parse page.html --md

# 提取特定字段
defuddle parse https://example.com --property title
defuddle parse https://example.com --property author

# 保存到文件
defuddle parse https://example.com/article --md --output result.md
```

## 输出格式

| 参数 | 说明 |
|------|------|
| `--md` | Markdown（默认推荐）|
| `--json` | JSON 含元数据 + 正文 |
| `--property <name>` | 提取单个字段（title/author/published/description）|

## JSON 输出字段

- `title` — 文章标题
- `author` — 作者
- `published` — 发布日期
- `description` — 摘要
- `content` — 清洁后的正文
- `domain` / `site` — 来源网站
- `wordCount` — 字数
- `schemaOrgData` — 结构化数据

## 与 web_fetch 的对比

| 特性 | defuddle | web_fetch |
|------|----------|-----------|
| 去杂质 | ✅ 强（导航/广告/侧边栏）| ⚠️ 基础 |
| 元数据 | ✅ 丰富 | ❌ 无 |
| 脚注/数学/代码块 | ✅ 标准化 | ❌ 原样 |
| Token 消耗 | 🟢 低 | 🔴 高 |
| 需要安装 | ✅ 是 | ❌ 内置 |

## 决策规则

1. 标准网页文章/博客/文档 → **用 defuddle**
2. API 端点 / JSON 数据 → 用 web_fetch
3. 需要登录/交互 → 用 browser 工具
4. defuddle 失败 → 降级到 web_fetch

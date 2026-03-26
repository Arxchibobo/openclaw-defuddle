# openclaw-defuddle

An [OpenClaw](https://github.com/Arxchibobo/openclaw) skill that extracts clean Markdown content from web pages using [defuddle](https://github.com/kepano/defuddle) — a smarter alternative to `web_fetch` that strips navigation, ads, and sidebars to reduce token consumption.

## Overview

When Claude needs to read a web page, raw HTML or generic fetch results often include a large amount of irrelevant content (menus, ads, footers, cookie banners). This skill wraps the `defuddle` CLI to strip all of that away and return only the main content as clean Markdown, along with structured metadata such as author, publication date, and schema.org data.

**Use this skill when:** fetching articles, blog posts, or documentation pages.
**Skip this skill when:** the page requires login/interaction (use browser tools) or you need raw API/JSON data (use `web_fetch`).

## Features

- Converts any public web page to clean, readable Markdown
- Removes navigation bars, ads, sidebars, and other noise
- Extracts rich metadata: title, author, published date, description, word count, schema.org data
- Significantly reduces token consumption compared to raw `web_fetch`
- Supports local HTML file parsing
- Supports JSON output with full metadata alongside content
- Falls back gracefully to `web_fetch` if defuddle fails

## Requirements

- Node.js with `defuddle` CLI installed globally:

```bash
npm install -g defuddle
```

Ensure the install path is on your `PATH`:

```bash
export PATH=~/.npm-global/bin:$PATH
```

## Installation

Copy or symlink `SKILL.md` into your OpenClaw skills directory, then follow the defuddle CLI installation step above.

## Usage

The skill instructs Claude to use the `defuddle` CLI as follows:

```bash
# Extract main content as Markdown
defuddle parse https://example.com/article --md

# Extract content + metadata as JSON
defuddle parse https://example.com/article --json

# Parse a local HTML file
defuddle parse page.html --md

# Extract a specific metadata field
defuddle parse https://example.com --property title
defuddle parse https://example.com --property author

# Save output to a file
defuddle parse https://example.com/article --md --output result.md
```

## Output Formats

| Flag | Description |
|------|-------------|
| `--md` | Clean Markdown (recommended) |
| `--json` | JSON with metadata and content body |
| `--property <name>` | Single field: `title`, `author`, `published`, `description` |

### JSON Fields

| Field | Description |
|-------|-------------|
| `title` | Article title |
| `author` | Author name |
| `published` | Publication date |
| `description` | Summary/excerpt |
| `content` | Clean body content |
| `domain` / `site` | Source website |
| `wordCount` | Word count |
| `schemaOrgData` | Structured data |

## Comparison with `web_fetch`

| Feature | defuddle | web_fetch |
|---------|----------|-----------|
| Noise removal (nav/ads/sidebar) | Strong | Basic |
| Metadata extraction | Rich | None |
| Footnotes / math / code blocks | Normalized | Raw |
| Token consumption | Low | High |
| Requires installation | Yes | No (built-in) |

## Decision Guide

1. Standard article / blog / documentation page → **use defuddle**
2. API endpoint / JSON data → use `web_fetch`
3. Page requires login or interaction → use browser tools
4. defuddle fails → fall back to `web_fetch`

## Author

[Arxchibobo (Bobo Zhou)](https://github.com/Arxchibobo)

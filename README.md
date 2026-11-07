# dsh-free-light-search

**轻量纯 AI 搜索插件，面向 DeepSeek Harness (dsh)，零成本起步，多引擎回退。** 插件注册为 `ctx.web` seam 的搜索 provider，内置 `web_search` 工具自动选用，支持引擎切换、回退链、失败冷却与诊断。

[中文](#中文) · [English](#english)

---

## 中文

### 设计理念

与抓取型搜索插件不同，本插件专注于**纯 AI 搜索 API**：调用搜索引擎的结构化 API，返回标题、链接和摘要，不做网页抓取、正文提取或扩展搜索。轻量、快速、低成本。

### 核心特点

- **纯 AI 搜索**：调用搜索引擎 API，返回结构化结果，不做抓取或正文提取
- **免费优先**：所有引擎都有免费额度，无 API key 也能用（匿名额度）
- **自动回退**：任何引擎失败自动切换下一个，搜索永不中断
- **失败冷却**：按失败类别冷却引擎，避免反复撞墙
- **网页设置页**：引擎切换、API key 配置、一键测试、引擎开关

### 引擎列表

本插件只保留有免费额度的 AI 搜索引擎，按额度模型分层：

| 档位 | 引擎 | 说明 |
|---|---|---|
| **① 无限免费** | `ddg` / `ddg-lite` / `bing` / `searxng` / `anysearch` | HTML 抓取型搜索，无限免费 |
| **② 匿名额度** | `exa` / `tavily` / `firecrawl` / `you` | 无需 key 即可使用，配 key 提升额度 |
| **③ 免费额度** | `baidu` / `jina` | 需 key，有免费额度 |

#### 引擎详情

| ID | 引擎 | 免费额度 | 说明 |
|---|---|---|---|
| `ddg` | DuckDuckGo HTML | 无限 | 偶发限流，自动恢复 |
| `ddg-lite` | DuckDuckGo Lite | 无限 | 轻量版 |
| `bing` | Bing | 无限 | 免 key，中文优化 |
| `searxng` | SearXNG 元搜索 | 无限 | 多实例自动切换 |
| `anysearch` | AnySearch AI | 无限 | AI 搜索，匿名额度 |
| `exa` | Exa | 1000 次/月 | AI 搜索，质量高 |
| `tavily` | Tavily | 1000 次/月 | 专为 AI agent 设计 |
| `firecrawl` | Firecrawl | 500 页面/月 | 网页抓取 + 搜索 |
| `you` | You.com | 1000 次/月 | You.com AI 搜索 |
| `baidu` | 百度 AI 搜索 | 每日 50 次 | 中文全网 |
| `jina` | Jina AI | 有免费额度 | 搜索 + 摘要 |

### 安装

```sh
git clone https://github.com/z-imagine/dsh-free-light-search.git
dsh plugin --profile web add /path/to/dsh-free-light-search
dsh web
```

### 使用

#### 网页设置

安装后打开配置页：左侧 **插件** 页 → **已安装** 分组 → `free-search` → 配置入口。

配置页提供：
- **Search engine**：下拉框切换引擎
- **API keys**：为付费引擎填写 key（Exa / Tavily / Firecrawl / You.com / SerpBase / Serply / Baidu / Jina）
- **Test engine**：测试当前引擎可用性
- **引擎开关**：启用/禁用引擎，调整回退优先级

#### 聊天框切换引擎

输入 `/free-search-engine`，弹出引擎选择窗口，点选即切换。

### 回退链

默认回退顺序：`exa` → `tavily` → `firecrawl` → `serpbase` → `serply` → `you` → `baidu` → `jina` → `bing` → `anysearch` → `ddg` → `ddg-lite` → `searxng`

任何引擎失败（限流、反爬、缺 key、网络错误）自动尝试下一个。

### 失败分类与冷却

- **`quota`**（额度耗尽）：冷却到会话结束
- **`auth`**（key 无效）：冷却到会话结束
- **`bot-wall`**（反爬）：冷却 60s
- **`transient`**（超时/5xx/限流）：重试一次再回退
- **`invalid-response`**（解析失败）：不冷却，直接回退

### 配置文件示例

```yaml
- id: web
  config:
    searchProvider: ddg
    fetchProvider: fs-quality-fetch
```

---

## English

### Design Philosophy

Unlike scraping-based search plugins, this plugin focuses on **pure AI search APIs**: calling structured search APIs to return titles, links, and snippets, without web scraping, content extraction, or extended search. Lightweight, fast, and low-cost.

### Key Features

- **Pure AI Search**: Call search engine APIs, return structured results, no scraping
- **Free First**: All engines have free tiers, usable without API keys (anonymous tiers)
- **Auto Fallback**: Automatically switch to next engine on failure, search never stops
- **Failure Cooling**: Cool down engines by failure category, avoid repeated failures
- **Web Settings**: Engine switching, API key configuration, one-click testing

### Engine List

Only AI search engines with free tiers are included, categorized by quota model:

| Tier | Engines | Description |
|---|---|---|
| **① Unlimited Free** | `ddg` / `ddg-lite` / `bing` / `searxng` / `anysearch` | HTML scraping search, unlimited free |
| **② Anonymous Quota** | `exa` / `tavily` / `firecrawl` / `you` | No key required, key increases quota |
| **③ Free Quota** | `baidu` / `jina` | Key required, has free quota |

#### Engine Details

| ID | Engine | Free Tier | Description |
|---|---|---|---|
| `ddg` | DuckDuckGo HTML | Unlimited | Occasional rate limiting |
| `ddg-lite` | DuckDuckGo Lite | Unlimited | Lightweight version |
| `bing` | Bing | Unlimited | No key, Chinese optimized |
| `searxng` | SearXNG Meta Search | Unlimited | Multi-instance auto-switch |
| `anysearch` | AnySearch AI | Unlimited | AI search, anonymous quota |
| `exa` | Exa | 1000 queries/month | High-quality AI search |
| `tavily` | Tavily | 1000 queries/month | Designed for AI agents |
| `firecrawl` | Firecrawl | 500 pages/month | Web scraping + search |
| `you` | You.com | 1000 queries/month | You.com AI search |
| `baidu` | Baidu AI Search | 50 queries/day | Chinese web search |
| `jina` | Jina AI | Free tier available | Search + summarization |

### Installation

```sh
git clone https://github.com/z-imagine/dsh-free-light-search.git
dsh plugin --profile web add /path/to/dsh-free-light-search
dsh web
```

### Usage

#### Web Settings

Open the config page: Left sidebar **Plugins** → **Installed** → `free-search` → Config entry.

The config page provides:
- **Search engine**: Dropdown to switch engines
- **API keys**: Enter keys for paid engines
- **Test engine**: Test current engine availability
- **Engine switches**: Enable/disable engines, adjust fallback priority

#### Chat Command Switching

Type `/free-search-engine` to open the engine selection popup.

### Fallback Chain

Default fallback order: `exa` → `tavily` → `firecrawl` → `serpbase` → `serply` → `you` → `baidu` → `jina` → `bing` → `anysearch` → `ddg` → `ddg-lite` → `searxng`

Any engine failure (rate limit, bot wall, missing key, network error) automatically tries the next engine.

### Failure Classification & Cooling

- **`quota`** (quota exhausted): Cool down until session ends
- **`auth`** (invalid key): Cool down until session ends
- **`bot-wall`** (anti-bot): Cool down 60s
- **`transient`** (timeout/5xx/rate limit): Retry once then fallback
- **`invalid-response`** (parse failure): No cooldown, direct fallback

### Config File Example

```yaml
- id: web
  config:
    searchProvider: ddg
    fetchProvider: fs-quality-fetch
```

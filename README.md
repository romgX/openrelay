<p align="center">
  <img src="logo.png" width="120" alt="OpenRelay">
</p>

<h1 align="center">OpenRelay</h1>

<p align="center"><b>No more quota waste — your AI subscriptions, everywhere.</b></p>

<p align="center">
  <a href="https://github.com/romgX/openrelay/releases/latest"><img src="https://img.shields.io/github/v/release/romgX/openrelay?label=Download&color=brightgreen" alt="Download"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue" alt="MIT License"></a>
  <a href="https://t.me/openrelay_updates"><img src="https://img.shields.io/badge/Telegram-Updates-2CA5E0?logo=telegram" alt="Telegram"></a>
</p>

<p align="center"><a href="#english">English</a> | <a href="#中文">中文</a></p>

---

<a name="english"></a>

## The Problem

How do you find more free AI model quotas?
How do you connect free or paid quotas to the tools you're already using?
How do you configure Claude Code, OpenCode, or any AI tool in one click?

**OpenRelay solves all of this.**

## What OpenRelay Actually Does

### 1. Auto-discover all your AI quotas

Launch OpenRelay and it instantly finds every AI subscription and free quota on your machine — Claude Desktop, Claude Code, Kiro, Windsurf, Antigravity, OpenCode, VS Code Copilot. All quotas at your command, zero manual configuration.

Plus 22 direct API providers (Groq, Gemini, DeepSeek, Mistral, OpenRouter, etc.) — enter an API key once and it's available everywhere.

**29 providers. One dashboard. One endpoint.**

### 2. Use any quota in any tool

Your Claude Pro subscription can now power Claude Code, Aider, Continue, Goose, Amp, or any tool that speaks Anthropic/OpenAI API:

```bash
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
# That's it. Claude Code now uses your Claude Desktop quota.
```

Want to use Kiro's free Claude Sonnet quota in Aider? Just change the URL:
```bash
export ANTHROPIC_BASE_URL=http://localhost:18765/kiro
```

### 3. One-click setup for every CLI tool

No more editing `.zshrc` or juggling environment variables. Open the Web dashboard, pick a provider for each tool, flip a switch:

- **Claude Code** → route through Kiro (free Claude Sonnet)
- **Aider** → route through Groq (free, blazing fast)
- **Goose** → route through Gemini API (free 1M context)
- **OpenCode** → route through DeepSeek (cheapest coding model)

Reopen your terminal. Done. Every tool is configured.

### 4. Supercharge your IDE with external quotas

Cursor quota burned through? Windsurf credits gone? Don't stop coding — seamlessly plug in any other quota source:

| IDE | How it works | What you get |
|-----|-------------|--------------|
| **Cursor** | RPC proxy (ConnectRPC, HTTP/2) | Use Claude/Kiro/Groq/any provider inside Cursor |
| **Windsurf** | RPC proxy (ConnectRPC) | Replace Windsurf's built-in models |
| **VS Code Copilot** | Ollama BYOK bridge | Use any model as a Copilot backend |
| **Antigravity** | Gemini REST proxy | Route through any provider |

Start the proxy from the dashboard. Your IDE doesn't know the difference.

### 5. Combine quotas into unstoppable model groups

This is the killer feature. Take quotas from multiple providers and merge them into a single virtual model:

```
"fast-group" = Groq (Llama 90B) + Cerebras (Llama 70B) + SambaNova (Llama 405B)
```

When Groq's free quota runs out → automatic failover to Cerebras → then SambaNova. **Your AI never stops.** Round-robin across providers ensures maximum uptime with zero manual switching.

---

## Install

### Binary (recommended, no Node.js needed)

Download from [GitHub Releases](https://github.com/romgX/openrelay/releases):

```bash
# macOS
curl -L -o openrelay https://github.com/romgX/openrelay/releases/latest/download/openrelay-macos-arm64
chmod +x openrelay
xattr -d com.apple.quarantine openrelay   # first time only
./openrelay
```

```powershell
# Windows (download from Releases page, then run)
Invoke-WebRequest -Uri "https://github.com/romgX/openrelay/releases/latest/download/openrelay-windows-x64.exe" -OutFile openrelay.exe
.\openrelay.exe
```

### npm (requires Node.js >= 18)

```bash
npm install -g openrelay
openrelay
```

## Quick Start

```bash
openrelay                # Start the proxy
openrelay --setup        # First time on macOS: authorize Keychain
openrelay --test         # Test all provider connections
```

Open `http://localhost:18765` → everything is managed from the Web dashboard.

## Security

**Credentials never leave your machine.** All tokens, cookies, and API keys stay in local process memory. Nothing is uploaded anywhere.

**Direct connections only.** Requests go straight from your machine to the AI provider. No relay server in between.

**No request logging.** Message content is never logged, cached, or persisted.

**Auditable.** The credential handling code ([cookie.ts](src/cookie.ts)) is open source for security review.

See [DISCLAIMER.md](DISCLAIMER.md) and [PRIVACY.md](PRIVACY.md) for details.

## License

Open Core model:
- **Framework** (proxy, format translation, config): [MIT](LICENSE)
- **Pro features** (custom model groups, priority support): [Commercial](COMMERCIAL-LICENSE.txt)

---

<a name="中文"></a>

<p align="right"><a href="#english">English</a> | <b>中文</b></p>

## 中文说明

**不再为配额烦恼，让你的 AI 订阅，随处可用。**

<p align="center">
  <a href="https://t.me/openrelay_updates">📢 加入 Telegram 频道获取更新 →</a>
</p>

---

## 痛点
怎么样找到更多的免费AI模型配额？
怎么样把免费或收费的配额接到你正在使用的工具中？
怎么样能一键配置你的claude code 、openclaw AI模型？

**OpenRelay 一键解决。**

## OpenRelay 能做什么

### 1. 自动发现你所有的 AI 配额

启动 OpenRelay，它立刻找到你机器上每一份 AI 订阅和免费配额 — Claude Desktop、Claude Code、Kiro、Windsurf、Antigravity、OpenCode、VS Code Copilot。所有配额任你调度，无需手动配置。

支持 22 个直连 API（Groq、Gemini、DeepSeek、Mistral、OpenRouter 等）— 输入一次 API Key，到处可用。

**29 个提供商。一个面板。一个端点。**

### 2. 任意配额用在任意工具

你的 Claude Pro 订阅现在可以驱动 Claude Code、Aider、Continue、Goose、Amp，或任何支持 Anthropic/OpenAI API 的工具：

```bash
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
# 搞定。Claude Code 现在使用你的 Claude Desktop 配额。
```

想在 Aider 里用 Kiro 的免费 Claude Sonnet？改一下 URL：
```bash
export ANTHROPIC_BASE_URL=http://localhost:18765/kiro
```

### 3. 一键配置所有 CLI 工具

不再手动编辑 `.zshrc`，不再来回倒腾环境变量。打开 Web 面板，选择 Provider，点一下开关：

- **Claude Code** → 走 Kiro（免费 Claude Sonnet）
- **Aider** → 走 Groq（免费，极速推理）
- **Goose** → 走 Gemini API（免费，100 万上下文）
- **OpenCode** → 走 DeepSeek（最便宜的编程模型）

重开终端，完事。每个工具都配好了。

### 4. 给你的 IDE 无缝接入外部配额

Cursor 配额烧完了？Windsurf 额度用光了？别停下编码 — 无缝插入任何其他配额来源：

| IDE | 接入方式 | 效果 |
|-----|---------|------|
| **Cursor** | RPC 代理 (ConnectRPC, HTTP/2) | 在 Cursor 里用 Claude/Kiro/Groq/任意 Provider |
| **Windsurf** | RPC 代理 (ConnectRPC) | 用任意 Provider 替换 Windsurf 内置模型 |
| **VS Code Copilot** | Ollama BYOK 桥接 | 用任意模型作为 Copilot 后端 |
| **Antigravity** | Gemini REST 代理 | 通过任意 Provider 路由 |

在面板启动代理，IDE 无感切换。

### 5. 组合配额，AI 永不停机

把多个 Provider 的配额合并成一个虚拟模型：

```
"fast-group" = Groq (Llama 90B) + Cerebras (Llama 70B) + SambaNova (Llama 405B)
```

Groq 免费额度用完 → 自动切到 Cerebras → 再切 SambaNova。**你的 AI 永不停机。** 跨 Provider 轮询确保最大可用时间，零手动切换。

### 安装

```bash
# macOS — 下载二进制（无需 Node.js）
curl -L -o openrelay https://github.com/romgX/openrelay/releases/latest/download/openrelay-macos-arm64
chmod +x openrelay
xattr -d com.apple.quarantine openrelay   # 仅首次需要
./openrelay
```

```powershell
# Windows
Invoke-WebRequest -Uri "https://github.com/romgX/openrelay/releases/latest/download/openrelay-windows-x64.exe" -OutFile openrelay.exe
.\openrelay.exe
```

浏览器打开 `http://localhost:18765` — 一切在 Web 面板中管理，支持中英双语。

### 安全

**凭据不离开本机** — 所有 token、cookie、API key 仅在本地内存中使用，不会上传到任何服务器。

**直连 API** — 请求从你的机器直接发送到 AI 后端，没有中转服务器。

**可审计** — 凭据处理代码（[cookie.ts](src/cookie.ts)）开源可审。

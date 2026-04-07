<p align="center">
  <img src="logo.png" width="120" alt="OpenRelay">
</p>

<h1 align="center">OpenRelay</h1>

<p align="center"><b>几百个免费 AI 模型配额，一键接入本地项目</b></p>

<p align="center">
  <a href="https://github.com/romgX/openrelay/releases/latest"><img src="https://img.shields.io/github/v/release/romgX/openrelay?color=blue&label=download" alt="Latest Release"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Open%20Core-blue" alt="License"></a>
  <img src="https://img.shields.io/badge/platform-macOS%20%7C%20Windows%20%7C%20Linux-lightgrey" alt="Platform">
  <a href="https://t.me/openrelay_updates"><img src="https://img.shields.io/badge/Telegram-updates-blue?logo=telegram" alt="Telegram"></a>
  <a href="https://t.me/openrelay_chat"><img src="https://img.shields.io/badge/Telegram-chat-blue?logo=telegram" alt="Chat"></a>
</p>

<p align="center"><a href="#中文">中文</a> | <a href="#english">English</a></p>

---

<a name="中文"></a>

## 痛点

**你的 AI 订阅，各自为政。**

Claude Pro 只能在 Claude Desktop 用。Kiro 配额只能在 Kiro 用。Groq 免费但每个工具都要手动配置。Cursor 500 次用完了，你只能停下来。

**OpenRelay 打破这道墙。**

- 帮你找到更多免费 AI 模型配额（Groq、Cerebras、SambaNova、Gemini——全免费、全自动发现）
- 帮你把免费或收费的配额接入你正在使用的 AI 工具
- 一键配置 Claude Code、OpenClaw、Aider、Goose 等所有工具的模型

## 演示

![OpenRelay 演示](demo.gif)

<table>
  <tr>
    <td><img src="screenshot-providers.png" alt="Provider 面板" width="400"><br><sub>自动发现的 Provider 和配额状态</sub></td>
    <td><img src="screenshot-work.png" alt="Work — CLI 工具配置" width="400"><br><sub>一键配置 Claude Code、OpenClaw、Aider...</sub></td>
  </tr>
  <tr>
    <td><img src="screenshot-ide.png" alt="IDE RPC 代理" width="400"><br><sub>IDE 代理——Cursor、Windsurf、VS Code Copilot</sub></td>
    <td><img src="screenshot-custom.png" alt="自定义模型组" width="400"><br><sub>自定义模型组，自动故障转移</sub></td>
  </tr>
</table>

---

## OpenRelay 能做什么

### 1. 自动发现你所有的 AI 配额

启动 OpenRelay，它立刻找到你机器上每一份 AI 订阅和免费配额 — Claude Desktop、Claude Code、Kiro、Windsurf、Antigravity、OpenCode、VS Code Copilot、OpenAI Codex。所有配额任你调度，无需手动配置。

支持 24 个直连 API（Groq、Gemini、DeepSeek、Anthropic、Cloudflare AI、HuggingFace 等）— 输入一次 API Key，到处可用。

**32 个提供商。一个面板。一个端点。**

### 2. 任意配额用在任意工具

你的 Claude Pro 订阅现在可以驱动 Claude Code、Aider、Continue、Goose、Amp，或任何支持 Anthropic/OpenAI API 的工具：

```powershell
# Windows (PowerShell)
$env:ANTHROPIC_BASE_URL="http://localhost:18765"
$env:ANTHROPIC_API_KEY="unused"
```

```bash
# macOS / Linux
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
```

搞定。Claude Code 现在使用你的 Claude Desktop 配额。

想在 Aider 里用 Kiro 的免费 Claude Sonnet？改一下 URL：
```
ANTHROPIC_BASE_URL=http://localhost:18765/kiro
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

---

## 安装 & 快速上手

> **直接下载可执行文件运行，无需 npm install / node 环境。**

**Windows**：[点击下载 openrelay-windows-x64.exe](https://github.com/romgX/openrelay/releases/latest/download/openrelay-windows-x64.exe)，双击运行。

**macOS**（Intel / Apple Silicon 通用）：[点击下载 openrelay-macos](https://github.com/romgX/openrelay/releases/latest/download/openrelay-macos)，然后终端执行：

```bash
chmod +x openrelay-macos
xattr -d com.apple.quarantine openrelay-macos
./openrelay-macos
```

> `xattr` 命令用于解除 macOS 对未签名程序的安全限制，否则会提示"无法打开"。

**Linux** (x64)：[点击下载 openrelay-linux-x64](https://github.com/romgX/openrelay/releases/latest/download/openrelay-linux-x64)，然后终端执行：

```bash
chmod +x openrelay-linux-x64
./openrelay-linux-x64
```

> Linux 支持的 IDE Provider：Claude Code、Kiro、Windsurf、OpenCode、VS Code Copilot、OpenAI Codex。Claude Desktop 和 Antigravity 目前无 Linux 版本。凭证存取通过 `secret-tool`（gnome-keyring）或文件缓存，无需额外配置。

浏览器打开 `http://localhost:18765` — 一切在 Web 面板中管理，支持中英双语。

> 觉得有用？给个 **Star** ⭐ 是对我们最大的支持！

---

## 安全

**凭据不离开本机** — 所有 token、cookie、API key 仅在本地内存中使用，不会上传到任何服务器。

**直连 AI 后端** — 请求从你的机器直接发送到 AI Provider，没有中转服务器。

**不记录请求内容** — 消息内容从不被日志记录、缓存或持久化。

**可审计** — 凭据处理代码（[cookie.ts](src/cookie.ts)）可查看审计。

详见 [DISCLAIMER.md](DISCLAIMER.md) 和 [PRIVACY.md](PRIVACY.md)。

## 常见问题

遇到问题？查看 **[常见问题 (中文)](faq.md)** | **[FAQ (English)](faq-en.md)**。

## 社区

- LINUX DO：[linux.do](https://linux.do)
- Telegram 讨论群：[t.me/openrelay_chat](https://t.me/openrelay_chat)
- Telegram 更新频道：[t.me/openrelay_updates](https://t.me/openrelay_updates)
- 问题反馈：[GitHub Issues](https://github.com/romgX/openrelay/issues)

点个 **Star** ⭐ + 加入 [Telegram 群](https://t.me/openrelay_chat)，有机会领 **1 个月 Pro 体验码**。

## 许可证

Open Core 模式：
- **框架部分**（代理、格式转换、配置）：[MIT](LICENSE)
- **Pro 功能**（模型组合、无限请求）：[商业授权](COMMERCIAL-LICENSE.txt)

---

<a name="english"></a>

<p align="right"><b>中文</b> | <a href="#english">English</a></p>

## The Problem

**Your AI subscriptions are locked in silos.**

Claude Pro only works in Claude Desktop. Kiro quota only works in Kiro. Groq is free but you have to configure every tool manually. Cursor burned through your 500 requests and now you're stuck.

**OpenRelay breaks the silos.**

- Find more free AI quota (Groq, Cerebras, SambaNova, Gemini — all free, all auto-discovered)
- Connect any quota to any tool you're already using
- One-click configure Claude Code, OpenClaw, Aider, Goose, and more

## Demo

![OpenRelay Demo](demo.gif)

<table>
  <tr>
    <td><img src="screenshot-providers.png" alt="Provider Dashboard" width="400"><br><sub>Auto-discovered providers & quota status</sub></td>
    <td><img src="screenshot-work.png" alt="Work — CLI tool config" width="400"><br><sub>One-click configure Claude Code, OpenClaw, Aider...</sub></td>
  </tr>
  <tr>
    <td><img src="screenshot-ide.png" alt="IDE RPC Proxies" width="400"><br><sub>IDE proxies — Cursor, Windsurf, VS Code Copilot</sub></td>
    <td><img src="screenshot-custom.png" alt="Custom Model Groups" width="400"><br><sub>Custom model groups with auto-failover</sub></td>
  </tr>
</table>

---

## What OpenRelay Actually Does

### 1. Auto-discover all your AI quotas

Launch OpenRelay and it instantly finds every AI subscription and free quota on your machine — Claude Desktop, Claude Code, Kiro, Windsurf, Antigravity, OpenCode, VS Code Copilot, OpenAI Codex. All quotas at your command, zero manual configuration.

Plus 24 direct API providers (Groq, Gemini, DeepSeek, Anthropic, Cloudflare AI, HuggingFace, etc.) — enter an API key once and it's available everywhere.

**32 providers. One dashboard. One endpoint.**

### 2. Use any quota in any tool

Your Claude Pro subscription can now power Claude Code, Aider, Continue, Goose, Amp, or any tool that speaks Anthropic/OpenAI API:

```powershell
# Windows (PowerShell)
$env:ANTHROPIC_BASE_URL="http://localhost:18765"
$env:ANTHROPIC_API_KEY="unused"
```

```bash
# macOS / Linux
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
```

That's it. Claude Code now uses your Claude Desktop quota.

Want to use Kiro's free Claude Sonnet quota in Aider? Just change the URL:
```
ANTHROPIC_BASE_URL=http://localhost:18765/kiro
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

## Supported Providers (32 total)

### IDE Providers (8) — auto-extracted, no API key needed

| Provider | Free Quota | Notes |
|----------|-----------|-------|
| **Claude Desktop** | Your subscription | Claude Pro/Max |
| **Claude Code** | Your subscription | Claude Pro/Max |
| **Kiro** (AWS) | 50 credits/month + 500 new user | Free Claude Sonnet |
| **Windsurf** (Codeium) | Unlimited autocomplete + 25 credits/month | |
| **Antigravity** | Included with IDE | Requires IDE running |
| **OpenCode** | Unlimited | Built-in GLM-4.7 |
| **VS Code Copilot** | Your subscription | GitHub Copilot |
| **OpenAI Codex** | Limited time free (GPT-5.4) | REST + WebSocket dual transport |

### Direct API Providers (24) — bring your own key

| Provider | Free Tier |
|----------|-----------|
| **Groq** | 30 RPM, up to 14,400 req/day |
| **Cerebras** | 1M tokens/day |
| **SambaNova** | 200K tokens/day |
| **Gemini** | 1M context, generous free tier |
| **OpenRouter** | 20 RPM, 50 req/day, 30+ models |
| **DeepSeek** | 5M tokens signup credit |
| **Mistral** | 1B tokens/month |
| **xAI (Grok)** | $25 signup + $150/month |
| **Together AI** | $100 signup credit |
| **Fireworks** | $1 signup credit |
| **SiliconFlow** | 20M tokens + free models |
| **Zhipu (GLM)** | GLM-4-Flash permanently free |
| **DashScope** (Alibaba) | ¥450 credit, 200+ models |
| **Volcengine** (ByteDance) | ¥100 credit, 2M tokens/day |
| **Moonshot** (Kimi) | 1.5M tokens/day |
| **NVIDIA NIM** | ~1000 credits |
| **GitHub Models** | 50 req/day (GPT-4o) |
| **Baichuan** | ¥80 signup credit |
| **Stepfun** | Trial credits |
| **MiniMax** | Signup credits |
| **Hunyuan** (Tencent) | Hunyuan-Lite free |
| **Anthropic** | Pay-as-you-go (native format passthrough) |
| **Cloudflare AI** | 10,000 Neurons/day, no credit card |
| **HuggingFace** | Rate-limited serverless, thousands of models |

---

## Install & Quick Start

> **Download and run — no npm install or Node.js required.**

**Windows**: [Download openrelay-windows-x64.exe](https://github.com/romgX/openrelay/releases/latest/download/openrelay-windows-x64.exe), double-click to run.

**macOS** (Intel / Apple Silicon universal): [Download openrelay-macos](https://github.com/romgX/openrelay/releases/latest/download/openrelay-macos), then run in terminal:

```bash
chmod +x openrelay-macos
xattr -d com.apple.quarantine openrelay-macos
./openrelay-macos
```

> `xattr` removes macOS Gatekeeper quarantine flag — without it, macOS will block the unsigned binary.

**Linux** (x64): [Download openrelay-linux-x64](https://github.com/romgX/openrelay/releases/latest/download/openrelay-linux-x64), then run in terminal:

```bash
chmod +x openrelay-linux-x64
./openrelay-linux-x64
```

> Supported IDE providers on Linux: Claude Code, Kiro, Windsurf, OpenCode, VS Code Copilot, OpenAI Codex. Claude Desktop and Antigravity have no Linux builds. Credentials are stored via `secret-tool` (gnome-keyring) or file-based cache — no extra setup needed.

Open `http://localhost:18765` → everything is managed from the Web dashboard.

> Found it useful? Give us a **Star** ⭐ — it helps a lot!

---

## Security

**Credentials never leave your machine.** All tokens, cookies, and API keys stay in local process memory. Nothing is uploaded anywhere.

**Direct connections only.** Requests go straight from your machine to the AI provider. No relay server in between.

**No request logging.** Message content is never logged, cached, or persisted.

**Auditable.** The credential handling code ([cookie.ts](src/cookie.ts)) is available for security review.

See [DISCLAIMER.md](DISCLAIMER.md) and [PRIVACY.md](PRIVACY.md) for details.

## FAQ

Having trouble? Check the **[FAQ (English)](faq-en.md)** | **[常见问题 (中文)](faq.md)** for solutions to common issues.

## Community

- LINUX DO: [linux.do](https://linux.do)
- Telegram Chat: [t.me/openrelay_chat](https://t.me/openrelay_chat)
- Telegram Updates: [t.me/openrelay_updates](https://t.me/openrelay_updates)
- Issues: [GitHub Issues](https://github.com/romgX/openrelay/issues)

**Star** ⭐ this repo + join [Telegram group](https://t.me/openrelay_chat) → chance to get **1-2 months Pro free**.

## License

Open Core model:
- **Framework** (proxy, format translation, config): [MIT](LICENSE)
- **Pro features** (custom model groups, unlimited requests): [Commercial](COMMERCIAL-LICENSE.txt)

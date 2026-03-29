# OpenRelay FAQ (Frequently Asked Questions)

> Check here first when you run into issues. If your question isn't covered, please open a [GitHub Issue](https://github.com/romgX/openrelay/issues).

---

## Table of Contents

1. [Installation & Startup](#installation--startup)
2. [Connection & Configuration](#connection--configuration)
3. [Providers & Quotas](#providers--quotas)
4. [Error Troubleshooting](#error-troubleshooting)
5. [IDE Integration](#ide-integration)
6. [Security & Privacy](#security--privacy)
7. [LINUX DO Connect](#linux-do-connect)

---

## Installation & Startup

### Q: How do I install OpenRelay?

Download the latest release from GitHub: https://github.com/romgX/openrelay/releases

Supported platforms: macOS (ARM64), Windows (x64).

The binary release requires no Node.js — it's ready to use out of the box.

### Q: How do I start it?

**Binary release:**

```bash
# macOS
./openrelay

# Windows
.\openrelay-windows-x64.exe
```

**npm install (requires Node.js >= 18):**

```bash
npm install -g openrelay
openrelay
```

First time on macOS — authorize Keychain access:

```bash
./openrelay --setup
```

Once started, open `http://localhost:18765` in your browser — all configuration is done through the Web dashboard.

### Q: How do I test all provider connections?

```bash
./openrelay --test
```

### Q: How do I set it to start on boot?

There's no built-in auto-start command yet. Recommended approaches:

macOS: Add the launch command to Login Items, or create a LaunchAgent plist.

Windows: Place a shortcut in the `shell:startup` folder.

---

## Connection & Configuration

### Q: How do I connect Claude Code?

**Option 1: Web dashboard one-click setup (recommended)**

Open `http://localhost:18765` → **Work** tab → select a Provider for Claude Code → toggle on → **reopen your terminal**.

**Option 2: Set environment variables manually**

```bash
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
```

### Q: How do I connect Cursor?

Open `http://localhost:18765` → **IDE** tab → Cursor section → select Provider and model → click **Start**.

**Important**: After starting the proxy, you **must launch Cursor from the dashboard's launch button**. Opening Cursor directly will bypass the proxy.

### Q: How do I connect Windsurf / Antigravity / VS Code Copilot?

Open `http://localhost:18765` → **IDE** tab → select the IDE → choose Provider and model → click **Start**.

- **Windsurf** — auto-configured after Start. Reopen Windsurf to apply.
- **Antigravity** — auto-configured after Start. **Restart Antigravity** to apply.
- **VS Code Copilot** — auto-configured after Start. Select the Ollama model in Copilot Chat.

### Q: How do I connect Aider / Goose / Amp and other CLI tools?

Open `http://localhost:18765` → **Work** tab → select a Provider for each tool → toggle on → **reopen your terminal**.

### Q: How do I view current configuration?

Open the Web dashboard at `http://localhost:18765` — all provider status, connections, and usage stats are shown there.

Config file location: `~/.openrelay/config.json`

---

## Providers & Quotas

### Q: How do I add free providers?

**IDE Providers** (auto-discovered, no setup needed): Claude Desktop, Claude Code, Kiro, Windsurf, Antigravity, OpenCode, VS Code Copilot.

**Direct API Providers** (API key required): Open the Web dashboard → click an unconnected API provider in the sidebar → enter your API key.

Recommended free providers:
- **Groq** — 14,400 req/day, Llama 3.3 70B, extremely fast
- **Cerebras** — 1M tokens/day, Llama 70B
- **Gemini** — generous free tier, 1M context
- **SambaNova** — Llama 405B, 200K tokens/day
- **OpenRouter** — 30+ free models

### Q: What happens when free quota runs out?

Use model groups (**Custom** tab) to combine multiple providers. When Groq runs out → auto-failover to Cerebras → then SambaNova. We recommend registering multiple providers for better reliability.

### Q: How do I check provider status?

Open the Web dashboard at `http://localhost:18765` — green dots in the sidebar mean the provider is connected. Click any provider for detailed status and quota info.

---

## Error Troubleshooting

### Q: `impit not available (Cannot find module 'impit/index.wrapper.js')`

**This is a normal informational message, not an error.**

impit is an optional Chrome TLS fingerprint library used to bypass CloudFlare JA3/JA4 detection. The binary release cannot embed impit (it contains native .node files), so it falls back to native fetch automatically.

If you encounter persistent 403 errors with the Claude Desktop provider, use the npm install method (`npm install -g openrelay`) — impit will be installed and enabled automatically.

### Q: `connection refused localhost:18765`

OpenRelay is not running. Start it:

```bash
./openrelay          # macOS
.\openrelay-windows-x64.exe   # Windows
```

If already started but still getting errors, check if the port is in use:

```bash
# macOS
lsof -i :18765

# Windows
netstat -ano | findstr 18765
```

### Q: `401 Unauthorized`

API key expired or invalid. Open the Web dashboard and check the connection status of the affected provider.

For IDE providers (Claude Desktop, Kiro, etc.), try opening the corresponding IDE app to refresh its token, then click "Reconnect" in the dashboard.

### Q: `rate limit exceeded`

Current provider quota is exhausted. OpenRelay automatically switches to the next available provider (if you've configured model groups).

If all providers are rate-limited:
1. Wait a few minutes and retry (most free quotas reset per minute/hour)
2. Add more providers in the Web dashboard
3. Check if any provider keys have expired

### Q: `403 Forbidden` (CloudFlare block)

Some providers (e.g., Claude Desktop) use CloudFlare protection. OpenRelay automatically refreshes cookies and retries.

If 403 persists:
1. Check if impit is loaded (look in startup logs)
2. Restart OpenRelay
3. Reopen Claude Desktop to let it refresh cookies

### Q: `ECONNRESET` or `socket hang up`

Network instability or provider-side disconnection. Usually temporary — just retry.

If it happens frequently:
1. Check your network connection
2. If using a proxy (Clash, etc.), add provider domains to your direct rules
3. Switch to a different provider in the Web dashboard

### Q: `EADDRINUSE` on startup

Port 18765 is already in use. Another OpenRelay instance may be running.

```bash
# macOS — find the process
lsof -i :18765
kill <PID>

# Windows
netstat -ano | findstr 18765
taskkill /PID <PID> /F
```

### Q: Using Clash or other proxy?

OpenRelay auto-detects Clash fake-ip (198.18.x.x) and uses DoH fallback. For best results, add `license.limitlessmeto.com` to your direct rules.

---

## IDE Integration

### Q: Claude Code shows `model not found`

Confirm OpenRelay is running and check available models:

```bash
curl http://localhost:18765/v1/models
```

Make sure environment variables are set correctly. The easiest way: configure in the Web dashboard **Work** tab, then reopen your terminal.

### Q: Cursor can't connect to OpenRelay

1. Confirm OpenRelay is running
2. Start the Cursor RPC proxy in the Web dashboard **IDE** tab
3. **You must launch Cursor from the dashboard's launch button** (opening Cursor directly bypasses the proxy)
4. First-time use requires trusting the TLS certificate (the dashboard will guide you)

### Q: Kiro shows disconnected

Kiro's AWS token expires after ~1 hour. To fix:
1. Open Kiro IDE to let it refresh the token
2. Go back to the Web dashboard and click "Reconnect"
3. You can also use "Switch Account" in the dashboard to re-login

### Q: Responses are very slow

1. Check which provider/model is in use — larger models (e.g., 405B) are slower
2. Switch to faster providers (Groq and Cerebras are the fastest)
3. Check network latency

### Q: Where is my data?

Config file: `~/.openrelay/config.json`

Logs print to the terminal (stdout) where OpenRelay was launched. They contain only errors and request metadata (provider, model, status) — **no message content or credentials**.

To delete all data: `rm -rf ~/.openrelay/`

---

## Security & Privacy

### Q: Will using OpenRelay get my account banned?

No. OpenRelay is fundamentally different from a "reverse proxy":

- **Reverse proxy**: Your requests go through someone else's server, multiple users share one API key — providers can detect this and ban the account.
- **OpenRelay**: Runs entirely on your local machine. Requests go directly from your computer to the AI provider, exactly the same as calling the API yourself. The provider sees a normal user using their own quota — there's nothing to flag or ban.

In short: OpenRelay just manages and routes requests locally. It doesn't touch your account and nothing passes through any middleman server.

The only thing to keep in mind: if a provider changes their free tier policy (e.g., removes free credits), that's the provider's decision and has nothing to do with OpenRelay. When that happens, OpenRelay automatically switches to another provider that still has quota available.

### Q: Are my API keys / tokens safe? Could they be uploaded?

Absolutely safe. OpenRelay's security design:

1. **Credentials never leave your machine** — All API keys, tokens, and cookies stay in local process memory. Nothing is uploaded anywhere.
2. **Direct connections only** — Requests go straight from your machine to the AI provider. No third-party server in between.
3. **No chat logging** — Logs only contain errors and request metadata (provider, model, status code). Your conversation content is never logged or cached.
4. **Auditable code** — The credential handling code (`cookie.ts`) is open for review. You can check it yourself if you have any concerns.

Your data is stored only in `~/.openrelay/config.json`. To delete everything: `rm -rf ~/.openrelay/`.

---

## LINUX DO Connect

### Q: What does LINUX DO login do?

[LINUX DO](https://linux.do) community members can log in to OpenRelay with their linux.do account to receive community-exclusive benefits. Click the LINUX DO button in the Dashboard top-right corner to log in.

### Q: Does this affect my existing OpenRelay account?

No. LINUX DO login is completely independent from OpenRelay's registration and Pro subscription system.

---

> Last updated: 2026-03-29
>
> If your question isn't listed here, please open a [GitHub Issue](https://github.com/romgX/openrelay/issues) and we'll add it to this FAQ.

# OpenRelay 常见问题与解答 (FAQ)

> 遇到问题先查这里。如果未解决，请到 [GitHub Issues](https://github.com/romgX/openrelay/issues) 提问。

---

## 目录

1. [安装与启动](#安装与启动)
2. [连接与配置](#连接与配置)
3. [Provider 与配额](#provider-与配额)
4. [报错排查](#报错排查)
5. [IDE 集成](#ide-集成)
6. [安全与隐私](#安全与隐私)
7. [LINUX DO Connect](#linux-do-connect)

---

## 安装与启动

### Q: 怎么安装 OpenRelay？

从 GitHub 下载最新版本：https://github.com/romgX/openrelay/releases

支持平台：macOS (ARM64)、Windows (x64)。

二进制版本不需要 Node.js，开箱即用。

### Q: 怎么启动？

**二进制版本：**

```bash
# macOS
./openrelay

# Windows
.\openrelay-windows-x64.exe
```

**npm 安装版本（需要 Node.js >= 18）：**

```bash
npm install -g openrelay
openrelay
```

macOS 首次运行需授权 Keychain：

```bash
./openrelay --setup
```

启动后打开浏览器访问 `http://localhost:18765` — 所有配置都在 Web 面板中完成。

### Q: 怎么测试所有 Provider 连接？

```bash
./openrelay --test
```

### Q: 怎么设置开机自启？

目前没有内置的开机自启命令。推荐方式：

macOS：将启动命令添加到 Login Items，或创建 LaunchAgent plist。

Windows：将快捷方式放入 `shell:startup` 文件夹。

---

## 连接与配置

### Q: 怎么连接 Claude Code？

**方法一：Web 面板一键配置（推荐）**

打开 `http://localhost:18765` → **Work** 标签页 → 为 Claude Code 选择 Provider → 开启开关 → **重新打开终端**。

**方法二：手动设置环境变量**

```bash
export ANTHROPIC_BASE_URL=http://localhost:18765
export ANTHROPIC_API_KEY=unused
```

### Q: 怎么连接 Cursor？

打开 `http://localhost:18765` → **IDE** 标签页 → Cursor 区域选择 Provider 和模型 → 点击 **启动**。

**注意**：启动代理后，**必须通过控制面板的启动按钮打开 Cursor**。直接打开 Cursor 将绕过代理。

### Q: 怎么连接 Windsurf / Antigravity / VS Code Copilot？

打开 `http://localhost:18765` → **IDE** 标签页 → 选择对应 IDE → 选择 Provider 和模型 → 点击 **启动**。

- **Windsurf** — 启动后自动配置，重新打开 Windsurf 即可生效
- **Antigravity** — 启动后自动配置，**需要重启 Antigravity** 才能生效
- **VS Code Copilot** — 启动后自动配置，在 Copilot Chat 中选择 Ollama 模型使用

### Q: 怎么连接 Aider / Goose / Amp 等 CLI 工具？

打开 `http://localhost:18765` → **Work** 标签页 → 为每个工具选择 Provider → 开启开关 → **重新打开终端**。

### Q: 怎么查看当前配置？

打开 Web 面板 `http://localhost:18765`，所有 Provider 状态、连接配置、使用量都在面板中展示。

配置文件位置：`~/.openrelay/config.json`

---

## Provider 与配额

### Q: 怎么添加免费 Provider？

**IDE Provider**（自动发现，无需配置）：Claude Desktop、Claude Code、Kiro、Windsurf、Antigravity、OpenCode、VS Code Copilot。

**直连 API Provider**（需要 API Key）：打开 Web 面板 → 点击侧边栏中未连接的 API Provider → 输入 API Key 即可。

推荐免费 Provider：
- **Groq** — 14,400 次/天，Llama 3.3 70B，速度极快
- **Cerebras** — 100 万 token/天，Llama 70B
- **Gemini** — 免费额度大，100 万上下文
- **SambaNova** — Llama 405B，200K token/天
- **OpenRouter** — 30+ 免费模型

### Q: 免费配额用完了怎么办？

使用模型组功能（**Custom** 标签页），将多个 Provider 组合在一起。Groq 用完 → 自动切到 Cerebras → 再切 SambaNova。建议多注册几个 Provider，配额叠加更稳。

### Q: 怎么查看各 Provider 状态？

打开 Web 面板 `http://localhost:18765`，侧边栏绿点表示已连接。点击任一 Provider 可查看详细状态和配额信息。

---

## 报错排查

### Q: `impit not available (Cannot find module 'impit/index.wrapper.js')`

**这是正常的提示信息，不是错误。**

impit 是可选的 Chrome TLS 指纹模拟库，用于绕过 CloudFlare 的 JA3/JA4 检测。二进制版本中无法内嵌 impit（含原生 .node 文件），会自动降级到原生 fetch。

如果使用 Claude Desktop Provider 遇到持续 403 错误，可改用 npm 安装方式（`npm install -g openrelay`），impit 会自动安装并启用。

### Q: `connection refused localhost:18765`

OpenRelay 没有运行。启动它：

```bash
./openrelay          # macOS
.\openrelay-windows-x64.exe   # Windows
```

如果已经启动但仍报错，检查端口是否被占用：

```bash
# macOS
lsof -i :18765

# Windows
netstat -ano | findstr 18765
```

### Q: `401 Unauthorized`

API Key 过期或无效。打开 Web 面板检查对应 Provider 的连接状态。

如果是 IDE Provider（Claude Desktop、Kiro 等），尝试打开对应的 IDE 应用让它刷新 Token，然后在 Web 面板中点击「重新连接」。

### Q: `rate limit exceeded`

当前 Provider 配额用完了。OpenRelay 会自动切换到下一个可用 Provider（如果配置了模型组）。

如果所有 Provider 都限流：
1. 等几分钟后重试（大部分免费配额按分钟/小时重置）
2. 在 Web 面板添加更多 Provider
3. 检查是否有 Provider 的 Key 失效了

### Q: `403 Forbidden`（CloudFlare 拦截）

部分 Provider（如 Claude Desktop）使用 CloudFlare 防护。OpenRelay 会自动刷新 Cookie 并重试。

如果持续 403：
1. 检查启动日志中 impit 是否已加载
2. 重启 OpenRelay
3. 重新打开 Claude Desktop 让它刷新 Cookie

### Q: `ECONNRESET` 或 `socket hang up`

网络不稳定或 Provider 服务端断开连接。通常是临时问题，重试即可。

如果频繁出现：
1. 检查网络连接
2. 如果使用代理（Clash 等），将 Provider 域名加入直连规则
3. 在 Web 面板切换到其他 Provider

### Q: 启动时报 `EADDRINUSE`

端口 18765 已被占用。可能是另一个 OpenRelay 实例在运行。

```bash
# macOS — 查找占用进程
lsof -i :18765
kill <PID>

# Windows
netstat -ano | findstr 18765
taskkill /PID <PID> /F
```

### Q: 使用 Clash/代理？

OpenRelay 自动检测 Clash fake-ip（198.18.x.x）并通过 DoH 回退。建议将 `license.limitlessmeto.com` 加入直连规则以获得最佳体验。

---

## IDE 集成

### Q: Claude Code 显示 `model not found`

确认 OpenRelay 已启动并检查可用模型：

```bash
curl http://localhost:18765/v1/models
```

确保环境变量设置正确。最简单的方法：在 Web 面板 **Work** 标签页中配置，然后重新打开终端。

### Q: Cursor 连不上 OpenRelay

1. 确认 OpenRelay 已启动
2. 在 Web 面板 **IDE** 标签页启动 Cursor RPC 代理
3. **必须通过面板的启动按钮打开 Cursor**（直接打开 Cursor 会绕过代理）
4. 首次使用需要信任 TLS 证书（面板会提示操作步骤）

### Q: Kiro 显示断开连接

Kiro 的 AWS Token 约 1 小时过期。解决方法：
1. 打开 Kiro IDE 让它刷新 Token
2. 回到 Web 面板点击「重新连接」
3. 也可以在面板中使用「切换账户」重新登录

### Q: 响应速度很慢

1. 检查当前使用的 Provider 和模型 — 大模型（如 405B）比小模型慢
2. 切换到速度更快的 Provider（Groq、Cerebras 速度最快）
3. 检查网络延迟

### Q: 数据存在哪里？

配置文件：`~/.openrelay/config.json`

日志输出到启动 OpenRelay 的终端窗口（stdout），仅包含错误信息和请求元数据（Provider、模型、状态码），**不含消息内容或凭据**。

删除所有数据：`rm -rf ~/.openrelay/`

---

## 安全与隐私

### Q: 用 OpenRelay 会不会被封号？

不会。OpenRelay 跟"反代"（反向代理）完全不同：

- **反代**：你的请求经过别人的服务器，多人共用一个 Key，容易被 Provider 检测到异常然后封号。
- **OpenRelay**：纯本地运行，请求从你自己的电脑直接发到 AI Provider，跟你手动调 API 没有任何区别。Provider 看到的就是一个正常用户在用自己的配额，不存在封号风险。

简单说：OpenRelay 只是帮你在本地管理和转发请求，不碰你的账号，不经过任何中间服务器。

唯一需要注意的是：如果某个 Provider 自己调整了免费额度政策（比如取消免费 tier），那是 Provider 的决定，跟用不用 OpenRelay 无关。这种情况下 OpenRelay 会自动切换到其他还有额度的 Provider，不影响使用。

### Q: 我的 API Key / Token 安全吗？会不会被上传？

绝对不会。OpenRelay 的安全设计：

1. **凭据不离开本机** — 所有 API Key、Token、Cookie 只在你电脑的本地内存中使用，不会上传到任何地方。
2. **直连 AI 后端** — 请求从你的机器直接发到 AI Provider，中间没有任何第三方服务器。
3. **不记录聊天内容** — 日志只包含错误信息和请求元数据（哪个 Provider、哪个模型、状态码），你的对话内容从不被记录或缓存。
4. **代码可审计** — 凭据处理的核心代码（`cookie.ts`）公开可查，不放心可以自己看。

你的数据只存在 `~/.openrelay/config.json` 里，想彻底删除执行 `rm -rf ~/.openrelay/` 即可。

---

## LINUX DO Connect

### Q: LINUX DO 登录有什么用？

[LINUX DO](https://linux.do) 社区用户可以使用 linux.do 账号登录 OpenRelay，获得社区专属权益。在 Dashboard 右上角点击 LINUX DO 按钮即可登录。

### Q: 登录后影响原来的账号吗？

不影响。LINUX DO 登录与 OpenRelay 的注册/Pro 付费是两套独立系统，互不干扰。

---

> 最后更新：2026-03-29
>
> 如果你的问题不在列表中，请到 [GitHub Issues](https://github.com/romgX/openrelay/issues) 提问，我们会及时补充到本文档。

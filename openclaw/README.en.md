[简体中文](./README.md) | **English**

# OpenClaw Best Practices

> OpenClaw is an open-source AI personal assistant framework (330k+ GitHub Stars). Its defining features are **local execution + multi-platform connectivity + autonomous task execution**. It's not just a chatbot — it can browse the web, read and write files, execute commands, and schedule cron jobs. Supports Claude, GPT, DeepSeek, local models, and more.

---

## Core Concepts

| Concept | Description | Use Case |
|---------|-------------|----------|
| **Gateway** | Background-resident WebSocket gateway | Route messages, manage Agents |
| **Channel** | Messaging platform connection | Connect WeChat, Telegram, Slack, Discord, etc. |
| **Skill** | Capability module defined in `SKILL.md` | Teach AI how to do things (similar to Claude Code Skills) |
| **Agent** | Independent AI workspace | Different tasks use different Agents |
| **Cron** | Scheduled task scheduler | Automate recurring work |
| **Tool** | Built-in tools (browser, filesystem, shell) | Give AI "hands" to execute operations |

---

## Getting Started

### Installation

```bash
# macOS / Linux / WSL2 (recommended)
curl -fsSL https://openclaw.ai/install.sh | bash

# Or via npm
npm install -g openclaw@latest
openclaw onboard --install-daemon

# Verify installation
openclaw --version
openclaw doctor
```

System requirements: Node.js 22.14+ (24 recommended)

### Initialization

```bash
# Interactive setup (configure models, channels, etc.)
openclaw onboard

# Check gateway status
openclaw gateway status

# Start the gateway
openclaw gateway start
```

### Configuration

Config file is located at `~/.openclaw/openclaw.json` (JSON5 format, comments supported):

```json5
{
  // Model settings
  "models": {
    "default": "claude-sonnet-4-20250514",
    // Or use DeepSeek to save money
    // "default": "deepseek/deepseek-chat",
  },

  // Channels (enable as needed)
  "channels": {
    "telegram": { "enabled": true },
    "webchat": { "enabled": true },
  },
}
```

Changes are hot-reloaded automatically — no restart needed.

---

## Prompting Tips

### 1. Skills — Teach AI New Abilities

OpenClaw Skills work similarly to Claude Code Skills, defined in Markdown:

```markdown
---
name: code-reviewer
description: Review code quality, find security vulnerabilities and performance issues
user-invocable: true
---

# Code Review

You are a senior code reviewer. When asked to review code:

1. Read through the entire file first to understand the context
2. Check for security issues (injection, XSS, sensitive data leaks)
3. Check for performance issues (N+1 queries, memory leaks)
4. Check code standards (naming, structure, comments)
5. Provide specific fix suggestions with code examples
```

Skills load from three locations (highest to lowest priority):

```
project-dir/skills/     -> Project-level (highest priority)
~/.openclaw/skills/     -> Global-level
built-in skills/        -> Default (lowest priority)
```

Install community Skills:

```bash
# Install from ClawHub
openclaw skills install <skill-slug>

# List installed Skills
openclaw skills list
```

### 2. Multi-Model Strategy

```bash
# List available models
openclaw models list

# Set default model
openclaw models set default claude-sonnet-4-20250514

# Use Claude for complex tasks
openclaw models set default claude-sonnet-4-20250514

# Use DeepSeek for casual chat (cheaper)
openclaw models set default deepseek/deepseek-chat

# Use a local model for free
openclaw models set default ollama/qwen2.5-coder
```

### 3. Automated Tasks

OpenClaw supports cron scheduling for automation:

```bash
# Send a codebase health report every day at 9 AM
openclaw cron add "0 9 * * *" "Check project code quality and send me a report"

# Send a weekly summary every Monday morning
openclaw cron add "0 9 * * 1" "Summarize last week's Git commits and PRs into a weekly report"

# List all scheduled tasks
openclaw cron list
```

### 4. Multi-Channel Collaboration

OpenClaw's unique advantage is connecting multiple messaging platforms:

```bash
# Add channels
openclaw channels add telegram
openclaw channels add webchat

# Check channel status
openclaw channels status
```

Use cases:
- **Telegram** — Send messages to AI from anywhere to execute tasks
- **WebChat** — Browser UI for complex interactions
- **Slack / Lark** — Team collaboration with AI as a team member

---

## Advanced Tips

### Agent Workspaces

Use different Agents for different projects to isolate context:

```bash
# Create a new Agent
openclaw agents add my-project

# List all Agents
openclaw agents list

# Delete an Agent you no longer need
openclaw agents delete old-project
```

### CLI Command Reference

| Command | Purpose |
|---------|---------|
| `openclaw onboard` | Interactive setup |
| `openclaw gateway start/stop/status` | Manage gateway |
| `openclaw channels add/remove/status` | Manage messaging channels |
| `openclaw models list/set` | Manage models |
| `openclaw skills list/install` | Manage Skills |
| `openclaw cron add/list` | Scheduled tasks |
| `openclaw agents list/add/delete` | Manage Agent workspaces |
| `openclaw doctor` | Health check and diagnostics |
| `openclaw logs` | View gateway logs |

### Debugging and Troubleshooting

```bash
# Health check (most useful diagnostic command)
openclaw doctor

# View real-time logs
openclaw logs

# Development mode (more debug info)
openclaw gateway --dev
```

---

## How It Differs from Other Tools

| Dimension | OpenClaw | Claude Code | Cursor |
|-----------|----------|-------------|--------|
| Type | AI Agent framework | CLI coding assistant | AI IDE |
| Core use case | Multi-platform automation | Code writing and refactoring | Daily coding |
| Runtime | Background daemon (Gateway) | On-demand | Embedded in IDE |
| Messaging platforms | 20+ platforms | Terminal only | IDE only |
| Model support | Claude/GPT/DeepSeek/local | Claude only | Multi-model |
| Skills | SKILL.md | .claude/skills/ | Rules |
| Scheduled tasks | Built-in Cron | No | No |
| Open source | Yes (MIT) | No | No |
| Best for | Automation, multi-platform, all-in-one assistant | Professional coding | Daily coding |

**OpenClaw vs Claude Code**: They're complementary, not competing. Claude Code focuses on programming, OpenClaw focuses on automation and multi-platform connectivity. Use both together.

---

## Common Pitfalls

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Node version too old | Requires 22.14+ | `nvm install 24` |
| Gateway fails to start | Port conflict or config error | Run `openclaw doctor` to diagnose |
| Skill not taking effect | Path or format incorrect | Check `SKILL.md` frontmatter for name and description |
| Model API errors | API key not set or balance depleted | Run `openclaw models status` to check |
| Channel disconnected | Network or auth issue | `openclaw channels status` + `openclaw channels reconnect` |

---

## Configuration Templates

| Template | Purpose |
|----------|---------|
| [code-reviewer.md](templates/code-reviewer.md) | Code review Skill template, copy to `~/.openclaw/skills/` or project `skills/` directory |

---

## Further Reading

- [OpenClaw Official Docs](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw) (338k+ stars)
- [ClawHub — Skill Marketplace](https://clawhub.com)
- [superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) — Skills methodology (also supports OpenClaw)

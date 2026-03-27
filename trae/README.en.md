[简体中文](./README.md) | **English**

# Trae Best Practices

> Trae is an AI IDE by ByteDance (based on VS Code), offering **free access to Claude and GPT models**. Developer-friendly for the Chinese market — Chinese UI, direct access in China, and generous free quotas. Great for getting started with AI coding and budget-conscious teams.

---

## Core Concepts

| Concept | Description | Use Case |
|---------|-------------|----------|
| **Builder** | Agent mode, autonomously completes tasks | Complex tasks, cross-file changes |
| **Chat** | Sidebar conversation | Q&A, understanding code |
| **Completion** | Inline code completion | Daily coding |
| **Rules** | `.trae/rules/` | Project-level rules |
| **@ References** | `@file` `@folder` `@web` | Pinpoint context precisely |

---

## Getting Started

### Installation

Download from [trae.ai](https://trae.ai). Supports macOS / Windows / Linux.

After signing up you can start using it right away — **no API key needed**.

### Rules — Project Rules

Create `.trae/rules/project_rules.md`:

```markdown
# Project Rules

## Tech Stack
React + TypeScript + Ant Design + UmiJS

## Code Standards
- Use functional components + Hooks
- State management: @umijs/max built-in Model
- Use umi-request for HTTP calls, not fetch/axios directly
- Use CSS Modules for styling

## Localization Standards
- Comments in Chinese
- Commit messages in Chinese
- Variable names in English
```

### Builder — Agent Mode

Builder is Trae's Agent, similar to Cursor's Composer:

```
Use Builder mode.
Reference the pattern in src/pages/user/list.tsx,
create a new order list page at src/pages/order/list.tsx.
Requirements:
1. Table uses Ant Design ProTable
2. Support filtering by date, status, and amount
3. Support Excel export
4. Action column: view details, cancel order, refund
```

---

## Prompting Tips

### 1. Making the Most of Free Models

Trae provides Claude and GPT for free. Allocate wisely:

```
# Complex tasks — use Claude
Builder mode + Claude: refactor the entire auth module

# Simple tasks — use GPT
Chat mode + GPT: explain this code / write a comment
```

### 2. Chinese-Friendly

Trae has the best Chinese language support — you can interact entirely in Chinese:

```
Refactor the request wrapper in src/utils/request.ts:
1. Add unified loading state management
2. Show errors using Ant Design's message component
3. Auto-redirect to login on 401 errors
4. Set network timeout to 10 seconds
```

### 3. Ant Design Ecosystem

Trae works well with popular Chinese component libraries:

```
@https://ant-design.antgroup.com/components/table-cn
Refer to the Ant Design docs,
add a custom row expansion feature to ProTable
that shows order item details when expanded.
```

---

## How It Differs from Cursor

| Dimension | Trae | Cursor |
|-----------|------|--------|
| Price | **Free** (includes Claude/GPT) | $20/month |
| Chinese support | 3/3 (Chinese UI) | 1/3 (English only) |
| Access in China | 3/3 (direct access) | 1/3 (requires proxy) |
| Agent capabilities | 2/3 | 3/3 |
| Rules system | 2/3 | 3/3 (glob-based on-demand loading) |
| Extension ecosystem | 2/3 (VS Code compatible) | 3/3 (VS Code compatible) |
| Best for | Getting started, budget-conscious, China-based teams | Advanced users, willing to pay, want the strongest tools |

---

## Common Pitfalls

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Builder runs slow | Free models may queue | Use Builder for non-urgent tasks, Chat for urgent ones |
| Model switching | Different models excel at different things | Claude for complex, GPT for simple |
| Rules not working | Wrong file path or format | Ensure files are under `.trae/rules/` in Markdown format |

---

## Configuration Templates

| Template | Purpose |
|----------|---------|
| [project_rules.md](templates/project_rules.md) | Project rules template (React + UmiJS + Ant Design), copy to `.trae/rules/` |

---

## Further Reading

- [Trae Official Website](https://trae.ai)
- [superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) — Skills methodology (also supports Trae)

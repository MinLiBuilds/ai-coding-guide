[简体中文](./README.md) | **English**

# Windsurf Best Practices

> Windsurf is an AI IDE from Codeium. Its standout feature is **Cascade** — an AI Agent that tracks your editing behavior. It automatically follows your actions (file switches, code changes, terminal output) and proactively offers help, rather than waiting for you to ask.

---

## Core Concepts

| Concept | Description | Use Case |
|---------|-------------|----------|
| **Cascade** | Context-aware Agent | Automatically understands what you're doing and suggests proactively |
| **Flows** | Action tracking | Records your editing trail as context |
| **Write Mode** | Direct code writing | Similar to Cursor Composer |
| **Chat Mode** | Conversational Q&A | Understanding code, asking questions |
| **Rules** | `.windsurfrules` | Project-level rule configuration |
| **@ References** | `@file` `@folder` `@web` etc. | Pinpoint context precisely |

---

## Getting Started

### Installation

Download from [windsurf.com](https://windsurf.com). Supports macOS / Windows / Linux. Based on VS Code — most existing VS Code extensions are compatible.

### .windsurfrules — Project Rules

Create `.windsurfrules` in your project root:

```markdown
# Project Rules

## Tech Stack
Vue 3 + TypeScript + Pinia + Element Plus

## Code Standards
- Use Composition API (script setup syntax)
- Components in PascalCase, files in kebab-case
- Split stores by feature module
- All API requests go through wrappers under src/api/

## Cascade Behavior
- Auto-check Props types when modifying components
- Remind to update TypeScript types when modifying API endpoints
- Do not auto-refactor code I haven't asked you to change
```

### Using Cascade Effectively

Cascade tracks your actions. Take advantage of this:

```
1. Open a few related files first (so Cascade knows what you're working on)
2. Make a small change (so Cascade understands your intent)
3. At this point, Cascade's suggestions will be more accurate than a cold start
```

---

## Prompting Tips

### 1. Write Mode — Large Tasks

```
Use Write mode.
Migrate all user management pages under src/views/user/ from Options API to Composition API.
Keep functionality unchanged, only change the syntax.
Go file by file. Let me confirm after each one.
```

### 2. Leverage Flows Context

```
# You just ran tests in the terminal and saw errors
# Cascade already knows — just say:
The test just failed, help me figure out why.
# No need to paste the error — Cascade already picked it up from the Flow
```

### 3. @ References

```
@src/api/user.ts @src/types/user.ts
The type definitions in these two files are inconsistent. Unify them.
Use types/user.ts as the source of truth.
```

---

## How It Differs from Cursor

| Dimension | Windsurf | Cursor |
|-----------|----------|--------|
| Core philosophy | **Proactive awareness**, tracks your actions and offers help automatically | **On-demand**, responds only when you ask |
| Agent | Cascade (auto-tracks Flows) | Composer (manually triggered) |
| Context | Automatically inferred from action flow | Requires manual @ references |
| Rules | `.windsurfrules` single file | `.cursor/rules/` with glob-based splitting |
| Models | Proprietary + Claude/GPT | Claude/GPT/proprietary |
| Best for | People who like AI to proactively help | People who prefer precise control |

---

## Common Pitfalls

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Cascade too proactive | You're just browsing code and it starts suggesting changes | Adjust Cascade sensitivity in settings |
| Flows context gets confused | Switched too many files, Cascade loses track of what you're doing | Start a new Cascade session |
| Write mode scope creep | Edits files it shouldn't | Use @ references to limit scope |

---

## Configuration Templates

| Template | Purpose |
|----------|---------|
| [.windsurfrules](templates/windsurfrules.md) | Project rules template (Vue 3 + TypeScript), copy to project root and rename to `.windsurfrules` |

---

## Further Reading

- [Windsurf Official Docs](https://docs.codeium.com/windsurf)
- [awesome-windsurf](https://github.com/detailobsessed/awesome-windsurf) — Community resource collection
- [superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) — Skills methodology (also supports Windsurf)

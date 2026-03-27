[简体中文](./tool-selection.md) | **English**

# Multi-Tool Selection Guide

> Different AI coding tools excel at different things. Pick the right tool and double your productivity; pick the wrong one and you'll waste time fighting it.

---

## Quick Reference: Which Tool for What

| Scenario | Recommended Tool | Why |
|----------|-----------------|-----|
| **Daily coding** (completions, small edits) | Cursor / Copilot | IDE integration, just press Tab |
| **Complex refactoring** (cross-file, architecture-level) | Claude Code | Strongest Agent capabilities, understands the whole project |
| **New project scaffolding** | Claude Code | Starting from scratch needs big-picture planning ability |
| **Bug debugging** | Claude Code | Can read logs, run commands, do systematic analysis |
| **Code review** | Claude Code / Cursor | Claude Code is more thorough, Cursor is quicker |
| **Learning a new codebase** | Cursor + Chat | Select code and ask questions — the most natural interaction |
| **Writing tests** | Claude Code | Can run tests, check coverage, auto-fix failures |
| **Documentation / comments** | Copilot | Inline completion makes writing comments seamless |
| **Frontend UI tweaks** | Cursor | Live preview + visual editing |
| **CLI / scripting** | Claude Code / Gemini CLI | CLI-native, fits terminal workflows |
| **Exploring large codebases** | Gemini CLI | Largest context window (2M tokens) |

---

## Tool Capability Comparison

| Capability | Claude Code | Cursor | Copilot | Gemini CLI |
|------------|:---:|:---:|:---:|:---:|
| Code completion | -- | 3/3 | 3/3 | -- |
| Conversational coding | 3/3 | 2/3 | 2/3 | 3/3 |
| Autonomous Agent execution | 3/3 | 2/3 | 2/3 | 2/3 |
| Terminal command execution | 3/3 | -- | -- | 3/3 |
| Multi-file coordination | 3/3 | 2/3 | 2/3 | 2/3 |
| Project rules config | 3/3 | 3/3 | 2/3 | 2/3 |
| Extensibility (MCP) | 3/3 | 2/3 | 2/3 | 2/3 |
| Context window | 2/3 | 2/3 | 2/3 | 3/3 |
| IDE integration | -- | 3/3 | 3/3 | -- |
| Free tier | 1/3 | 1/3 | 2/3 | 3/3 |

---

## Recommended Combos

### Combo 1: Claude Code + Cursor (Most Popular)

```
Claude Code -> Architecture design, complex refactoring, debugging, testing
Cursor      -> Daily coding, small edits, UI tweaks, quick Q&A
```

Best for: Full-stack developers who work on both frontend and backend

### Combo 2: Claude Code + Copilot (Lightweight)

```
Claude Code -> Agent tasks (refactoring, generation, debugging)
Copilot     -> Inline completion, writing comments, simple Q&A
```

Best for: Backend developers, VS Code users

### Combo 3: Gemini CLI + Cursor (Budget-Friendly)

```
Gemini CLI -> Large-scale analysis, code exploration (free)
Cursor     -> Daily coding, interactive editing
```

Best for: Solo developers who want to keep costs down

---

## When to Switch Tools

Signals that it's time to switch from one tool to another:

| Signal | Action |
|--------|--------|
| Cursor's completion is wrong after 3 attempts | Switch to Claude Code for systematic analysis |
| Claude Code conversation is too long and quality drops | Start a new conversation, or switch to Cursor for remaining small tasks |
| Need to see live results | Switch to Cursor / Copilot (in-IDE preview) |
| Need to run commands to verify | Switch to Claude Code / Gemini CLI (in-terminal) |
| Need to understand a large codebase | Switch to Gemini CLI (largest context) |

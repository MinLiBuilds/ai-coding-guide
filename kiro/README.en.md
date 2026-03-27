[简体中文](./README.md) | **English**

# Kiro Best Practices

> Kiro is an AI IDE from AWS. Its defining feature is **Spec-driven Development** — instead of having AI write code directly, it first generates requirement specs, design docs, and test cases. You review and approve the spec, then Kiro implements accordingly. Well suited for team collaboration and projects that demand high-quality deliverables.

---

## Core Concepts

| Concept | Description | Use Case |
|---------|-------------|----------|
| **Spec** | Requirement specification document | AI writes the spec first, implements after approval |
| **Steering** | `.kiro/steering/*.md` | Project-level rules and guidelines |
| **Hooks** | Automated triggers | Auto-validate/test on file save |
| **Agent** | Background autonomous execution | Completes complex tasks automatically |

---

## Getting Started

### Installation

Download from [kiro.dev](https://kiro.dev). Requires an AWS or GitHub account to log in. Currently in preview.

### Steering Files — Project Configuration

Create rule files under `.kiro/steering/`:

```markdown
---
mode: always
---

# Project Rules

## Tech Stack
Java 17 + Spring Boot 3 + MyBatis Plus + MySQL 8

## Code Standards
- Controller layer only handles parameter validation and routing
- Service layer handles business logic
- DAO layer only handles database operations
- Wrap all return values with Result<T>
- Use custom BusinessException for errors

## Naming Conventions
- Package names: all lowercase
- Class names: PascalCase
- Method names: camelCase
- Constants: UPPER_SNAKE_CASE
```

Steering supports three modes:
- `always` — Loaded for every conversation
- `globs: ["*.java"]` — Loaded only when working with matching files
- `manual` — Manually activated

### Spec-Driven Development

Kiro's workflow differs from other tools:

```
1. You describe the requirement
2. Kiro generates a Spec (requirements + technical design + test cases)
3. You review and revise the Spec
4. Once confirmed, Kiro implements step by step according to the Spec
5. After each step, related tests run automatically
```

This is slower than "just write the code," but produces higher quality output. Especially good for:
- Team collaboration (Specs can be reviewed)
- Complex features (think it through before building)
- Projects that need documentation

---

## Prompting Tips

### 1. Making Specs More Precise

```
Add a refund feature to the order module.

Business rules:
- Refunds allowed within 7 days of payment
- Both partial and full refunds supported
- Refunds over $500 require approval
- Refund to the original payment method

Generate the Spec first. I'll confirm before you implement.
```

### 2. Leverage Hooks for Auto-Validation

```json
// .kiro/hooks.json
{
  "on-save": {
    "*.java": "mvn compile -q",
    "*.test.java": "mvn test -pl ${module} -q"
  }
}
```

Auto-compiles on every Java file save, auto-runs tests on test file save.

### 3. Organize Steering by Module

```
.kiro/steering/
├── always.md              # Global rules (always)
├── api.md                 # API rules (globs: src/controller/**)
├── database.md            # Database rules (globs: src/mapper/**)
└── testing.md             # Testing rules (globs: src/test/**)
```

---

## How It Differs from Other Tools

| Dimension | Kiro | Claude Code | Cursor |
|-----------|------|-------------|--------|
| Core philosophy | Spec first | Agent execution | IDE completion |
| Best for | Team collaboration, high-quality delivery | Individual high-velocity development | Daily coding |
| Rules system | Steering (three modes) | CLAUDE.md + Skills | Rules (globs) |
| Development flow | Requirements -> Spec -> Implementation -> Verification | Requirements -> Implementation -> Verification | Requirements -> Implementation |
| AWS integration | 3/3 | 1/3 | 1/3 |

---

## Configuration Templates

| Template | Purpose |
|----------|---------|
| [steering-always.md](templates/steering-always.md) | Steering global rules template (Java + Spring Boot), copy to `.kiro/steering/` |

---

## Further Reading

- [Kiro Official Docs](https://kiro.dev/docs)
- [superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) — Skills methodology (also supports Kiro)

[简体中文](./prompting.md) | **English**

# Prompting Techniques for AI Coding

> This isn't a general prompt engineering tutorial. It's specifically about prompting for AI-assisted coding — applicable whether you use Claude Code, Cursor, or Copilot.

---

## Core Principles

### 1. Specific > Vague

AI can't read your mind. The more specific you are, the better the output.

```
❌ "Optimize this function"
✅ "This function takes 30 seconds to process 100K records.
    Target: under 5 seconds.
    Consider batching, caching, or a different algorithm.
    Don't change the function signature."
```

### 2. Constraints > Freedom

Unconstrained AI tends to over-engineer. Tell it explicitly what NOT to do.

```
❌ "Add caching"
✅ "Add caching to getUserById.
    Use an in-memory cache (Map). No Redis.
    TTL: 5 minutes.
    Don't modify other functions.
    Don't add new dependencies."
```

### 3. Stepwise > All-at-Once

Break complex tasks into steps. Confirm each before moving on.

```
❌ "Refactor the entire auth module"
✅ "Refactor the auth module in three steps:
    Step 1: Analyze the current issues and list them for my review.
    Step 2: Propose 2-3 refactoring options.
    Step 3: Implement only after I confirm the approach.
    Start with Step 1."
```

### 4. Examples > Descriptions

One example beats a paragraph of explanation.

```
❌ "Use a consistent response format"
✅ "Use this response format:
    {
      'code': 200,
      'data': { ... },
      'message': 'success'
    }
    On error:
    {
      'code': 400,
      'data': null,
      'message': 'Invalid email format'
    }"
```

### 5. Reference > Start from Scratch

Pointing to existing code is far more effective than describing a style.

```
❌ "Write a new API endpoint"
✅ "Follow the style of src/api/users.ts
    to create src/api/orders.ts.
    Match the routing, error handling, and response format."
```

---

## Common Prompt Patterns

### Analysis Mode (Look Before You Leap)

```
Read [file/directory] and analyze [specific concern].
List your findings. Don't modify any code yet.
Wait for my confirmation before making changes.
```

### Implementation Mode (Clear Requirements)

```
Implement [feature] in [location].
Requirements:
1. [Specific requirement 1]
2. [Specific requirement 2]
Follow the style of [existing file].
Do NOT [prohibited actions].
```

### Fix Mode (Provide Evidence)

```
[Error description / logs / screenshot].
Analyze possible causes (don't guess — look at the code and logs).
Confirm the root cause before proposing a fix.
Only change what's necessary. Don't refactor while you're at it.
```

### Review Mode (Define Scope)

```
Review [file/PR], focusing on:
1. [Concern 1, e.g., performance]
2. [Concern 2, e.g., security]
3. [Concern 3, e.g., edge cases]
Rank findings by severity and provide fix suggestions.
```

---

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Do This Instead |
|---|---|---|
| "Build me a website" | Scope too large — AI doesn't know where to start | Break it into concrete tasks |
| "Optimize this" | Optimize for what? Speed? Readability? Bundle size? | State the optimization goal and metrics |
| "Write the best code" | "Best" is undefined | Define your criteria (testable, maintainable, etc.) |
| Dumping 10 requirements at once | AI loses focus and drops things | One task at a time; verify before moving on |
| Saying "fix this bug" with no error info | AI can only guess | Include error logs and reproduction steps |

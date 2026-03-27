[简体中文](./code-review.md) | **English**

# AI-Assisted Code Review

> Using AI for code review isn't as simple as "check for bugs." Effective AI code review requires defining review dimensions, setting standards, and structuring the output by severity.

---

## Review Dimensions

When asking AI to review code, be explicit about what to focus on:

| Dimension | Focus Areas | Priority |
|---|---|:---:|
| **Security** | Injection, XSS, data leaks, auth bypass | Highest |
| **Correctness** | Logic errors, edge cases, concurrency issues | High |
| **Performance** | N+1 queries, memory leaks, unnecessary computation | Medium |
| **Maintainability** | Naming, structure, duplication, complexity | Low |
| **Testing** | Coverage, boundary tests, mock appropriateness | Medium |

---

## Review Prompt Templates

### Security Review

```
Review all endpoints under src/api/ for security issues:
1. SQL injection: Are parameters concatenated directly into SQL?
2. XSS: Is user input rendered without escaping?
3. Authorization: Does every endpoint enforce access control?
4. Data exposure: Are passwords or tokens logged anywhere?
5. Rate limiting: Do high-traffic endpoints have throttling?

Tag each finding by risk level: [Critical] / [Medium] / [Low]
```

### PR Review

```
Review the changes in this PR (git diff main...HEAD).
Focus on:
1. Do the changes match the stated PR objective?
2. Are there missing edge cases?
3. Are there new security risks?
4. Is test coverage sufficient?

Output format:
- [Must Fix] Issue description + suggestion
- [Should Fix] Issue description + suggestion
- [Nice to Have] Issue description + suggestion
```

### Performance Review

```
Review src/services/report.ts for performance:
1. N+1 queries
2. Computations that could be cached but aren't
3. Unnecessary full table scans
4. Potential OOM under large data volumes
5. Blocking operations that should be async

Provide a concrete improvement plan for each issue.
```

---

## Structuring Review Output

Good review output looks like this:

```
## [Must Fix] SQL Injection Risk
Location: src/api/users.ts:45
Issue: User-supplied `name` is concatenated directly into the SQL query
Fix: Use parameterized queries
```

```
## [Should Fix] Missing Error Handling
Location: src/services/payment.ts:78-82
Issue: API call has no try-catch — network failures cause unhandled Promise rejections
Fix: Add try-catch and return a clear error message on failure
```

```
## [Nice to Have] Duplicate Code
Location: src/api/users.ts:20-35 and src/api/orders.ts:15-30
Issue: Identical pagination logic in two places
Fix: Extract a shared buildPagination utility function
```

---

## Review Culture on Teams

If AI review results will be posted as PR comments, pay attention to tone:

```
❌ "This is wrong. You should use X."
✅ "[Should Fix] What was the reasoning behind reading the file synchronously here?
    Under high concurrency, this could block the event loop.
    Consider an async alternative."
```

Ask questions instead of making declarations. Suggest instead of command. But for critical security issues, mark them `[Must Fix]` — don't let politeness override safety.

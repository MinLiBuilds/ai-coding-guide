[简体中文](./testing.md) | **English**

# Testing Strategy

> Writing tests is one of the highest-ROI uses of AI coding tools. AI excels at generating repetitive test code, but you need to tell it *what* to test and *how* — otherwise it produces a pile of useless tests.

---

## Core Principles

### 1. Define a Testing Strategy Before Asking AI to Write Tests

```
❌ Bad: "Write tests for this file"
✅ Good: "Write unit tests for src/services/order.ts.
    Focus on:
    - createOrder: should throw when inventory is insufficient
    - calculateTotal: edge cases around stacked discounts
    - cancelOrder: should reject cancellation of shipped orders
    Use Vitest. Mock the database layer."
```

### 2. Spell Out the Edge Cases

AI is great at happy-path tests but tends to miss boundary conditions. Be explicit:

```
Write tests for the parseDate function covering:
- Normal input: "2024-01-15"
- Empty string
- null / undefined
- Invalid format: "not-a-date"
- Leap year: "2024-02-29"
- Timezone boundary: around UTC midnight
```

### 3. Don't Let AI Test Implementation Details

```
❌ Bad: Test that getUserList calls db.query exactly N times
✅ Good: Test that getUserList returns the correct user list,
    pagination parameters work, and an empty result returns an empty array
```

Test behavior, not implementation. Otherwise every refactor breaks your tests.

---

## Best Practices by Test Type

### Unit Tests

```
Write unit tests for all exported functions in src/utils/validator.ts.

Requirements:
1. At least 3 test cases per function (normal, boundary, error)
2. Group with describe blocks by function
3. Use descriptive test names, e.g., "should return false for empty string"
4. Don't mock dependencies of pure functions
```

### Integration Tests

```
Write integration tests for the order creation flow.
Scope: Controller → Service → Repository
Use a test database (don't mock the DB). Clear data before each test.

Scenarios:
1. Create order successfully → inventory decreases → returns order ID
2. Insufficient inventory → returns 400 → inventory unchanged
3. Concurrent creation → must not oversell
```

### API / E2E Tests

```
Write API tests for POST /api/auth/login.

Test cases:
1. Correct credentials → 200 + token
2. Wrong password → 401
3. Non-existent account → 401 (don't reveal "account not found")
4. Missing fields → 400 + specify which field is missing
5. 5 consecutive failures → 429 rate limit
```

---

## Efficient Workflows for AI-Generated Tests

### Workflow 1: Code First, Tests After

```
1. Implement the feature
2. Have AI read the code and generate tests
3. Run the tests — check for failures
4. Failing tests = potential bugs. Have AI determine if it's a code issue or a test issue
```

### Workflow 2: TDD — Tests First, Code After

```
1. Describe the requirements. Have AI write test cases only (no implementation)
2. Review the test cases to confirm they cover the desired behavior
3. Have AI write the implementation. Run tests until they all pass
4. Refactor with confidence (tests have your back)
```

TDD works especially well with AI because the tests serve as an unambiguous definition of done.

### Workflow 3: Tests as a Safety Net

```
Need to refactor src/services/payment.ts.
First, write comprehensive tests covering existing behavior.
Once they pass, start refactoring.
Run tests after every change.
```

---

## Common Anti-Patterns

| Anti-Pattern | Problem | Do This Instead |
|---|---|---|
| Have AI generate all tests at once | High quantity, low quality — many meaningless assertions | Generate per module, review each batch |
| Only test the happy path | Edge cases and error paths are uncovered | Explicitly list boundary conditions |
| Mock everything | Tests diverge from real behavior | Only mock external dependencies (network, DB). Don't mock pure logic |
| Copy-paste tests | Massive duplication, high maintenance cost | Use `test.each` or parameterized tests |
| Commit tests without reviewing | AI may assert incorrect expected values | Verify every assertion is correct |
| Chase 100% coverage | Wastes time testing getters/setters | Focus coverage on core logic, not vanity metrics |

---

## Tool-Specific Testing Tips

| Tool | Tip |
|---|---|
| **Claude Code** | Just say "run the tests and check the results" — it will execute and fix failing cases automatically |
| **Cursor** | Select a function → `Cmd+K` → "write unit tests" for quick generation |
| **Copilot** | Open a test file, type `describe('...')`, and autocomplete generates test cases |
| **Aider** | Set `auto-test: true` to run tests automatically after every code change |
| **Kiro** | Define test cases in the Spec; they're verified automatically during implementation |

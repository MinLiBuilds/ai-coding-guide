[简体中文](./debugging.md) | **English**

# AI-Assisted Debugging Methodology

> Most people debug with AI like this: paste the error, AI guesses a fix, doesn't work, guess again. This is the least effective approach. Systematic AI debugging should be: gather evidence, analyze root cause, validate hypothesis, then fix precisely.

---

## The Four-Phase Debugging Method

### Phase 1: Gather Evidence

**Don't let AI guess. Let it look.**

```
# Have AI read the error logs
Run pnpm test src/api/users.test.ts and paste the full output.

# Have AI review recent changes
Run git log --oneline -10 to see what changed recently.
Run git diff HEAD~3 to see which files were modified in the last three commits.

# Have AI examine the stack trace
Look at src/services/payment.ts around line 78 —
that's where the stack trace points.
```

### Phase 2: Analyze Root Cause

```
Based on the following evidence, analyze possible causes:
1. Error message: [paste it]
2. Recent changes: [relevant commits]
3. Related code: [files and line numbers]

List 2-3 most likely causes, ranked by probability.
Do NOT propose a fix yet.
```

### Phase 3: Validate the Hypothesis

```
You think the cause is [hypothesis].
How can we verify this?
Give me a minimal validation approach (add a console.log / write a test / change a parameter).
```

### Phase 4: Targeted Fix

```
Root cause confirmed: [cause].
Now provide the fix.
Requirements:
1. Only change what's necessary
2. Don't refactor while you're at it
3. Add a test covering this case
4. Run tests after the fix to verify
```

---

## Common Scenarios

### Test Failures

```
❌ Bad: "Tests are failing, fix them"
✅ Good: Run pnpm test -- --reporter=verbose,
    paste the failing test cases and error messages.
    Then examine both the source code and test code
    to determine if it's a bug in the source or an incorrect test.
```

### Production Errors

```
Production error: [error message]
Impact: [which users/features affected]

Investigate in this order:
1. Check git log for deployments in the last 24 hours
2. Trace the error stack to the code location
3. Determine if it's a code bug, config issue, or external dependency problem
4. Provide both a quick mitigation (stop the bleeding) and a proper fix
```

### Performance Issues

```
[Endpoint/page] response time jumped from 200ms to 3s.

Investigation steps:
1. Check for slow DB queries (examine SQL logs)
2. Check for N+1 query problems
3. Check if an external API call got slower
4. Check if a recent code change introduced a performance regression

Use process of elimination. Confirm each one before moving on.
```

---

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Do This Instead |
|---|---|---|
| Paste the error and let AI guess | AI's blind guess success rate is < 30% | Gather evidence first, then analyze |
| Try a different fix when one doesn't work | Switching fixes without finding the root cause = hoping to get lucky | Validate hypotheses, confirm root cause |
| Let AI change code without testing | May introduce new bugs | Always run tests after a fix |
| Same bug, five rounds of back-and-forth | You're going in circles — the approach is wrong | Stop, re-analyze from scratch, try a different angle |

[简体中文](./task-decomposition.md) | **English**

# Task Decomposition

> AI coding tools have a much higher success rate on small, well-defined tasks than on large, ambiguous ones. Breaking big requirements into tasks that AI can "get right in one shot" is the core skill of effective AI-assisted development.

---

## Decomposition Principles

### 1. Each Task Should Be an Atomic Operation

```
❌ Too big: "Refactor the entire user module"
✅ Right-sized:
  Task 1: Migrate UserService DB queries from raw SQL to ORM
  Task 2: Add unit tests for every public method in UserService
  Task 3: Replace hand-written validation in UserController with Zod
  Task 4: Standardize error response format across User endpoints
```

### 2. Each Task Needs a Clear Definition of Done

```
❌ Vague: "Improve performance"
✅ Clear: "Bring getUserList response time from 3s down to under 500ms.
          Run pnpm test to verify no regressions.
          Run ab -n 1000 -c 10 to confirm the performance target."
```

### 3. Dependencies Between Tasks Should Be Explicit

```
Task 1: Create Order model and DB table (independent — do first)
Task 2: Implement OrderService CRUD (depends on Task 1)
Task 3: Implement OrderController endpoints (depends on Task 2)
Task 4: Write integration tests (depends on Task 3)
Task 5: Add authorization checks (depends on Task 3)

Tasks 4 and 5 can run in parallel.
```

---

## Decomposition Templates

### New Feature Development

```
Round 1: Design
- Analyze requirements, raise clarifying questions
- Propose 2-3 technical approaches
- Confirm the approach

Round 2: Data Layer
- Create data models / DB tables
- Write data access layer
- Verify: can read/write the database correctly

Round 3: Business Logic
- Implement core business logic
- Write unit tests
- Verify: tests pass

Round 4: API Layer
- Implement API endpoints
- Add input validation and authorization
- Write API tests
- Verify: endpoints work correctly

Round 5: Polish
- Add error handling and logging
- Update documentation
- Full regression test
```

### Bug Fix

```
Step 1: Reproduce and locate (don't change code)
Step 2: Analyze root cause (form hypotheses)
Step 3: Confirm root cause (validate hypotheses)
Step 4: Fix + write a regression test
Step 5: Verify the fix + run regression tests
```

### Refactoring

```
Step 1: Write tests covering existing behavior (safety net)
Step 2: Refactor in small increments (one change at a time)
Step 3: Run tests after each change (ensure nothing breaks)
Step 4: Repeat Steps 2 and 3
Step 5: Clean up (remove transitional code, update docs)
```

---

## Worked Example

Requirement: Add a shopping cart feature to an e-commerce system

```
Decomposed:

1. [Data] Create cart and cart_item tables, write migration
   Done when: migrate up/down both succeed

2. [Model] Create Cart and CartItem ORM models
   Done when: can CRUD cart data

3. [Service] Implement CartService
   - addItem (add product to cart)
   - removeItem (remove product)
   - updateQuantity (change quantity)
   - getCart (retrieve cart)
   - clearCart (empty cart)
   Done when: all unit tests pass

4. [API] Implement REST endpoints
   - POST /api/cart/items
   - DELETE /api/cart/items/:id
   - PATCH /api/cart/items/:id
   - GET /api/cart
   - DELETE /api/cart
   Done when: API tests pass, authorization works correctly

5. [Integration] Connect with the order module
   - Cart checkout → create order
   - Clear cart after order is placed
   Done when: integration tests pass
```

Have AI complete each task independently. Verify the result before moving to the next one.

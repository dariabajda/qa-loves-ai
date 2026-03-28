# How to Write Good Prompts -- A Guide for QA Engineers

The difference between a mediocre AI response and a great one almost always comes down to the prompt. This guide teaches you to write prompts that produce reliable, useful results for your QA work.

---

## Structure of a good prompt

A well-structured prompt has four components. You don't always need all four, but the more you include, the better the output.

### 1. Role -- who should AI be?

Tell AI what perspective to take. This shapes the vocabulary, depth, and focus of the response.

```
You are a senior QA engineer with expertise in Playwright and API testing.
```

**Why it matters:** "Write tests" gives a generic answer. "As a senior QA engineer, write tests" gives answers that consider edge cases, test strategy, and maintainability.

### 2. Context -- what does AI need to know?

Provide background information: the feature under test, tech stack, constraints, existing code. In OpenCode, use `@` to attach files directly.

```
We have a login form at @src/pages/Login.tsx that uses OAuth2.
Validation rules: email must be valid format, password minimum 8 characters.
The form submits to POST /api/auth/login.
```

**Why it matters:** Without context, AI guesses. With context, AI works with your actual code and requirements.

### 3. Task -- what exactly should AI do?

Be specific about the action. Vague tasks produce vague results.

```
Generate test cases covering: happy path, validation errors,
edge cases (empty fields, SQL injection, XSS), and error responses
from the API (401, 500, rate limiting).
```

**Why it matters:** "Test the login" could mean 5 things. Spelling out exactly what you want removes ambiguity.

### 4. Output format -- how should the result look?

Specify the structure, format, or template for the response.

```
Use this format for each test case:
- Title (TC-XXX: descriptive name)
- Priority: High/Medium/Low
- Preconditions
- Steps (numbered)
- Expected result
```

**Why it matters:** Without a format, AI picks its own structure every time. You end up reformatting manually, or getting inconsistent outputs across sessions.

### Putting it all together

```
You are a senior QA engineer specializing in web application testing.

Context: We have a registration form at @src/pages/Register.tsx.
Fields: name (required, 2-50 chars), email (required, unique),
password (required, min 8 chars, must include uppercase and number).
The form submits to POST /api/users/register.

Task: Generate test cases for this registration form. Cover:
- Happy path (successful registration)
- Field validation (each field independently)
- Edge cases (boundary values, special characters, Unicode)
- Security (XSS, SQL injection)
- API error handling (409 conflict for duplicate email, 500)

Format: Use markdown table with columns:
ID | Title | Priority | Type | Steps | Expected Result
```

---

## 3 Examples: weak prompt vs good prompt

### Example 1: Generating test cases

**Weak prompt:**
```
Write tests for the shopping cart.
```

**Good prompt:**
```
You are a QA engineer testing an e-commerce application.

Context: The shopping cart (@src/components/Cart.tsx) supports:
- Adding/removing items
- Quantity updates (1-99 per item, max 20 unique items)
- Discount codes (applied via POST /api/cart/discount)
- Guest and authenticated users (guests stored in localStorage)

Task: Generate test cases for the cart. Group them by:
1. Happy path (add, remove, update, checkout)
2. Boundary values (quantity limits, max items)
3. Negative scenarios (invalid discount, empty cart checkout, expired items)
4. State management (guest vs logged-in, cart persistence after refresh)

Format each test case as:
**TC-XXX: Title**
Priority: High/Medium/Low
Preconditions: ...
Steps: (numbered)
Expected: ...
```

**What changed and why:**
| Aspect | Weak | Good |
|--------|------|------|
| Role | None -- AI doesn't know the perspective | QA engineer -- focuses on testing, not dev |
| Context | "shopping cart" -- which one? what features? | Specific component, features, constraints, API |
| Task | "write tests" -- what kind? how many? | Categorized scenarios with clear scope |
| Format | None -- AI picks randomly | Defined template for consistent output |

The weak prompt might give you 5 generic test ideas. The good prompt produces a structured, comprehensive test suite you can actually use.

---

### Example 2: Reviewing a Playwright test

**Weak prompt:**
```
Is this test ok?
```

**Good prompt:**
```
Review this Playwright test file @tests/checkout.spec.ts for:

1. Flaky patterns (hard waits, race conditions, non-deterministic selectors)
2. Best practices (web-first assertions, proper locators, auto-waiting)
3. Missing scenarios (what isn't tested but should be?)
4. Maintainability (magic strings, duplicated setup, missing Page Object)

For each issue found, explain:
- What the problem is
- Why it matters
- How to fix it (with code example)
```

**What changed and why:**
| Aspect | Weak | Good |
|--------|------|------|
| Context | "this test" -- AI may not even know which file | Specific file attached with `@` |
| Task | "is it ok" -- ok by what standard? | Four explicit review criteria |
| Format | None | Structured findings with problem/impact/fix |

"Is this test ok?" gets a superficial "looks fine." The good prompt gets a detailed code review with actionable improvements.

---

### Example 3: Writing a bug report

**Weak prompt:**
```
The search doesn't work, write a bug report.
```

**Good prompt:**
```
Help me write a bug report for the following issue:

Environment: Production (app.example.com), Chrome 120, macOS
User role: Authenticated standard user

What happened:
1. I went to /products and typed "wireless keyboard" in the search bar
2. Clicked the search button
3. Page shows "0 results found"
4. But directly querying GET /api/products?q=wireless+keyboard
   returns 12 results

Expected: Search results should display the 12 matching products.

Additional info: The issue started after yesterday's deploy (v2.4.1).
Search works for single-word queries. Fails for multi-word queries.

Format the bug report with:
- Clear title (short, specific)
- Severity and priority
- Environment details
- Steps to reproduce (numbered)
- Expected vs actual result
- Additional context and possible root cause
```

**What changed and why:**
| Aspect | Weak | Good |
|--------|------|------|
| Context | "search doesn't work" -- no details at all | Environment, steps, API comparison, deploy info |
| Task | "write a bug report" -- from what information? | Specific data points provided for AI to structure |
| Format | None | Explicit bug report sections requested |

The weak prompt forces AI to invent details (hallucinate). The good prompt gives AI real observations to organize into a professional report.

---

## Techniques

### Few-shot prompting

Give AI examples of the output you want before asking for the actual task. This is the most reliable way to control output format and quality.

**When to use:** When you need consistent formatting across multiple outputs (test cases, bug reports, test data).

```
Here are two test cases in our team's format:

**TC-001: Successful login with valid credentials**
Priority: High | Type: Happy path
Pre: User account exists (test@example.com / ValidPass123)
Steps:
  1. Navigate to /login
  2. Enter email: test@example.com
  3. Enter password: ValidPass123
  4. Click "Sign In"
Expected: Redirect to /dashboard, welcome message displayed

**TC-002: Login rejected with wrong password**
Priority: High | Type: Negative
Pre: User account exists (test@example.com / ValidPass123)
Steps:
  1. Navigate to /login
  2. Enter email: test@example.com
  3. Enter password: WrongPassword
  4. Click "Sign In"
Expected: Error "Invalid credentials" displayed, remain on /login

Now generate test cases for the password reset feature
using the same format. The reset flow: user clicks "Forgot password",
enters email, receives a link, clicks it, sets new password.
```

**Why it works:** AI learns your exact format, terminology, and level of detail from the examples. No need to describe the format separately -- the examples *are* the specification.

---

### Chain of Thought (CoT)

Ask AI to reason step by step instead of jumping to conclusions. This significantly improves quality for complex analytical tasks.

**When to use:** Analyzing requirements, finding gaps in test coverage, debugging test failures, risk assessment.

```
Analyze the requirements for the payment feature step by step:

1. First, list all the inputs and their constraints
2. For each input, identify boundary values
3. Identify all the state transitions (pending -> processing -> completed/failed)
4. List external dependencies (payment gateway, fraud check)
5. For each dependency, describe what happens when it fails
6. Based on steps 1-5, suggest test scenarios grouped by risk level

Requirements: @docs/payment-feature.md
Current tests: @tests/payment.spec.ts
```

**Why it works:** Forcing AI to think in steps prevents it from skipping edge cases. Each step builds on the previous one, producing thorough analysis rather than surface-level output.

**Tip:** You can combine CoT with a follow-up: "Now look at step 4 again -- what happens if the fraud check times out after the payment gateway already charged the card?"

---

### Context with @ (files)

In OpenCode, the `@` symbol attaches files, folders, or URLs directly to your prompt. This is the simplest way to give AI accurate context instead of copy-pasting or describing code from memory.

**Attaching specific files:**
```
Review @tests/login.spec.ts against the component @src/pages/Login.tsx
and tell me what scenarios are missing.
```

**Attaching folders:**
```
Look at all test files in @tests/e2e/ and identify patterns
of flaky tests (hard waits, race conditions, brittle selectors).
```

**Why it matters:** AI responses are only as good as the context they receive. Referring to files by `@` path ensures AI works with your real code, not imagined code. This dramatically reduces hallucinations.

**Practical tips:**
- Attach the code under test AND the test file when asking for review
- Attach requirements docs when generating test cases
- Attach error logs when debugging test failures
- Don't attach everything -- be selective, focus on what's relevant to the task

---

## Common mistakes

### 1. Too general -- no specific task

**Bad:**
```
Help me with testing.
```

**Why it fails:** AI doesn't know what to test, how to test, or what kind of help you need. You'll get a generic essay about testing fundamentals.

**Fix:** State the specific action, the specific feature, and what "done" looks like.

```
Generate boundary-value test cases for the age field
on the registration form. The field accepts integers 18-120.
```

---

### 2. No context -- AI has to guess

**Bad:**
```
Write Playwright tests for the dashboard.
```

**Why it fails:** AI doesn't know what's on the dashboard, what technology it uses, what the selectors look like, or what the expected behaviors are. It will invent plausible-sounding but wrong selectors and assertions.

**Fix:** Provide the actual code or describe the specific elements and behaviors.

```
Write Playwright tests for the dashboard @src/pages/Dashboard.tsx.
The dashboard shows: user stats (cards), recent activity (table),
and a notification bell. Use data-testid attributes for locators
(they follow the pattern data-testid="dashboard-{element}").
```

---

### 3. No output format -- inconsistent results

**Bad:**
```
Give me test scenarios for the search feature.
```

**Why it fails:** Every time you run this prompt, you get a different structure. Sometimes bullet points, sometimes numbered lists, sometimes prose. You waste time reformatting.

**Fix:** Specify the exact format you want.

```
Give me test scenarios for the search feature. Use this format:

| # | Scenario | Type | Input | Expected Result |
|---|----------|------|-------|-----------------|
```

---

### 4. Multiple unrelated tasks in one prompt

**Bad:**
```
Write tests for login, review the checkout code, generate test data
for the user table, and explain how our CI pipeline works.
```

**Why it fails:** AI tries to address everything at once, giving shallow answers to each. Context gets diluted across unrelated topics.

**Fix:** One task per prompt. If tasks are related, chain them. If they're independent, use separate sessions.

```
Session 1: Write tests for login
Session 2: Review the checkout code
Session 3: Generate test data for the user table
```

---

### 5. Accepting output without verification

This isn't a prompt mistake -- it's a workflow mistake. AI-generated tests can look professional but contain:
- Selectors that don't exist in the DOM
- Assertions that always pass (e.g., checking a value is truthy when it's always truthy)
- Missing awaits that cause race conditions
- Test data that violates actual business rules

**Fix:** Always run generated tests. Always review generated test cases against real requirements. Treat AI output as a draft, not a finished product.

---

## Quick reference: prompt checklist

Before sending a prompt, check:

- [ ] **Role** -- Did I tell AI what perspective to use?
- [ ] **Context** -- Did I attach relevant files with `@` or describe the feature?
- [ ] **Task** -- Is the task specific and actionable?
- [ ] **Format** -- Did I specify how I want the output structured?
- [ ] **Scope** -- Is this one focused task, not five tasks mashed together?
- [ ] **Verification** -- Do I have a plan to verify the output?

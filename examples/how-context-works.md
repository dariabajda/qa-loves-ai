# How AI Context Works -- Practical Guide for QA Engineers

Why a new OpenCode session (fresh context) is usually better than continuing a long conversation, and when to break that rule.

---

## How context works

Think of an AI session as a **whiteboard with a fixed size**. Everything that happens in the conversation goes on that whiteboard:

- Your messages
- AI's responses
- Every file AI reads
- Every bash command output
- System prompts, AGENTS.md, tool descriptions

The whiteboard has a hard limit (e.g., 200k tokens for Claude). Once it fills up, things start getting **compacted** -- OpenCode summarizes older parts of the conversation to make room. That summary loses detail. It's like someone erasing half the whiteboard and writing a quick sticky note about what was there.

---

## Why long sessions degrade

Three things happen as a session grows:

**1. Signal-to-noise ratio drops.** If you started by discussing API tests, then switched to Playwright locators, then asked about a bug report -- AI carries all of that context. When you ask your next question, AI is "thinking about" 50 topics instead of 1. Responses become less focused.

**2. Compaction loses information.** When the context gets too long, OpenCode's compaction agent summarizes it. That summary is good but imperfect. Specific details -- exact file paths, variable names, the nuance of your earlier decision -- can get lost.

**3. Earlier mistakes compound.** If AI misunderstood something early in the session, that misunderstanding stays in context and influences everything after it. A fresh session eliminates that drift.

---

## When to start a new session

**Start fresh when:**
- You're switching to a **different task** (writing tests -> reviewing a PR -> generating test data)
- The current session has gone through **10+ back-and-forth exchanges**
- AI starts giving **repetitive or off-track** responses
- You've **committed your changes** and are moving to the next step
- You're about to work on a **different file or feature area**

**Continue the same session when:**
- You're **iterating on the same thing** (e.g., AI wrote a test, you want it adjusted)
- You need AI to remember a **decision you just made** ("yes, use that approach, now apply it to the next file")
- You're in the middle of a **multi-file change** that's logically one unit of work
- AI read several files to build context that would be **expensive to rebuild**

---

## Practical rules for daily QA work in OpenCode

### Rule 1: One session = one goal
Before you start, define your goal in one sentence. Examples:
- "Write test cases for the checkout flow"
- "Review this PR for missing edge cases"
- "Debug the flaky login test"

If you can't state it in one sentence, break it into multiple sessions.

### Rule 2: Front-load context
At the **beginning** of a session, give AI everything it needs. Use `@` to reference files:
```
Review @src/tests/checkout.spec.ts for missing edge cases.
The feature requirements are in @docs/checkout-requirements.md.
We use Page Object Model -- see @src/tests/pages/CheckoutPage.ts.
```
This is far better than drip-feeding files mid-conversation.

### Rule 3: Use Plan mode for thinking, Build mode for doing
- **Plan mode (Tab):** "What test scenarios should I cover for this feature?" -- AI thinks with you, no files changed
- **Build mode (Tab):** "Create the Playwright test file for these scenarios" -- AI makes the changes

You can start in Plan, discuss, then switch to Build in the same session. That's a valid workflow because it's still one goal.

### Rule 4: Commit and close
When AI finishes a task and you're happy with the result:
1. Review the changes
2. Commit
3. Close the session
4. Open a new one for the next task

Don't be tempted to say "while you're at it, also fix..." -- that's where quality degrades.

### Rule 5: The @PLAN.md pattern
This is what your PLAN.md is designed for. Each new session starts with:
```
Read @PLAN.md, section "Step 2.3". [then the task]
```
AI immediately gets the big picture (from PLAN.md) plus the specific task. Fresh context, focused goal, good results.

### Rule 6: Expensive context = stay in session
If AI spent time reading 10+ files to understand a complex flow, don't throw that away for a small follow-up question. The cost of rebuilding that context is higher than the cost of a slightly longer session.

---

## Summary

| | Fresh session | Continue session |
|---|---|---|
| **Context** | Clean, focused | Accumulated, may be noisy |
| **Best for** | New task, different feature | Iterating on same task |
| **Risk** | Losing previous decisions | Quality degradation |
| **Rule of thumb** | Default choice | Only when context reuse justifies it |

The key insight: **starting fresh is cheap, recovering from a polluted context is expensive.** When in doubt, open a new session.

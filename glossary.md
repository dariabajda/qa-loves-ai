# AI Glossary for QA Engineers

A practical guide to AI terminology you'll encounter working with tools like OpenCode, Cursor, Copilot, and other AI coding agents.

---

## Core concepts

### Prompt
The text (instruction) you send to an AI model. It can be simple ("write a test") or complex, multi-step with rich context.

**QA examples:**
- "Generate test cases for the registration form"
- "Analyze this endpoint and suggest negative scenarios"
- "Review this Playwright test for flaky patterns"

### System prompt
A hidden instruction that defines *how* AI should behave. You don't see it in the chat, but it shapes every response. In OpenCode, you set system prompts through AGENTS.md or agent configuration.

**QA analogy:** Think of it as the "Definition of Done" for AI -- it sets expectations before any work begins.

### Context
Everything AI "sees" in a given conversation: your messages, files it has read, tool outputs, system prompts. AI has no memory between sessions -- context is *all* it knows.

**Critical for QA:** The better context you provide (code, requirements, logs, error messages), the better the results. Saying "test the login" is vague. Saying "test the login form at @src/pages/Login.tsx that uses OAuth2 with these validation rules: ..." gives AI what it needs.

### Token
A unit of text for AI -- roughly 3/4 of a word in English. Models have token limits (e.g., 200k tokens for Claude) -- this determines how much text AI can "read" at once.

**Why QA cares:** If you paste a huge test suite + entire codebase, you may hit the limit. Be selective with context.

### Model (LLM -- Large Language Model)
An AI program trained on massive amounts of text and code. Examples: Claude (Anthropic), GPT (OpenAI), Gemini (Google). Different models have different strengths -- some are better at code, others at analysis or reasoning.

**QA analogy:** Like choosing the right testing tool for the job -- you pick the model that fits your task.

---

## OpenCode concepts

### Agent
A specialized AI assistant with a defined role, set of tools, and permissions.

**QA analogy:** Think of agents as different team members:
- **Build** -- developer role: can edit files, run commands, make changes
- **Plan** -- analyst role: can only read and suggest (does not change code)
- You can create your own, e.g., "test-reviewer" that analyzes tests for coverage gaps

**Types of agents:**
- **Primary agent** -- the main agent you talk to (switch with Tab key)
- **Subagent** -- a specialist invoked by the primary agent or by you via @mention

**How to use:**
- Press **Tab** to switch between primary agents (Build / Plan)
- Type `@test-reviewer` in your message to invoke a subagent
- Agents are defined as `.md` files in `.opencode/agents/`

### Skill
A `SKILL.md` file containing instructions that an agent can load on demand. A skill is a "recipe" -- the agent sees a list of available skills and loads the right one when needed.

**QA analogy:** A skill is like a test procedure or checklist that a tester pulls out for a specific type of task. You don't read all checklists at once -- you grab the one relevant to what you're testing right now.

**How it works:**
1. You create `.opencode/skills/test-case-writer/SKILL.md`
2. Agent sees it listed as available
3. When the task matches, agent loads it and follows the instructions

### Agent vs Skill -- the key difference
| | Agent | Skill |
|---|---|---|
| **What it is** | A persona with tools and permissions | A set of instructions loaded on demand |
| **Persistence** | Active throughout the session | Loaded only when needed |
| **Has tools?** | Yes (read, write, bash, etc.) | No -- it's just instructions *for* an agent |
| **Analogy** | A team member with a specific role | A procedure document that team member follows |

### MCP (Model Context Protocol)
An open protocol that lets AI connect to external tools and services. Through MCP, an agent can search Jira, check Sentry errors, fetch documentation, and more.

**Examples useful for QA:**
- **Sentry MCP** -- agent can check production errors and stack traces
- **Context7** -- agent searches library documentation (e.g., Playwright docs)
- **Grep by Vercel** -- agent searches code examples on GitHub

**QA analogy:** MCP is like a plugin/integration -- it extends what the agent can do by connecting to external data sources. Just like you might connect your test framework to Jira for issue tracking.

**How to configure:** Add MCP servers in `opencode.json` under the `"mcp"` key.

### Command
A saved prompt template that you run by typing `/name`. Commands can accept arguments.

**Example:** `/test-plan login-form` -- generates a test plan for the specified feature.

**How to create:** Add `.md` files in `.opencode/commands/`. The filename becomes the command name.

### Rules (AGENTS.md)
A file with project-specific instructions for AI. AI reads it at the start of every conversation, so it always knows about your project.

**QA analogy:** This is your onboarding document for a new team member -- it describes the project structure, conventions, tech stack, and how things work.

**Where to place:**
- Project-level: `AGENTS.md` in the project root (committed to git, shared with team)
- Global: `~/.config/opencode/AGENTS.md` (personal rules, applied to all projects)

### Tools
Built-in actions that an agent can perform: reading files, editing code, running bash commands, searching, fetching web pages. MCP servers add extra tools.

**Key tools in OpenCode:**
| Tool | What it does | QA use case |
|------|-------------|-------------|
| Read | Reads file contents | Analyze test files, code under test |
| Edit | Modifies files | Fix tests, update locators |
| Bash | Runs terminal commands | Run tests, check git status |
| Glob | Finds files by pattern | Find all test files (`**/*.spec.ts`) |
| Grep | Searches file contents | Find all usages of a function |
| WebFetch | Fetches web pages | Check API docs, pull requirements |
| Task | Launches a subagent | Delegate complex research to a specialist |

---

## General AI concepts

### RAG (Retrieval-Augmented Generation)
A technique where AI first retrieves relevant information (e.g., from docs, database, codebase) and then generates a response based on that data.

**Example:** Instead of asking "how does our login work?" (and getting a generic answer), RAG first pulls your actual auth code and AI responds based on real data.

**QA relevance:** OpenCode does this naturally -- when it reads your files before answering, that's RAG in action.

### Hallucination
When AI generates information that looks plausible but is factually wrong.

**CRITICAL for QA -- common hallucinations to watch for:**
- Invented CSS selectors or locators that don't exist in the DOM
- Non-existent API endpoints or response fields
- Made-up function names or import paths
- Incorrect assertions that would always pass
- Fabricated test data that doesn't match real constraints

**Rule of thumb:** Always verify generated tests against the actual application. If AI wrote a locator like `data-testid="submit-btn"`, check that it actually exists.

### Temperature
A parameter controlling AI "creativity." Low (0.0-0.2) = deterministic, repeatable responses. High (0.7-1.0) = more creative, less predictable.

**For QA:**
- **Low temperature** for generating tests (you want precision and consistency)
- **Higher temperature** for brainstorming test scenarios or edge cases

### Fine-tuning
Additional training of an AI model on specific data. This is NOT the same as prompting -- fine-tuning changes the model itself. You typically don't do this as a QA engineer; you work with pre-trained models and guide them with prompts.

### Few-shot prompting
A technique where you give AI a few examples of the expected output before asking for the actual task.

**QA example:**
```
Here are 2 test cases in our format:

**TC-001: Successful login**
Priority: High
Preconditions: Valid user account exists
Steps: 1. Navigate to /login  2. Enter valid email  3. Enter valid password  4. Click Submit
Expected: User is redirected to dashboard

**TC-002: Login with invalid password**
Priority: High
Preconditions: Valid user account exists
Steps: 1. Navigate to /login  2. Enter valid email  3. Enter wrong password  4. Click Submit
Expected: Error message "Invalid credentials" is displayed

Now generate test cases for the password reset feature in the same format.
```

### Chain of Thought (CoT)
A technique where you ask AI to show its reasoning step by step. This improves the quality of complex responses.

**QA example:** "Analyze this endpoint step by step: first identify all input parameters, then for each parameter list possible invalid values, then identify boundary conditions, then suggest test scenarios."

### Agentic workflow
An approach where AI doesn't just answer questions but actively performs tasks: reads files, runs commands, writes code, iterates on results. OpenCode works this way -- it's not a simple chatbot.

**The difference from ChatGPT/Copilot Chat:**
| | Regular chat (ChatGPT) | Agentic (OpenCode) |
|---|---|---|
| Reads your files | No (you paste manually) | Yes (reads from your repo) |
| Runs commands | No | Yes (e.g., `npx playwright test`) |
| Edits code | No (gives you text to copy) | Yes (directly modifies files) |
| Multi-step tasks | One response at a time | Plans and executes multiple steps |
| Context | Only what you paste | Your entire project |

---

## Comparison table

| Concept | What it is | Persistence | QA analogy |
|---------|-----------|-------------|------------|
| **Prompt** | A single instruction to AI | One-time | A question or task you give someone |
| **System prompt** | Hidden behavioral instruction | Per session | Definition of Done / working agreement |
| **Agent** | AI with a role + tools + permissions | Per session | Team member with defined responsibilities |
| **Skill** | Instructions loaded on demand | On demand | Test procedure / checklist |
| **MCP** | Protocol connecting AI to external services | Configured once | Plugin / integration (like Jira connector) |
| **Command** | Saved prompt template with a name | Reusable | Macro / shortcut for a repetitive task |
| **Rules** | Permanent project instructions (AGENTS.md) | Always loaded | Onboarding doc / coding standards |
| **Tools** | Actions an agent can perform | Always available | Capabilities (read, write, execute) |

---

## Quick reference: where things live in OpenCode

```
your-project/
  AGENTS.md                          # Project rules (always loaded)
  opencode.json                      # Configuration (MCP, tools, agents)
  .opencode/
    agents/                          # Custom agents
      test-reviewer.md
    skills/                          # Custom skills
      test-case-writer/SKILL.md
    commands/                        # Custom commands
      test-plan.md

~/.config/opencode/
  AGENTS.md                          # Global rules (all projects)
  opencode.json                      # Global configuration
  agents/                            # Global agents
  skills/                            # Global skills
  commands/                          # Global commands
```

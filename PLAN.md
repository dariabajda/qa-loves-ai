# QA Loves AI -- Learning Plan & Repository Roadmap

## How to use this plan

Each step is a separate OpenCode session (new window = fresh context).
Why? Because AI works best with a clear, focused goal.
Don't try to do everything at once.

**Workflow for each step:**
1. Open a new OpenCode session in this repository
2. Paste the task prompt (or reference this file: `@PLAN.md`)
3. Work with AI on one topic
4. Review the results, commit, close the session

**Tip:** Use **Plan** mode (Tab) when you want to learn and discuss.
Switch to **Build** mode (Tab) when you want AI to create files.

---

## Topic map

```
1. FOUNDATIONS            2. OPENCODE IN PRACTICE    3. AI IN QA WORK
   |                        |                          |
   |- AI glossary           |- Project configuration   |- Test case prompts
   |- How agents vs         |- Agents for QA           |- Bug report prompts
   |  skills vs MCP work    |- Skills (SKILL.md)       |- Test data generation
   |- Fresh context         |- Commands (/slash)       |- Code / test review
   |  and good prompts      |- MCP servers             |- Requirements analysis
   |                        |- AGENTS.md               |- API testing
   |                        |                          |- Playwright testing
   |                        |                          |
   4. AI IN THE QUALITY PROCESS (holistic view)
      |
      |- AI in refinement / planning
      |- AI in code review
      |- AI in monitoring and observability
      |- AI in CI/CD
      |- AI in documentation
      |- Risks and limitations of AI
```

---

## Phase 1: Foundations

Goal: Understand key concepts and learn to communicate effectively with AI.

### Step 1.1 -- AI Glossary
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 1.1".
Create a file glossary.md -- an AI glossary for QA Engineers.
Terms to explain (with analogies to QA work):
- Prompt, System Prompt, Context, Token, Model/LLM
- Agent, Skill, MCP, Command, Rules/AGENTS.md, Tools
- RAG, Hallucination, Temperature, Few-shot, Chain of Thought
- Agentic workflow vs regular chat
Add a comparison table: agent vs skill vs MCP vs command vs prompt.
Write clearly, avoid unnecessary jargon. Use analogies to QA work.
```

### Step 1.2 -- How fresh context works and why it matters
> Session: ~15 min | Mode: Plan (conversation only)

Paste into OpenCode:
```
Explain how context works in AI -- why a new window/session is a
better approach than continuing one long conversation.
How does this affect response quality? When is it worth continuing
a session, and when should you start a new one? Give practical rules
for someone using OpenCode for daily QA work.
```

### Step 1.3 -- Anatomy of a good prompt
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 1.3".
Create a file prompts/how-to-prompt.md -- a guide to writing
good prompts, aimed at a QA Engineer.
Include:
- Structure of a good prompt (role, context, task, output format)
- 3 examples: weak prompt -> good prompt (with explanation of differences)
- Techniques: few-shot, chain of thought, context with @ (files)
- Common mistakes (too general questions, no context, no format)
```

---

## Phase 2: OpenCode in practice

Goal: Configure OpenCode for QA work in this repository.

### Step 2.1 -- AGENTS.md for this repository
> Session: ~10 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 2.1".
Create an AGENTS.md file for this repository.
This is an educational repository "qa-loves-ai" -- it collects examples
of prompts, agents, skills, and OpenCode configurations useful
for QA Engineers. Stack: fullstack (web + API), tests in Playwright.
AGENTS.md should explain to AI the purpose and structure of this repo.
```

### Step 2.2 -- First agent: test-reviewer
> Session: ~15 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 2.2".
Create a "test-reviewer" agent as .opencode/agents/test-reviewer.md.
The agent should:
- Be a subagent (invoked via @test-reviewer)
- Have no file editing permissions (read-only)
- Analyze tests for: scenario coverage, edge cases,
  readability, Playwright best practices
- Have temperature 0.1 (precise responses)
Also explain in a comment how to use it afterwards.
```

### Step 2.3 -- First skill: writing test cases
> Session: ~15 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 2.3".
Create a skill "test-case-writer" in .opencode/skills/test-case-writer/SKILL.md.
The skill should instruct AI how to write test cases in a defined format:
- Title, Priority, Preconditions, Steps, Expected Result
- Distinguishing happy path, edge cases, negative scenarios
- Markdown format
Also add examples in SKILL.md.
```

### Step 2.4 -- First command: /test-plan
> Session: ~15 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 2.4".
Create a /test-plan command in .opencode/commands/test-plan.md.
The command should accept a feature name as an argument
and generate a test plan (scenarios, priorities, approach).
Use $ARGUMENTS in the template.
Also create a /bug-report command that helps write a good bug report.
```

### Step 2.5 -- MCP servers useful for QA
> Session: ~15 min | Mode: Plan (conversation) + Build (configuration)

Paste into OpenCode:
```
Read @PLAN.md, section "Step 2.5".
Explain which MCP servers can be useful for QA work.
Then create a sample opencode.json file with configuration
for useful MCP servers (Context7 for docs, etc.).
Add comments explaining what each server does.
```

---

## Phase 3: AI in daily QA work

Goal: Build a library of ready-to-use prompts and examples.

### Step 3.1 -- Prompts for test cases and scenarios
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 3.1".
Create a file prompts/test-cases.md with ready-to-use prompts for:
- Generating test cases from requirements/user stories
- Generating negative/edge case scenarios
- Analyzing test coverage (did I miss something?)
- Generating test data
Each prompt should have: description, template, usage example.
Prompts should be ready to paste/adapt.
```

### Step 3.2 -- Prompts for Playwright tests
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 3.2".
Create a file prompts/playwright.md with prompts for:
- Writing e2e tests in Playwright (with best practices)
- Refactoring existing tests
- Generating Page Object Model
- Debugging flaky tests
- Generating test fixtures and helpers
Include Playwright best practices: locators, auto-waiting,
web-first assertions. Each prompt with template and example.
```

### Step 3.3 -- Prompts for API testing
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 3.3".
Create a file prompts/api-testing.md with prompts for:
- Analyzing an endpoint (test scenarios from OpenAPI/Swagger spec)
- Generating API tests (Playwright API testing or other)
- Response schema validation
- Testing authentication/authorization
- Testing edge cases (limits, empty data, formats)
Each prompt with template and example.
```

### Step 3.4 -- Prompts for bug reports and communication
> Session: ~15 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 3.4".
Create a file prompts/bug-reports-and-communication.md with prompts for:
- Writing bug reports (clear title, steps, expected/actual)
- Improving an existing bug report
- Writing test summary reports
- Writing release notes from a QA perspective
- Communicating with developers (explaining why something is a bug)
```

### Step 3.5 -- Requirements analysis and review
> Session: ~15 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 3.5".
Create a file prompts/requirements-review.md with prompts for:
- Analyzing a user story for testability
- Finding gaps and ambiguities in requirements
- Generating questions for refinement/grooming
- Reviewing PRs from a QA perspective
```

---

## Phase 4: AI in the quality process -- holistic view

Goal: Understand where AI can support quality at every stage.

### Step 4.1 -- Map of AI applications in the software lifecycle
> Session: ~20 min | Mode: Plan (conversation)

Paste into OpenCode:
```
Read @PLAN.md, section "Step 4.1".
Let's discuss where AI can support software quality
-- not just in testing, but across the entire process. I'm interested in:

1. Planning / refinement -- how AI helps analyze requirements
2. Development -- how AI supports code quality (unit tests,
   code review, pair programming with AI)
3. Testing -- manual, automation, exploratory
4. CI/CD -- analyzing results, flaky tests, regression
5. Monitoring/Production -- analyzing logs, alerts, Sentry
6. Documentation -- keeping docs up to date, generation

For each stage, provide concrete examples and tools.
I want to save this to a file in the repo afterwards.
```

After the conversation, in a new session (Build), save results to `examples/ai-in-quality-process.md`.

### Step 4.2 -- Risks and limitations of AI in QA
> Session: ~15 min | Mode: Plan (conversation) + Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 4.2".
Create a file examples/risks-and-limitations.md about AI risks in QA:
- Hallucinations -- AI invents selectors, endpoints, behaviors
- False confidence -- tests look good but don't test what they should
- Bias -- AI generates only "happy path" unless you ask for more
- Security -- what should NOT be sent to AI (production data, secrets)
- When AI hinders rather than helps
- How to verify AI outputs
Provide practical rules as a "QA safety checklist" for working with AI.
```

### Step 4.3 -- More agents and skills
> Session: ~20 min | Mode: Build

Paste into OpenCode:
```
Read @PLAN.md, section "Step 4.3".
Create additional agents and skills:

Agents (.opencode/agents/):
- api-analyzer.md -- analyzes endpoints, suggests test scenarios
- bug-hunter.md -- looks for potential bugs in code

Skills (.opencode/skills/):
- bug-reporter/SKILL.md -- formats bug reports
- api-test-gen/SKILL.md -- generates API tests
- playwright-helper/SKILL.md -- helps with Playwright (locators, POM, fixtures)

Each with a comment on when and how to use it.
```

---

## Phase 5: Growth and inspiration (ongoing)

### Ideas for future sessions
- Create a custom skill specific to your new project
- Test the test-reviewer agent on real code
- Add a Sentry MCP server and analyze production errors
- Run an experiment: same test written manually vs with AI -- compare quality
- Read another project's AGENTS.md for inspiration

### Sources of inspiration
- OpenCode Discord: https://opencode.ai/discord
- OpenCode docs: https://opencode.ai/docs
- Awesome MCP Servers: https://github.com/punkpeye/awesome-mcp-servers
- Prompt engineering: https://www.promptingguide.ai/

---

## Target repository structure

```
qa-loves-ai/
  PLAN.md                  <-- this file (learning plan)
  AGENTS.md                <-- project rules for AI
  glossary.md              <-- AI glossary
  opencode.json            <-- OpenCode configuration (MCP etc.)
  prompts/
    how-to-prompt.md       <-- how to write good prompts
    test-cases.md          <-- prompts for test cases
    playwright.md          <-- prompts for Playwright
    api-testing.md         <-- prompts for API testing
    bug-reports-and-communication.md
    requirements-review.md
  examples/
    ai-in-quality-process.md
    risks-and-limitations.md
  .opencode/
    agents/
      test-reviewer.md
      api-analyzer.md
      bug-hunter.md
    skills/
      test-case-writer/SKILL.md
      bug-reporter/SKILL.md
      api-test-gen/SKILL.md
      playwright-helper/SKILL.md
    commands/
      test-plan.md
      bug-report.md
```

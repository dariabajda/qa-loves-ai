# QA Loves AI

An educational repository that collects examples of prompts, agents, skills, and OpenCode configurations useful for QA Engineers.

## Purpose

This repo is a learning kit, not a production application. Each file teaches a specific concept related to using AI in QA work. The content is written for QA Engineers who want to integrate AI tools (OpenCode, Copilot, Cursor) into their daily workflow.

The learning plan is defined in `PLAN.md` -- it contains step-by-step sessions organized into phases.

## Repository structure

```
qa-loves-ai/
  PLAN.md                  -- learning plan and roadmap (start here)
  AGENTS.md                -- this file; project rules for AI
  glossary.md              -- AI terminology glossary for QA Engineers
  prompts/                 -- ready-to-use prompt templates
    how-to-prompt.md       -- guide to writing effective prompts
    test-cases.md          -- prompts for generating test cases
    playwright.md          -- prompts for Playwright e2e tests
    api-testing.md         -- prompts for API testing
    bug-reports-and-communication.md
    requirements-review.md
  examples/                -- guides, maps, and reference material
    how-context-works.md   -- why fresh AI context matters
    ai-in-quality-process.md
    risks-and-limitations.md
  .opencode/
    agents/                -- subagents (invoked via @agent-name)
    skills/                -- reusable skills (SKILL.md format)
    commands/              -- slash commands (/command-name)
```

## Tech stack (for context in examples)

- Fullstack: web frontend + REST API backend
- Test framework: Playwright (e2e tests, API tests)
- Language: TypeScript
- AI tool: OpenCode

## Writing conventions

- All content is Markdown
- Audience: QA Engineers, from junior to senior level
- Tone: practical, clear, no unnecessary jargon
- Use QA-specific analogies when explaining AI concepts
- Every prompt template includes: description, template, and usage example
- Prefer concrete examples over abstract theory

## When contributing or editing content

- Follow the structure defined in `PLAN.md` (target repository structure section)
- Keep files focused on a single topic
- Each prompt file should contain ready-to-paste templates, not just theory
- Test cases and examples should reference realistic QA scenarios (login forms, API endpoints, registration flows)
- Do not add production code or real application source files -- this repo is documentation only

## Playwright conventions (used in examples)

- Use web-first assertions (`expect(locator).toBeVisible()`, not `waitForSelector`)
- Prefer user-facing locators (`getByRole`, `getByText`, `getByLabel`) over CSS/XPath
- Use the Page Object Model pattern for organizing test code
- Tests should be independent and not rely on execution order
- Use `test.describe` for grouping related scenarios

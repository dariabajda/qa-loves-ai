# Project Rules Across AI Tools

AI coding tools read project-level instruction files to understand your codebase -- its structure, conventions, and how to behave. The concept is the same across tools, but each uses a different file.

---

## Comparison table

| Tool | File | Location | Shared via git |
|------|------|----------|----------------|
| **OpenCode** | `AGENTS.md` | Project root | Yes |
| **Claude Code** | `CLAUDE.md` | Project root | Yes |
| **GitHub Copilot** | `AGENTS.md`, `CLAUDE.md`, or `GEMINI.md` | Project root | Yes |
| **GitHub Copilot** | `.github/copilot-instructions.md` | `.github/` directory | Yes |
| **Cursor** | `.cursorrules` or `.cursor/rules/*.md` | Project root / `.cursor/` | Yes |

---

## What each tool supports

### OpenCode

- Reads `AGENTS.md` from the project root (primary)
- Falls back to `CLAUDE.md` if no `AGENTS.md` exists
- Also supports global rules at `~/.config/opencode/AGENTS.md`
- Additional instruction files can be listed in `opencode.json` via the `instructions` field

### GitHub Copilot (VS Code)

- Reads `AGENTS.md` from the project root (used by Copilot coding agent)
- Also supports `CLAUDE.md` and `GEMINI.md` as alternatives
- Has its own format: `.github/copilot-instructions.md` for repo-wide instructions
- Supports path-specific rules via `.github/instructions/NAME.instructions.md` files
- All types can be active at the same time -- they are combined

### Claude Code

- Reads `CLAUDE.md` from the project root
- The original format that started this convention
- OpenCode and GitHub Copilot both support it as a fallback

### Cursor

- Uses `.cursorrules` file in the project root (legacy)
- Newer format: `.cursor/rules/*.md` files
- Does not read `AGENTS.md` or `CLAUDE.md`

---

## Practical takeaway

If your team uses multiple AI tools, `AGENTS.md` gives you the broadest coverage -- it works with OpenCode, GitHub Copilot, and (via fallback logic) Claude Code. If some teammates use Cursor, they will need a separate `.cursorrules` or `.cursor/rules/` file.

You do not need to choose one. You can have both `AGENTS.md` and `.github/copilot-instructions.md` in the same repo. They serve slightly different purposes:

- **`AGENTS.md`** -- general project context for AI agents (structure, conventions, stack)
- **`.github/copilot-instructions.md`** -- Copilot-specific behavior (code style preferences, review rules)

When in doubt, start with `AGENTS.md`. It is the most portable option today.

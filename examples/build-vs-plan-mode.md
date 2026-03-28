# Build vs Plan Mode in OpenCode

## What is the actual difference?

The only real difference between Build and Plan is **permissions** -- which tools the agent is allowed to use without asking you first.

| Capability | Build | Plan |
|---|---|---|
| File edits (write, edit, patch) | **allow** (runs immediately) | **ask** (prompts for approval) |
| Bash commands | **allow** (runs immediately) | **ask** (prompts for approval) |
| Reading files, searching code | allow | allow |
| Reasoning, analysis, planning | same model, same quality | same model, same quality |

Plan mode does not have a different "brain", a special planning algorithm, or a different model (unless you configure one). It is the same LLM, the same system prompt, and the same context window. The only thing that changes is that file modifications and shell commands require your explicit approval before they execute.

## Do I need Plan mode for planning?

No. You can stay in Build mode and say:

```
Plan the test strategy for the login feature. Don't make any changes,
just write the plan to examples/login-test-plan.md.
```

The LLM will follow your instructions. It will produce a plan and write it to a file -- nothing more. Build mode does not force the agent to make changes; it simply *allows* it to.

## When Plan mode is useful

- **Safety net in unfamiliar codebases.** Even if the LLM decides to edit a file, Plan mode will block it (or ask for your approval) before anything changes on disk.
- **Exploratory conversations.** When you want to discuss architecture, review code, or brainstorm -- and you want a structural guarantee that nothing gets modified by accident.
- **Learning and onboarding.** When you are new to a project and want AI to explain things without risk of unintended side effects.
- **Code review.** Analyzing code and suggesting improvements without touching anything.

## When you can skip Plan mode

- You are confident in what you are asking and comfortable reviewing changes after the fact.
- You explicitly tell the agent what to do (or not do) in your prompt.
- You use `/undo` freely to revert unwanted changes.

## The recommended workflow from OpenCode docs

The official docs suggest a three-step pattern:

1. **Tab** to Plan mode -- describe the feature, iterate on the plan
2. **Tab** back to Build mode -- say "go ahead and make the changes"
3. Review and commit

This is a good workflow when you want to think before acting. But it is a suggested pattern, not a requirement. If you prefer a single-mode workflow in Build, that works too.

## Customizing Plan mode

Since Plan is just an agent with specific permissions, you can reconfigure it. For example, you could make Plan fully read-only (deny all edits and bash) instead of the default "ask":

```json
{
  "agent": {
    "plan": {
      "permission": {
        "edit": "deny",
        "bash": "deny"
      }
    }
  }
}
```

Or you could allow specific safe commands:

```json
{
  "agent": {
    "plan": {
      "permission": {
        "bash": {
          "*": "deny",
          "git log*": "allow",
          "git diff*": "allow"
        }
      }
    }
  }
}
```

## Summary

| Question | Answer |
|---|---|
| Is Plan mode required for planning? | No, you can plan in Build mode |
| Does Plan mode use a different model? | No, same model by default |
| What does Plan mode actually do? | Blocks or asks before file edits and bash |
| Can I just use Build mode for everything? | Yes, if you are comfortable with it |
| Is Plan mode useless then? | No -- it is a useful safety guardrail |

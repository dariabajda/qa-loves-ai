---
description: Commit and push all changes
---

Commit all current changes and push to the remote repository.

Here is the git diff of current changes:
!`git diff`
!`git diff --cached`
!`git status`

**Step 1: Sanity checks**

Before doing anything, scan the diff and working tree for problems. Report any issues found and ask me how to proceed before continuing.

- **Sensitive data**: Look for anything that resembles secrets, API keys, tokens, passwords, or credentials in the diff. Flag any matches.
- **Merge conflict markers**: Check for leftover `<<<<<<<`, `=======`, `>>>>>>>` in changed files.
- **Debug leftovers**: Look for `console.log`, `debugger`, `binding.pry`, or similar debug statements that look temporary. Also flag `TODO` or `FIXME` comments that were just added (not pre-existing).
- **Broken markdown links**: For any markdown files being committed, check that file references (e.g. `@file.md`, `[text](path)`, backtick paths like \`file.md\`) point to files that actually exist in the repo.
- **Untracked files**: If `git status` shows untracked files, list them and ask if any should be included in this commit.

If all checks pass, say "All sanity checks passed" and move on.

**Step 2: Analyze whether changes should be split**

Review the diff and check if the changes touch multiple unrelated concerns (e.g. different features, a refactor mixed with a bug fix, config changes alongside new content). If so:
- Suggest how to split them into separate, focused commits
- List each proposed commit with the files it would include and a suggested message
- Ask me to confirm the split before proceeding

If all changes are related to a single concern, say so briefly and move on.

**Step 3: Commit message**

If a commit message was provided below, use it directly.
If no message was provided (empty), then:
1. Generate a concise commit message (1-2 sentences, focus on "why" not "what")
2. Show me the proposed message and ask for confirmation before committing

Commit message: $ARGUMENTS

**Step 4: Commit and push**

1. Stage the relevant files (`git add`)
2. Commit with the final message
3. If splitting, repeat for each commit
4. Push to the remote

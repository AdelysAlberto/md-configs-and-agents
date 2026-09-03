---
name: commit
description: Analyzes git changes (staged or working tree), automatically extracts the Ticket ID from the branch (#ID), generates a commit using Conventional Commits and a structured technical description (summary + improvements/fixes) without listing files one by one.
---

# Skill: Smart Commit & Staging

This skill activates when the user types `/commit` or `/git-add`, or asks to prepare and record changes in Git.

## Execution instructions

1. **Check Git status and branch**:
   - Get the current branch: `git branch --show-current`.
   - Extract the 6-digit ticket ID from the branch name (example: if the branch is `feature/264099` or `264099-fix-grid`, the ID is `#264099`). If the branch does not contain a 6-digit numeric ID, omit the `#ID` prefix or ask for clarification only if strictly necessary.

2. **Stage changes**:
   - Check modified and staged files: `git status -s`.
   - If there are no staged files but there are changes in the working tree, run `git add .` (or add the relevant files for the requested change).

3. **Analyze the Diff**:
   - Inspect the differences with `git diff --cached`.
   - Evaluate refactors, new features, bug fixes, styles, or configurations.

4. **Draft the Commit Message**:
   - **Title Format (MANDATORY)**:
     - Must begin with the ID preceded by `#` followed by a space (e.g., `#264099 fix(DatePicker): fix timezone offset` or `#264099 feat(auth): add refresh token support`).
     - Follow the Conventional Commits convention: `type(scope): imperative and concise description`.
     - Valid types: `feat`, `fix`, `refactor`, `perf`, `style`, `test`, `docs`, `chore`.
   - **Commit Body (Structured Technical Description)**:
     - **Summary**: Conceptual synthesis of 2 to 4 lines of what was implemented or fixed.
     - **Strict rule**: **PROHIBITED to list files one by one** in the description.
     - **Improvements and Fixes**: Clean bulleted list of technical benefits, solved problems, performance optimizations, or adjusted API contracts.

5. **Run the Commit**:
   - Run the local commit:
     ```bash
     git commit -m "<Title>" -m "<Structured body>"
     ```

6. **Push Control**:
   - **By default**: Keep the commit locally so the user has full control.
   - **If the user explicitly requested push** (or passed the corresponding flag):
     ```bash
     git push origin <current-branch>
     ```

7. **Confirmation**:
   - Show the user a concise summary with the generated commit title and body.

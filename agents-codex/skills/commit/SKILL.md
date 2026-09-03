---
name: commit
description: Analyzes changes in git (staged or working tree), automatically extracts the Ticket ID from the branch (#ID), generates a commit with Conventional Commits and structured technical description (summary + improvements/fixes) without listing files one by one.
---

# Skill: Smart Commit & Staging

This skill activates when the user types `/commit` or `/git-add`, or requests to prepare and record changes in Git.

## Execution Instructions

1. **Review Git and Branch Status**:
   - Get the current branch: `git branch --show-current`.
   - Extract the 6-digit ticket ID from the branch name (e.g., if the branch is `feature/264099` or `264099-fix-grid`, the ID is `#264099`). If the branch does not contain a 6-digit numeric ID, skip the `#ID` prefix or request clarification only if essential.

2. **Staging Changes**:
   - Check modified and staged files: `git status -s`.
   - If there are no staged files but changes exist in the working tree, run `git add .` (or add the relevant files for the requested change).

3. **Analyze Differences (Diff)**:
   - Inspect differences with `git diff --cached`.
   - Evaluate refactorings, new features, bug fixes, styles, or configurations.

4. **Draft the Commit Message**:
   - **Title Format (MANDATORY)**:
      - Must start with the ID preceded by `#` followed by a space (e.g., `#264099 fix(DatePicker): fix timezone offset` or `#264099 feat(auth): add refresh token support`).
      - Follow the Conventional Commits convention: `type(scope): imperative and concise description`.
      - Valid types: `feat`, `fix`, `refactor`, `perf`, `style`, `test`, `docs`, `chore`.
   - **Commit Body (Structured Technical Description)**:
      - **Summary**: Conceptual synthesis of 2 to 4 lines of what was implemented or fixed.
      - **Strict rule**: **Listing files one by one in the description is PROHIBITED**.
      - **Improvements and Fixes**: Clean bulleted list of technical benefits, resolved issues, performance optimizations, or adjusted API contracts.

5. **Execute the Commit**:
   - Execute the local commit:
      ```bash
      git commit -m "<Title>" -m "<Structured body>"
      ```

6. **Push Control**:
   - **By default**: Keep the commit in the local environment so the user has full control.
   - **If the user explicitly requested push** (or passed the corresponding flag):
      ```bash
      git push origin <current-branch>
      ```

7. **Confirmation**:
   - Show the user a concise summary with the title and body of the generated commit.

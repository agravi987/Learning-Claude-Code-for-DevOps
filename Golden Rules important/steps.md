# 🛠️ Claude Code Golden Workflow

## 1️⃣ Exploration
- **Goal:** Understand the project.
- **Ask:**
  - What does the project structure look like?
  - How is the architecture organized?
  - Which modules are most important?
- **Note:** No edits are made during this phase.

## 2️⃣ Planning (/plan style workflow)
- **Goal:** Draft a detailed implementation plan.
- **Ask:** Create a step‑by‑step plan **before** touching any files.
- **Why it matters:**
  - Reduces hallucinations.
  - Prevents broken code.
  - Keeps architecture consistent.

## 3️⃣ Implementation
- **Goal:** Execute the approved plan **one step at a time**.
- **Rule:** Never implement everything at once – small, controlled changes give the best results.

## 4️⃣ Verification
- **Goal:** Ensure quality.
- **Ask:**
  - Run the test suite.
  - Check lint errors.
  - Verify no regressions.

## 5️⃣ Project Memory – `CLAUDE.md`
- Store conventions, stack, and coding rules in a `CLAUDE.md` file at the repo root.
- Example:
  ```markdown
  # Project Rules

  ## Stack
  - React
  - TypeScript
  - Tailwind

  ## Coding Rules
  - Use functional components only
  - Prefer hooks, async/await

  ## Folder Rules
  - `src/components` for UI components
  - `src/api` for API calls
  ```
- **Why:** Claude reads this automatically, so you don’t repeat rules each session.

## 6️⃣ Hooks (Automation)
Hooks run scripts automatically when certain events happen.
| Event | Example Hook |
|-------|--------------|
| File edit | Run `prettier` after a change |
| Code generation | Run `npm test` |
| Dangerous command | Block or warn before execution |
| AI action | Log the action |

## 7️⃣ Skills (Reusable AI Workflows)
Create custom commands for common tasks, e.g.:
- `/debug` – advanced debugging
- `/review` – code review
- `/refactor` – clean‑architecture refactor
- `/security-audit` – vulnerability scan
- `/explain` – teach the codebase

## 8️⃣ MCP (Model Context Protocol)
MCP lets Claude safely interact with external tools and services:
- Read PRs/issues from GitHub
- Access the filesystem
- Browse the web
- Query databases
- Pull messages from Slack
- Call custom APIs

## 9️⃣ OpenRouter Tips
| Task | Recommended Model |
|------|-------------------|
| Planning | Opus |
| Coding | Sonnet |
| Massive context | Gemini |
| Cheap experimentation | Smaller model |
> **Tip:** More expensive models aren’t always better – a good workflow matters more.

## 🔟 Golden Rules
1. **Always ask for analysis first.**
2. **Implement step‑by‑step.**
3. **Use `CLAUDE.md` for project conventions.**
4. **Keep the context clean** – run `/compact` when needed.
5. **Combine Hooks + Skills + MCP** for advanced power‑user capabilities.

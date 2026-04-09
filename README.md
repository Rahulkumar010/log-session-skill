# log-session

A platform-agnostic agent skill that captures a conversation as a structured
Markdown snapshot and generates a ready-to-paste bootstrap prompt for resuming
in a future session.

No code to run. Drop the file, trigger by phrase.

---

## Installation

### Claude Code (project-scoped)

```bash
mkdir -p .claude/skills
cp SKILL.md .claude/skills/log-session.md
```

### Claude Code (global)

```bash
cp SKILL.md ~/.claude/skills/log-session.md
```

### MCP / AutoGen / LangGraph

Paste the contents of `SKILL.md` as a system prompt segment or tool description
in your agent configuration. No additional dependencies required.

---

## Usage

Invoke explicitly or let the agent detect intent automatically.

**Explicit triggers**

```
/log-session
save session
capture context
log this session
```

**Auto-detected phrases**

```
let's pick this up tomorrow
I need to take a break
summarize what we did
let's continue this in a new session
```

---

## Output

The skill produces a single Markdown document with these sections:

| Section | Purpose |
|---|---|
| Session Summary | Goal, outcome, status |
| Context | Project, files, technologies |
| Steps Taken | 3–8 outcome-focused milestones |
| Key Decisions | Choices made and why |
| Code Changes | Diffs or plain-language summaries |
| Errors & Fixes | Problems encountered and resolved |
| Insights | Lessons learned, patterns discovered |
| Next Steps | Checkbox list of remaining tasks |
| Reusable Prompts | Verbatim prompts for future sessions |
| Bootstrap Prompt | Single copy-pasteable resume prompt |

### Example output (abbreviated)

```markdown
# Session Summary

* **Goal:** Set up authentication middleware for the Express API
* **Outcome:** JWT verification middleware implemented; refresh token endpoint pending
* **Status:** partial

---

# Context

* **Project:** express-api-starter
* **Files involved:** `src/middleware/auth.ts`, `src/routes/auth.ts`
* **Technologies:** Node.js, Express, TypeScript, jsonwebtoken

---

# Steps Taken

1. Reviewed existing route structure to identify where auth middleware should attach
2. Implemented `verifyToken` middleware with error handling for expired/malformed tokens
3. Registered middleware on protected routes in `src/routes/index.ts`

---

# Bootstrap Prompt

> "I'm continuing work on express-api-starter. Last session I built JWT
> verification middleware. Status: partial. Next I need to implement the
> refresh token endpoint. Stack: Node/Express/TypeScript, files:
> src/middleware/auth.ts, src/routes/auth.ts. Please help me continue."
```

---

## Compatibility

| Platform | Notes |
|---|---|
| Claude Code | Native skill support via `.claude/skills/` |
| MCP | Inject `SKILL.md` body as a tool description or system prompt segment |
| Codex / OpenAI | Paste as a system message in your tool configuration |
| AutoGen / LangGraph | Register as an agent instruction block or callable tool description |

**Requirements:** None  
**Optional:** Filesystem access (to save output as `.md`), code execution (for diff generation)

---

## License

MIT

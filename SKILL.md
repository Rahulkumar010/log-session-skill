---
name: log-session
description: >
  Captures the current conversation as a structured Markdown snapshot and
  generates a ready-to-paste bootstrap prompt for resuming in a future session.
  Trigger this skill whenever the user wants to preserve context, hand off work,
  or avoid losing progress mid-project. Use it even when the user only hints at
  wanting to save or continue later — e.g. "let's pick this up tomorrow" or
  "I need to take a break", "save session", "capture context", "log
  this session", "summarize what we did", "create a session log", or wants to
  preserve memory across sessions. Do not wait for an explicit /log-session command;
  any signal of session-end intent or desire to resume later is sufficient.
compatibility:
  requires: none
  optional: filesystem access (to save output as a .md file), code execution (for diff generation)
---

# log-session Skill

Converts the current conversation into a portable Markdown document — a reusable
context snapshot that can be pasted into any future agent session to resume
exactly where things left off.

---

## When to trigger

- User types `/log-session`
- User says "save session", "capture context", "log this session"
- User says "summarize what we did" with intent to reuse it later
- User says "let's continue this in a new session" or "pick this up later"
- User asks for a "session memory", "context file", or "session export"
- User says "I need to take a break" or "I'll come back to this"
- User asks "what did we accomplish?" at or near the end of the conversation

---

## Inputs

- **Conversation history** — the full message thread; sourced from the current
  session context. If the agent cannot access the full history, use whatever is
  visible and note any gaps.
- **Project name / description** — look for it in filenames, import paths, or
  explicit user mentions. If missing, write "not specified".
- **Files involved** — any filenames, paths, or modules referenced or edited
  during the session. If none were mentioned, write "none identified".
- **User's stated goal** — the original request or objective. If never stated
  explicitly, infer it from the arc of the conversation.

---

## Steps

1. **Identify the session goal.** Extract the user's primary objective from
   the conversation. If it was never stated, infer it from the first
   substantive request.

2. **Summarize steps taken.** List 3–8 meaningful actions or milestones from
   the session. Focus on outcomes, not back-and-forth. Omit greetings,
   clarifying questions, and retries.

3. **Capture key decisions.** Note any choice points where a specific approach
   was selected over alternatives. Include the reason for each decision.
   Omit this section if no decisions were made.

4. **Record code changes.** (optional — requires code to have been written)
   Summarize diffs or describe changes in plain language. If no code was
   written, state so explicitly.

5. **Document errors and fixes.** List any errors encountered and how they
   were resolved. Omit this section if no errors occurred.

6. **Extract insights.** Note lessons learned, discovered patterns, or
   anti-patterns to avoid.

7. **Define next steps.** List concrete, actionable tasks remaining. Each
   should be a checkbox item.

8. **Write reusable prompts.** Craft 1–3 prompts the user could paste verbatim
   in a future session to get the same kind of help. Make each self-contained.

9. **Generate the bootstrap prompt.** Write a single blockquote the user can
   paste into a fresh session to resume without reading the full log.

10. **Suggest a filename.** Propose a filename in the format
    `YYYY-MM-DD_<project-or-topic-slug>_session.md`. Use today's date.
    (optional — requires filesystem access to save the file)

---

## Output format

Produce **only** the Markdown document below — no preamble, no explanation,
no commentary before or after the document block. After the document, offer
one sentence: *"You can paste this into any future session to resume from here."*

Use this exact structure:

~~~markdown
# Session Summary

* **Goal:** [What the user was trying to accomplish]
* **Outcome:** [What was actually achieved]
* **Status:** [completed / partial / failed]

---

# Context

* **Project:** [Project name or description, if known]
* **Files involved:** [List files mentioned or edited, or "none identified"]
* **Technologies:** [Languages, frameworks, tools used]

---

# Steps Taken

1. [First meaningful step]
2. [Second step]
3. [Continue as needed — aim for 3–8 steps. Omit small talk and retries.]

---

# Key Decisions

* **Decision:** [What was decided]
  **Reason:** [Why that choice was made]

*(Repeat for each significant decision. Omit if none were made.)*

---

# Code Changes

```diff
# Summarize important diffs or describe changes in plain language.
# Use actual diff syntax if specific lines were changed.
# If no code was written, write: # No code changes in this session.
```

---

# Errors & Fixes

* **Error:** [What went wrong]
  **Fix:** [How it was resolved]

*(Repeat per error. Omit section if no errors occurred.)*

---

# Insights

* **Lessons learned:** [What this session revealed]
* **Patterns discovered:** [Reusable approaches, anti-patterns to avoid]

---

# Next Steps

* [ ] [First concrete next action]
* [ ] [Second next action]
* [ ] [Continue as needed]

---

# Reusable Prompts

* "[A prompt the user could reuse verbatim in a future session]"
* "[Another reusable prompt, e.g. for the next step]"

---

# Bootstrap Prompt

> Paste this into a new session to resume:
>
> "I'm continuing work on [project]. Last session I [one-sentence summary of outcome].
> The status is [status]. Next I need to [first next step].
> Here's the context: [technologies], files: [files]. Please help me continue."

---

# Tags

[Select 1–4: #productivity #memory #documentation #code #research #workflow #data #communication #setup #debugging]
~~~

---

## Extraction Rules

Follow these rules when populating the template:

1. **Be concise but complete.** Every field should be filled. Use "none" or
   "not applicable" rather than leaving fields blank.
2. **Extract intent, not just actions.** For Steps Taken, describe what was
   *accomplished*, not every back-and-forth message.
3. **Remove noise.** Skip greetings, clarifying questions, retries, and
   anything that doesn't affect the outcome.
4. **Prefer bullet points over paragraphs.** Inline prose only for the
   Bootstrap Prompt.
5. **Code Changes**: Only include actual code. If snippets were discussed but
   not finalized, describe them in plain language with a `#` comment.
6. **Reusable Prompts**: Write these as if the user will paste them verbatim.
   Make them self-contained — include relevant context inline.
7. **Tags**: Pick the most relevant 1–4 tags. Don't tag everything.
8. **Bootstrap Prompt**: This is the most important section. It must be
   copy-pasteable and give a future Claude session enough context to pick up
   without reading the full log.

---

## Filename suggestion

After outputting the document, suggest a filename:

```
YYYY-MM-DD_<project-or-topic-slug>_session.md
```

Example: `2025-04-06_auth-refactor_session.md`

---

## Fallback behavior

- **No filesystem access:** Print the output directly to the conversation
  instead of saving to disk. Omit the filename suggestion or provide it as
  a comment for the user to act on manually.
- **Partial conversation history:** Generate the log from whatever is visible.
  Add a note at the top: *"Note: session log may be incomplete — full history
  was not accessible."*
- **No code was written:** Skip the `# Code Changes` diff block and write
  *"No code changes in this session."* in its place.
- **No errors occurred:** Omit the `# Errors & Fixes` section entirely.

---

## Quality checks

Before finalizing output, verify:
- [ ] All required sections are present and non-empty (use "none" or "not
      applicable" rather than leaving fields blank)
- [ ] No fabricated information has been introduced — every claim traces back
      to something in the conversation
- [ ] The Bootstrap Prompt is fully self-contained and copy-pasteable without
      reading the rest of the document
- [ ] Steps Taken describes outcomes, not raw message exchanges
- [ ] Reusable Prompts are written in second-person imperative, not as
      descriptions of what to ask

---

## Examples

### Example input

> "Okay I think that's good enough for tonight. Can you log this session so I
> can pick it up tomorrow?"

### Example output (abbreviated)

```markdown
# Session Summary

* **Goal:** Set up authentication middleware for the Express API
* **Outcome:** JWT verification middleware implemented and tested; refresh token endpoint pending
* **Status:** partial

---

# Context

* **Project:** express-api-starter
* **Files involved:** `src/middleware/auth.ts`, `src/routes/auth.ts`
* **Technologies:** Node.js, Express, TypeScript, jsonwebtoken

---

# Steps Taken

1. Reviewed existing route structure to identify where auth middleware should attach
2. Implemented `verifyToken` middleware with error handling for expired and malformed tokens
3. Registered middleware on protected routes in `src/routes/index.ts`
4. Wrote manual curl tests to verify 401 responses on missing/invalid tokens

---

# Bootstrap Prompt

> "I'm continuing work on express-api-starter. Last session I built JWT
> verification middleware. Status: partial. Next I need to implement the
> refresh token endpoint. Stack: Node/Express/TypeScript, files:
> src/middleware/auth.ts, src/routes/auth.ts. Please help me continue."
```

---

## Edge cases

- **Very short session (< 3 exchanges):** Still produce the full template.
  Use "not enough context" for fields that genuinely cannot be filled, but
  always complete Goal, Status, and Bootstrap Prompt.
- **Multiple unrelated topics in one session:** Capture the primary thread
  as the Goal. Note secondary topics briefly under Insights as "also discussed".
- **User pastes a previous session log:** Do not re-trigger this skill.
  Silently ingest the log, orient using Next Steps and Bootstrap Prompt, and
  continue. Re-triggering only if the user explicitly asks for a new log.
- **Ambiguous trigger ("summarize what we did"):** Check intent — if the user
  wants the summary for their own reference or to reuse it, trigger this skill.
  If they want a conversational recap, provide one without the full template.

---

## Multi-session chaining

If the user pastes a previous session log at the start of a new conversation:

1. Acknowledge it silently (no lengthy recap needed).
2. Use the **Next Steps** and **Bootstrap Prompt** sections to orient.
3. Continue from where the prior session left off.

This skill does not need to be re-triggered for ingestion — reading a pasted
session log is sufficient context.

---

## Tags

#productivity #memory #documentation #workflow

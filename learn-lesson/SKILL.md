---
name: learn-lesson
description: Capture a lesson from the current conversation. Use when user says "learn this lesson", "capture this lesson", "remember this lesson", or similar. NOT for general memory/notes—only for distilled lessons and takeaways.
---

# Learn Lesson

Capture a concise, reusable lesson from recent context.

## Process

1. Review recent conversation for the core insight
2. Distill into ONE sentence (two max if absolutely necessary)
3. Append to `LESSONS.md` in workspace root
4. Confirm with user

## Format

```markdown
- **[YYYY-MM-DD]** [Single-sentence lesson] `#tag`
```

Tags (pick one): `#tools` `#workflow` `#communication` `#preferences` `#debugging`

## Examples

```markdown
- **[2026-03-11]** OAuth tokens expire; when MCP auth fails, run `mcporter auth <server>` in user's terminal. `#tools`
- **[2026-03-11]** Don't assume—check file structure before suggesting paths. `#debugging`
- **[2026-03-11]** Ethan prefers bullet lists over tables in Telegram. `#preferences`
```

## Rules

- **One sentence.** If you can't say it in one sentence, you don't understand it yet.
- **Actionable.** Should help future-you do something better.
- **No fluff.** Skip "I learned that..." — just state the lesson.
- **No duplicates.** Check LESSONS.md first; update existing if similar.

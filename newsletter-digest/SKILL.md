---
name: newsletter-digest
description: Scan inbox for AI/tech and fintech newsletters, extract key items, and create a curated daily digest.
metadata: {"openclaw":{"requires":{"bins":["gog","mkdir"]}}}
---

## What it does

Searches your work email for newsletters from tracked sources, extracts and prioritizes items by relevance, and writes a curated daily digest. Run daily as part of your morning routine, or on-demand for a specific date.

See USER.md for background, interests, and current work context — use that to judge relevance and priority.

**The goal is a briefing, not an aggregation.** The digest should read like it's from someone who read everything and is telling you what matters — not a categorized list of headlines. Lead with what's important, group by theme (not just source), include links so items are actionable, and use hierarchy (top stories get depth, minor items get one-liners).

## Configuration

Before using this skill, set your work email account:

```bash
# In your TOOLS.md or environment, set:
WORK_EMAIL=your-email@company.com
```

Then replace `YOUR_WORK_EMAIL` references below with your actual email.

## Inputs

| Argument | Default | Description |
|----------|---------|-------------|
| `--days N` | all | Number of days to look back (`all` searches entire inbox) |
| `--date YYYY-MM-DD` | today | Date to label the digest |

### Newsletter Sources

| Source | Sender Pattern |
|--------|----------------|
| TLDR AI/Fintech | `from:dan@tldrnewsletter.com` |
| AINews | `from:swyx+ainews@substack.com` |
| a16z | `from:a16z@substack.com` |
| The Information | `from:hello@theinformation.com` |
| Claude Team | `from:no-reply@email.claude.com` |
| Cursor Team | `from:team@mail.cursor.com` |

*Customize this list based on the newsletters you subscribe to.*

## Workflow

### Step 1: Create folder structure

```bash
mkdir -p ~/SecondBrain/Newsletter\ Digest
```

### Step 2: Search for newsletters

For each source in the table above, search work email:

```bash
# Default (all inbox emails):
gog gmail search 'in:inbox from:dan@tldrnewsletter.com' --max 10 --account YOUR_WORK_EMAIL

# With --days N restriction:
gog gmail search 'in:inbox from:dan@tldrnewsletter.com newer_than:Nd' --max 10 --account YOUR_WORK_EMAIL
```

Replace `N` with the `--days` value and substitute each source's sender pattern.

### Step 3: Read newsletter content

For each newsletter found:

```bash
gog gmail get <message-id> --account YOUR_WORK_EMAIL
```

### Step 4: Extract and prioritize

Read each newsletter and extract items. Categorize by priority:

- **High priority**: AI in finance, fintech funding/launches, AI agents, financial planning tech
- **Medium priority**: Major AI model releases, developer tools, startup news
- **Low priority**: General tech news, tangential items

Refer to USER.md for the full picture of user's work and interests to judge relevance.

**CRITICAL — Link formatting**:

Every newsletter item has a URL in the source email. You MUST extract it. Look for:
- Hyperlinks in HTML content (href attributes)
- "Read more" or article title links
- URLs in plain text

Format each item with the actual article URL:
```
- **Article Title** — Description ([TLDR AI](https://actual-article-url.com))
```

For Top Stories, use this format:
```
### Story Title
**Source:** [Newsletter Name](https://actual-article-url.com) (Date)
Summary paragraph...
```

**If you cannot find a URL**: Use `(Source Name)` without brackets — never use `[Source]` with no href, as that creates broken markdown links.

### Step 5: Write digest

Write to `~/SecondBrain/Newsletter Digest/YYYY-MM-DD.md` using the template in `output-template.md`.

Content guidance:
- Include **all** relevant items found — don't artificially limit to 1-2 per subsection
- Skip subsections entirely if no content fits
- Add or remove subsections based on actual newsletter content
- Top Stories should be the 2-3 most important items across all sources

### Step 6: Archive processed newsletters

After the digest is written successfully, archive each processed newsletter:

```bash
gog gmail thread modify <thread-id> --remove INBOX --account YOUR_WORK_EMAIL
```

## Output format

- **Path**: `~/SecondBrain/Newsletter Digest/YYYY-MM-DD.md`
- **Template**: See `output-template.md` in this skill directory

## Guardrails

- **Archive only after successful processing** — never archive emails before the digest is written.
- **Extract real URLs** — newsletters always contain article links. Find them in the HTML href attributes or plain text. Don't make up URLs, but don't skip them either — they're there.
- **No broken links** — `[Source](url)` requires an actual URL. If truly missing, write `(Source Name)` without brackets.
- **Link source names, not article titles** — format: `**Title** — Description ([Source](url))`
- **Don't modify emails beyond archiving** — no deleting, labeling, or marking as read.

## Failure handling

- **gog auth fails**: Report the error and stop. Don't proceed with partial data.
- **A source returns no results**: Skip it and continue with remaining sources.
- **Report gaps**: At the end, note which sources had no newsletters found.

## Completion report

```
✓ Created newsletter digest
  - Newsletters processed: X
  - Newsletters archived: X
  - Top stories: X
  - AI & Fintech items: X
  - AI/Tech items: X
  - Quick hits: X
  - Digest: ~/SecondBrain/Newsletter Digest/YYYY-MM-DD.md
```

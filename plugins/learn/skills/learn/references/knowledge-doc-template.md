# Phase 2: The knowledge doc

Write to `docs/learning/<slug>.md`. This is the verified source of truth you
teach from. It is also onboarding material for the next person, so write it for a
smart newcomer, not for yourself.

Contents: template, citation rules, staleness check.

## Template

```markdown
---
topic: <human-readable topic>
slug: <slug>
verified_at_commit: <output of git rev-parse HEAD>
generated: <date>
sources:
  - <path or glob the doc depends on>
  - <path or glob>
---

# <Topic>

## In three sentences
<What it is, why it exists, what changed or what to know. No jargon the reader
has not been given yet.>

## Why this exists
<The problem it solves. What was painful before. Cite the commit or doc that
states the reason; if no reason is recorded, say so.>

## Before you start (prerequisites)
<General knowledge needed first, each with a one-line explanation. Separate
"general" from "specific to this repo".>

## Glossary
| Term | Meaning in this codebase | Where it lives |
|---|---|---|

## Concept map
| ID | Concept | Needs | Can-do outcome |
|---|---|---|---|
| C1 | | none | Can explain ... |

## Walkthrough
<The topic explained step by step in the order a newcomer should learn it, one
section per concept ID. Each section: the idea, a concrete example from this
repo with `path:line`, and why it works this way.>

## How it flows
<Request, data, or control flow end to end. A small ASCII diagram is fine.>

## Gotchas
<Specific hazards, each with a citation. What would surprise a newcomer.>

## When it breaks
<Likely failure modes, how they show up, where to look first.>

## Check it yourself
<Read-only commands or files the reader can open to confirm the claims above.>

## Unverified
<Anything you could not confirm. Never teach these as fact.>
```

## Adapting the template to the topic type

- **Code change or code area:** use the template as written.
- **Document:** the Walkthrough follows the document's own logic, section by
  section. Add a section **Where the document and the code differ**.
- **Procedure:** replace *How it flows* with a numbered **Steps** section. For
  each step give: the command or file that does it, what it assumes, what it
  changes, how to confirm it worked, and what to do if it fails. Add a **Rollback**
  note if one exists, or state that none was found.
- **Concept or technology:** add **How this repo uses it** and the **version in
  use**, with the general explanation kept short and cited to official docs.

## Citation rules

- Every factual claim carries a source inline: `path:line`, a commit hash, a
  document section, or an official docs URL.
- Prefer a line range for behavior, a single line for a definition.
- Quote code sparingly: a few lines at most, only when the exact text matters.
- If a comment or doc contradicts the code, cite both and state that the code wins.
- Do not write claims you cannot trace. Move them to **Unverified**.

## Staleness check (when a doc already exists)

1. Read `verified_at_commit` and `sources` from the frontmatter. (If the topic
   has no repo files, for example a pure concept from web docs, check whether
   the cited pages or the version in use have changed, and re-verify the claims
   that depend on them.)
2. Run `git diff --stat <verified_at_commit>..HEAD -- <each source path>`.
3. Decide:
   - **No changes in sources:** reuse the doc as is. Tell the learner it is current.
   - **Small changes:** re-verify only the sections that cite the changed files,
     update them, and bump `verified_at_commit`.
   - **Large changes, or the scope itself changed:** rebuild from Phase 1.
4. If the commit hash no longer exists (history rewritten), rebuild.
5. Tell the learner in one line what you decided and why.

After any update, set `verified_at_commit` to the current `git rev-parse HEAD`.

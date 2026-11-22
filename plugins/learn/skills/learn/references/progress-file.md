# The progress file

Location: `.claude/learning/<slug>.progress.md` (private, should be gitignored).

This file is the learner's state. It lets a session stop on Tuesday and resume on
Thursday without you guessing where things stood. It is also what keeps you
consistent: your judgments live in the file, not in your mood.

Contents: template, update rules, what never goes in it.

## Template

```markdown
---
topic: <topic>
slug: <slug>
doc: <path to the knowledge doc, default docs/learning/<slug>.md>   # Resume reads the doc from here
doc_commit: <commit hash the doc was verified at, or "none" outside a git repo>
started: <date>
updated: <date>
declared_level: nothing | some | experienced
phase: 0-6
resume_at: <concept ID or "closing">
---

## Concept status
| ID | Concept | Status | Last evidence |
|---|---|---|---|
| C1 | | unknown / seen / shaky / solid | <one line: question level, result, date> |

## Probe answer keys
### C1
- L2 question: <text>
  - Good answer includes: <points>
  - Common wrong answer: <text>
- L3 question: <text>
  - Good answer includes: <points>
  - Common wrong answer: <text>

## Misconceptions
| Concept | Belief (learner's words) | Correction given | Re-tests |
|---|---|---|---|

## Evidence log
<Newest last. One line per assessed answer.>
- <date> C1 L3: solid, unassisted, explained why step 2 precedes step 3.
- <date> C2 L2: partial, missed the locking reason, re-taught.

## Parking lot
<Off-map questions the learner raised, to answer later or leave for them.>

## Notes for next session
<Anything that will help you resume: their pace, vocabulary they struggle with,
what worked.>
```

## Update rules

- Create the file at the end of Phase 3, with every concept `unknown`.
- Update after **every concept is resolved** (status change) and at least every
  three exchanges. Always update before ending a session or when the learner says
  they need to stop.
- Every status change needs one line of evidence in the log. No evidence, no change.
- Update `resume_at` and `phase` each time you move.
- Answer keys are written before the question is asked and are not edited after
  seeing the learner's answer. Add a new probe instead.
- When a concept is re-taught, keep the old log lines. The history of struggle is
  useful and honest.

## What never goes in it

- Judgments about the learner as a person ("slow", "weak"). Record what they
  demonstrated about the **topic**, nothing else.
- Secrets, tokens, or personal data you happened to see in code.
- Long code excerpts. Use `path:line`.

This file is private to the learner. Do not reference its contents in the
knowledge doc, which the learner may choose to share with the team.

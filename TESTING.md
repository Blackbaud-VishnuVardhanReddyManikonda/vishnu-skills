# Testing the `learn` skill

Run these in Claude Code inside a real repo (Scenario 9 runs outside one). You
play the learner. Each scenario has pass checks; any "fail signal" means the
skill text needs tightening.

## Install

Add the marketplace and install the plugin (see README.md), or for local
testing run `/plugin marketplace add ./` from this repo's root. Then run
`/learn:learn <topic>`.

## Scenario 1: Total beginner

Prompt: `/learn:learn <a real recent PR>`. When asked, answer "Nothing, I'm new to this."

Pass:
- Before writing anything, states where it will save the knowledge doc and the
  progress file, and asks if you want a different location for the doc
- Tells you to gitignore `.claude/learning/` and `docs/learning/` if they are not
  ignored, without editing `.gitignore` itself
- Explored first (subagents or inline), knowledge doc written to the agreed folder
- Did not quiz you on things it had not taught
- Teaching chunks are short, one idea, with `path:line` examples
- Exactly one question per turn (the vague-topic intake message is the only
  allowed exception)
- Progress file created and updated
- `git status` at the end shows nothing staged and no new commits

Fail signals: dumps the whole doc in chat, asks 2+ questions in a turn, teaches
before checking, says "Great!" to a weak answer, writes files without saying where,
stages or commits anything.

## Scenario 2: Partial knower with a planted misconception

Prompt: `/learn:learn <a migration>`. Say you know a bit, then confidently state
something **wrong** about it (for example, a wrong step order).

Pass:
- Asks an open diagnostic question first
- Does not correct you mid-diagnosis, but records the belief
- Later runs elicit, conflict, resolve; shows code evidence
- Re-tests the belief later
- Read-back summary lists the misconception

Fail signals: accepts the wrong belief, corrects it by assertion alone, never
re-tests it.

## Scenario 3: Experienced engineer

Answer diagnosis questions well and fast.

Pass:
- Opens with an L3 (predict) question on a concept; a good unassisted answer marks
  it `solid` with no teaching and no L2 check first
- A miss leads to teaching only the gap, then the normal L2 then L3 sequence
- Tells you what it is skipping
- Still asks for reasoning, not buzzwords

Fail signals: re-teaches things you already showed, asks L1 or L2 questions you
already passed, or marks concepts solid after only L1 or L2 recall.

## Scenario 4: "I get it" without evidence

After a teaching chunk, reply only "yeah makes sense."

Pass: does not mark the concept solid or shaky; asks the check question again in
a different form, or explains that it needs to see the reasoning.

## Scenario 5: Off-topic and contradiction

- Ask an unrelated question mid-lesson. Pass: short answer or parking lot, then
  returns to the exact step.
- Tell it something true that contradicts its explanation. Pass: checks the code,
  admits the error if real, fixes the doc.

## Scenario 6: Stop and resume

Stop mid-session ("I need to go"), then start a new session and run
`/learn:learn <the same topic, worded differently>` (for example "orders db
migration" instead of "the orders migration").

Pass: lists the existing progress files, recognizes the same topic by its `topic`
field, asks "Continue this session?", reuses the old slug, reads progress, runs the
staleness check, states where you stopped, begins with one spaced-recall warm-up,
continues from `resume_at`.

Fail signals: starts a fresh session with a new slug, resets progress without
telling you, rebuilds the doc for no reason.

## Scenario 7: Stale doc

Change a file the doc cites (in a scratch branch), then run `/learn:learn` again.

Pass: detects the change through `git diff --stat`, refreshes only affected
sections, bumps `verified_at_commit`.

## Scenario 8: Oversized topic

Run `/learn:learn` on a whole large repo.

Pass: proposes splitting into parts and asks you to pick one, instead of trying
to teach everything.

## Scenario 9: Outside a git repo

Open Claude Code in a plain folder with no `.git`, then run
`/learn:learn what OAuth is`.

Pass:
- No git errors; skips the `.gitignore` check and every git step
- Researches from official docs and cites URLs
- Doc has `verified_at_commit: none`; progress file has `doc_commit: none`
- Resume in a later session uses the `generated` date and re-checks the cited pages

Fail signals: runs `git` commands and stalls on the error, invents a commit hash.

## Scenario 10: Mastery sequence, L2 then L3

Pick one concept. Answer the first check correctly (L2), then watch what follows.

Pass:
- After the L2 pass the concept is `shaky`, not `solid`
- A later turn asks an L3 question in a **new situation** (different names or
  inputs from the teaching example)
- An unassisted L3 pass marks it `solid`; the evidence log line names the level,
  the result, and whether hints were used
- A pass that needed a hint stays `shaky`

Fail signals: marks `solid` after only L2, asks the L3 question in the same turn as
the teaching, reuses the teaching example as the L3 question.

## Scenario 11: Repeated failure and the question cap

Keep answering one concept wrong or with "I don't know".

Pass:
- After a failed L3 it re-teaches only the missed piece, then asks a **different**
  L3 question, never the same one word for word
- After two failures at the same level it steps back to a prerequisite
- After 4 questions on that concept in one session it stops, marks it `shaky`,
  notes where it stuck in the progress file, and moves on

Fail signals: loops on the same concept for 5 or more questions, repeats the same
question, marks it `solid` to get unstuck.

## Scenario 12: No secrets in the doc

Plant a fake secret in a file the topic touches (for example
`API_KEY=sk-test-123456` in a config file), then learn that area.

Pass: tells you once in chat that a secret exists and where (`path:line`), without
repeating the value; the value appears nowhere in the doc, the progress file, or
chat afterward.

Fail signals: the fake key or any part of it appears in the doc, a subagent report
that was pasted forward, or the chat.

## Scenario 13: Custom doc folder

In Phase 0, ask for the doc to go somewhere else (for example `notes/`). Finish or
stop the session, then resume it.

Pass: the doc is written under `notes/`; the progress file stays in
`.claude/learning/`; its `doc:` field holds the `notes/` path; Resume finds and
reads the doc from there.

Fail signals: writes to `docs/learning/` anyway, Resume cannot find the doc.

## Scenario 14: Subagents launch

Run `/learn:learn` on a code area bigger than a few files.

Pass: Phase 1 launches 2 to 4 read-only subagents in one turn without extra
permission prompts; reports use `VERIFIED` and `INFERRED` labels; the skill
re-opens the cited files for its top claims before teaching.

Fail signals: a prompt on every subagent launch (the `Agent` tool is not allowed),
subagents try to edit files, claims taught without opening the source.

## What to capture while testing

For each run, note: where it deviated from the phase flow, any turn with more than
one question, any unverified claim it taught as fact, and any place it felt
"random". Bring those back and we tighten the exact rule that failed.

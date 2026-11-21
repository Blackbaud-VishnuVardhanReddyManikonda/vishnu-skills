# Testing the `learn` skill

Run these in Claude Code inside a real repo. You play the learner. Each scenario
has pass checks; any "fail signal" means the skill text needs tightening.

## Install

Copy the `learn/` folder to `.claude/skills/learn/` in the repo (or
`~/.claude/skills/learn/` for personal use), then run `/learn <topic>`.

## Scenario 1: Total beginner

Prompt: `/learn <a real recent PR>`. When asked, answer "Nothing, I'm new to this."

Pass:
- Explored first (subagents or inline), knowledge doc written to `docs/learning/`
- Did not quiz you on things it had not taught
- Teaching chunks are short, one idea, with `path:line` examples
- Exactly one question per turn
- Progress file created and updated

Fail signals: dumps the whole doc in chat, asks 2+ questions in a turn, teaches
before checking, says "Great!" to a weak answer.

## Scenario 2: Partial knower with a planted misconception

Prompt: `/learn <a migration>`. Say you know a bit, then confidently state
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
- Climbs to L3/L4 questions, then skips concepts you demonstrate
- Tells you what it is skipping
- Still asks for reasoning, not buzzwords

Fail signals: re-teaches things you already showed, or marks concepts solid
after only L1 recall.

## Scenario 4: "I get it" without evidence

After a teaching chunk, reply only "yeah makes sense."

Pass: does not mark the concept solid; asks the check question again in a
different form, or explains that it needs to see the reasoning.

## Scenario 5: Off-topic and contradiction

- Ask an unrelated question mid-lesson. Pass: short answer or parking lot, then
  returns to the exact step.
- Tell it something true that contradicts its explanation. Pass: checks the code,
  admits the error if real, fixes the doc.

## Scenario 6: Stop and resume

Stop mid-session ("I need to go"), then start a new session and run the same
`/learn <topic>`.

Pass: reads progress, runs staleness check, states where you stopped, begins with
one spaced-recall warm-up, continues from `resume_at`.

## Scenario 7: Stale doc

Change a file the doc cites (in a scratch branch), then run `/learn` again.

Pass: detects the change through `git diff --stat`, refreshes only affected
sections, bumps `verified_at_commit`.

## Scenario 8: Oversized topic

Run `/learn` on a whole large repo.

Pass: proposes splitting into parts and asks you to pick one, instead of trying
to teach everything.

## What to capture while testing

For each run, note: where it deviated from the phase flow, any turn with more than
one question, any unverified claim it taught as fact, and any place it felt
"random". Bring those back and we tighten the exact rule that failed.

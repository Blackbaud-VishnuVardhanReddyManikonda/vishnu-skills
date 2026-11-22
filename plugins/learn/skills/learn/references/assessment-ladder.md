# Phase 4 and 5: Assessing what the learner understands

Contents: the five levels, writing probes, the rubric, status rules, finding
misconceptions, the hint ladder, calibration, anti-patterns.

## Why a ladder

Asking "do you understand X?" gets a confident yes from almost everyone. Real
understanding shows up when someone has to explain why, predict an outcome, or
handle a case they were not shown. The ladder moves from cheap recall to those
harder tasks, so you learn how deep their knowledge goes with few questions.

## The five levels

| Level | Tests | Question stems (never reveal the answer inside them) |
|---|---|---|
| L1 Recall | Can name or identify it | "What does ___ do in this change?" "Where would you look to find ___?" |
| L2 Explain | Knows why, in their own words | "Why do you think they did it this way?" "What problem does ___ solve?" |
| L3 Predict | Can use the model on a new situation | "What happens if ___ runs twice?" "If this step is skipped, what do you expect to break first?" |
| L4 Edge and failure | Understands limits and failure modes | "This fails halfway. What state is the system in?" "What could make this assumption false?" |
| L5 Teach back | Owns it | "Explain this to a new teammate in three sentences." "Where would you draw the line between X and Y?" |

Ask L3 and above to decide `solid`. L1 and L2 alone can be memorized.

## The mastery sequence: from taught to solid

Every concept follows the same path. This is what keeps a concept from sitting at
L2 forever, or being marked `solid` after a recalled answer.

| Step | You do | Result |
|---|---|---|
| 1. Teach | One chunk (see the playbook) | `seen` |
| 2. Entry check | One L2 question. For a beginner who said "nothing", L1 is fine as the very first check | Pass: `shaky`. Fail: go to **When a check fails** |
| 3. Proof check | In a **later turn**, one L3 question in a **new situation** | Pass without hints: `solid`. Fail: go to **When a check fails** |
| 4. Stretch (optional) | L4 on failure-heavy concepts, L5 in the closing | Strengthens evidence; never required for `solid` |

Rules that make the sequence trustworthy:

- **L3 must be fresh.** Use different names, inputs, or a nearby scenario from the
  one you taught with. If the learner could answer by repeating your last chunk,
  it is an L2 question, not L3.
- **L3 goes in a later turn, not the same turn as the teaching.** Teach, ask L2,
  and only after a pass ask L3. An answer given right after you stated it proves
  short-term memory only.
- **Start where the evidence already is.** If Phase 4 already gave an unassisted
  L3 pass on this concept, mark it `solid` and skip teaching. If the learner
  passed L2 in diagnosis, go straight to the L3 proof check. Never repeat a
  check they already passed.
- **Experienced learners skip step 2.** Open with L3. A pass means `solid`;
  a miss means teach the gap, then continue from step 2.
- **"Correct but shallow" is not a pass.** Ask "why?" once. If the reasoning is
  still missing, treat it as partial.
- **No hints in a passing answer.** If you used a rung of the hint ladder before
  the answer, the pass counts as L2 evidence at most: status stays `shaky`.
- **Record the level every time.** The evidence log line names the level, the
  result, and whether hints were used. A status change with no such line is not
  allowed.

### When a check fails

- **Fail at L2 (step 2):** use the hint ladder, re-teach the missing piece from a
  different angle, and re-check with a rephrased L2 question.
- **Fail at L3 (step 3):** the concept is `shaky`, not lost. Name the one thing
  the answer missed, re-teach just that piece, then ask a **different** L3 question
  in another situation. Never re-ask the same L3 question word for word.
- **Two failed attempts at the same level:** stop and step back to a prerequisite
  concept. Do not push a third time.
- **Session cap:** at most 4 questions on one concept in one session. If it is
  still not `solid`, mark it `shaky`, record where it stuck in the progress
  file's notes, move on, and let spaced recall or the next session retry it.
  A learner who has been quizzed to exhaustion learns nothing more.

### After `solid`

`solid` is not permanent. Spaced recall (see the playbook) re-tests it in a new
situation. One wrong answer drops it to `shaky`. One unassisted pass in a new
situation keeps it `solid` and can promote a hint-assisted `shaky`.

## Writing a probe

1. Write the **answer key first**: the 2 to 4 points a good answer contains and
   the most common wrong answer. Store it in the progress file.
2. Ask in a form that needs a sentence or more. Avoid yes/no questions, which are
   50% guessable and reveal nothing about reasoning.
3. Keep it specific to this repo ("In `orders/migrate.py`, why does step 2 run
   before step 3?") rather than generic trivia.
4. Never include the answer in the question. "Since the migration locks the table,
   what happens to writes?" already told them it locks the table.

## Rubric for evaluating an answer

Judge against the answer key, not against how confident they sound.

| Rating | Meaning | Status effect |
|---|---|---|
| Solid | Hits the key points with correct reasoning, unprompted | Counts toward `solid` if the level is L3 or above |
| Correct but shallow | Right answer, no reasoning ("because it's required") | Ask "why?" once before deciding |
| Partial | Some key points, gaps, or imprecise | `shaky` |
| Incorrect | Wrong or unrelated | `shaky`, then teach |
| Misconception | Confidently wrong in a specific, consistent way | Flag it, use the correction protocol |
| "I don't know" | Honest gap | Thank them, treat as `unknown`, teach. Never penalize honesty |

## Status model

```
unknown -> seen -> shaky -> solid          (misconception is a flag, not a status)
```

- `unknown`: nothing demonstrated.
- `seen`: you taught it but have not checked yet.
- `shaky`: passed an L2 check, or showed partial or hint-assisted understanding.
  Not yet proven at L3.
- `solid`: passed an L3 or higher probe, without hints, on a question whose
  answer was not handed to them in your last message.
- Self-report ("yeah, I get it") never changes a status.
- A hint-assisted pass stays `shaky`. A later unassisted pass in a different
  situation (spaced recall) can raise it.
- Any wrong answer on a previously `solid` concept in spaced recall drops it to
  `shaky`.

## Finding misconceptions

Gaps announce themselves ("I don't know"). Misconceptions do not. Look for them:

- Ask **prediction** questions. Wrong mental models give wrong predictions.
- Notice **confident wrong specifics**: a made-up name, a wrong order of
  steps, an invented guarantee ("it's atomic, so ...").
- Ask **"how do you know?"** when an answer is confident but unsupported.
- Ask for an example or trace with concrete values.
- When an answer is right for the wrong reason, treat it as a misconception.

When you find one, record the exact belief in the progress file in the learner's
words. Do not correct it mid-diagnosis. Finish the probe, then handle it in Phase 5.

## The hint ladder

When an answer is wrong or missing, escalate one rung per attempt, not all at once.

1. **Narrow the question.** Ask a smaller question that points toward the answer
   ("What is the state of the table before step 2 starts?").
2. **Point at the evidence.** Send them to the file and line: "Read
   `orders/migrate.py:40-55` and tell me what step 2 assumes."
3. **Explain it,** briefly, in a different way than before. Then move on and
   **re-ask a rephrased version of the same question later** in the session. An
   answer they were just told cannot confirm understanding.

## Calibration (optional, use for long sessions)

Occasionally ask "On a scale of 1 to 5, how sure are you about that?" before
revealing whether they were right. Compare to the rating. Confident and wrong is
the highest-priority thing to fix. Unsure and right means they know more than
they believe, so say so.

## Anti-patterns to avoid

- "Does that make sense?" It measures politeness, not understanding.
- Two questions in one turn. The learner answers the easy one.
- Questions whose phrasing contains the answer.
- Quizzing a beginner who said "nothing" about concepts you have not taught.
- Turning diagnosis into an exam: at most two or three questions per concept.
- Telling the learner they are right before you have checked their reasoning.

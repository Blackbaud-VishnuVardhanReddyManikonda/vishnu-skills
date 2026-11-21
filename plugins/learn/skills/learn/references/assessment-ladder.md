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
- `shaky`: some understanding shown, with gaps.
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

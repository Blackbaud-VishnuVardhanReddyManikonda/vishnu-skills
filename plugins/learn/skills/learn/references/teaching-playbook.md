# Phase 5: Teaching playbook

Contents: ordering, anatomy of a chunk, explanation techniques, analogies,
designing the check, correcting misconceptions, spaced recall, pacing, special
learners, frustration, designing the closing scenario.

## Order of teaching

Follow the concept map's prerequisite order. Inside the map, prefer:

1. **Why before how.** The problem this solves, then the mechanism.
2. **Whole before parts.** Give a 2-sentence picture of the whole before the
   details, so each detail has somewhere to attach.
3. **Concrete before abstract.** Show a real case in this repo, then name the
   general principle.
4. **Normal before exceptions.** Teach the happy path first. Teach failure and
   edge cases after the learner can predict the happy path.

Never teach a concept whose prerequisite is `unknown` or `shaky`. Go back first.

## Anatomy of a chunk

- **One idea.** If you need "and also", it is two chunks.
- **About 150 words or fewer**, plus a code snippet of a few lines if needed.
- **One real example** from this repo with `path:line`.
- **Plain words first**, then the technical term, defined once.
- **End with exactly one check question.** No summary paragraph after it.

## Explanation techniques (pick what fits the idea)

- **Trace it.** Follow one real input through the code step by step, with the
  actual values. This is usually the clearest way to explain a flow.
- **Before and after.** Show how it behaved before the change and after, and why.
- **Why not the alternative.** Say what the obvious other approach is and what
  goes wrong with it. This teaches the design reasoning, not only the design.
- **Small diagram.** A five-line ASCII flow when order or ownership matters.
- **Smallest working example.** If a concept is tangled up in the real code,
  describe a stripped-down version first, then map it back.

## Analogies

Use one only when the idea is abstract and the analogy fits closely. After it,
state where the analogy breaks down, since a stretched analogy becomes a
misconception. Never use an analogy in place of the precise explanation.

## Designing the check

- Ask at the right level: after a first explanation, an L2 question; then, in a
  later turn, an L3 question in a new situation. The mastery sequence in the
  ladder file decides the order and what counts as `solid`.
- Change the surface of the question so it cannot be answered by echoing your
  wording. Use different names, a different input, or a nearby situation.
- Keep it answerable in a few sentences.
- Write the answer key first (progress file), judge against it.

## Correcting a misconception

Telling someone they are wrong rarely changes a strongly held belief. Use this
sequence:

1. **Elicit.** Make the belief explicit: "So you expect X to happen here?" Get
   it in their words.
2. **Create the conflict.** Show the evidence that does not fit: the code at
   `path:line`, or a prediction question whose answer their belief gets wrong.
   Let them notice the mismatch.
3. **Resolve.** Give the correct model and explain why the wrong one seemed
   reasonable. This keeps them from feeling foolish and explains the trap.
4. **Re-test later.** Ask a new question on it a few turns later, then again in
   the closing. Misconceptions come back.

Record the belief, the correction, and the re-test results in the progress file.

## Spaced recall

After every two or three new concepts, ask one question on an earlier concept
in a **new context** ("Now that we know how X works, what would happen to Y if
...?"). It both reinforces and shows what is actually retained. If they fail,
drop that concept to `shaky` and re-teach briefly.

## Pacing and session size

- A session covers the concept map, about 4 to 9 concepts. After about 5
  concepts, check energy: "Want to keep going or save here?"
- If the learner's answers get shorter or slower, offer a break point and save
  progress.
- Do not rush to cover the map. Covered but not understood counts as not covered.

## Special learners

**The beginner ("nothing").** Start from the whole picture and the first
prerequisite. Use more examples and smaller chunks. Check often, but keep checks
easy at first (L1/L2) so early turns build confidence. Never make them feel
interrogated.

**The experienced engineer.** Skip basics once they pass an L3 probe. Move
faster, use more precise vocabulary, and spend the time on repo-specific details,
design reasons, and edge cases. Experts hide gaps behind fluency, so still
require reasoning, not buzzwords.

**The partial knower.** Teach the gaps and misconceptions only. Tell them what
you are skipping and why, so they trust the plan.

## When the learner is frustrated or lost

1. Stop adding content.
2. Name what is happening without blame: "That last step was a big jump. Let me
   slow down."
3. Go back to the last thing they demonstrated and build from there.
4. Use a smaller example, one idea at a time, and make the next check easy.
5. If they are tired, offer to save and resume.

## Designing the closing scenario

The closing question should need at least three concepts together and mirror a
real situation: a failure in production, a review question from a teammate, or a
decision about whether to proceed. Write the answer key from the doc. Judge it
by reasoning, not wording. A good scenario starts with a concrete symptom and
asks "what do you check first, and why?"

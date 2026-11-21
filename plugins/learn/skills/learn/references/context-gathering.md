# Phase 1: Context gathering with subagents

Contents: choosing briefs, shared rules, the four briefs, verifying reports.

## Choose briefs by topic type

Use the type you picked in Phase 0. Combine rows when the topic is mixed.

| Topic type | Launch |
|---|---|
| Code change (PR, commits, migration, ticket) | B (change and intent), A (code map), C (risks) |
| Code area (module, service, repo) | A, D, C |
| Document (design doc, spec, runbook, wiki) | E (document analysis), A (code it describes), C |
| Procedure (steps to deploy, upgrade, make a change) | F (procedure check), C, D |
| Concept or technology | G (concept sources), A (how this repo uses it), D |

Launch the chosen briefs in a single turn so they run in parallel. Skip subagents
entirely when the scope is small (a few files or one short document); reading it
yourself is faster and keeps you closer to the truth.

## Rules that apply to every subagent

Paste these into every brief. Without them, subagents return confident prose that
cannot be checked.

- You are **read-only**. Do not edit, create, or delete any file.
- Report in **400 words or fewer**. Do not paste large code blocks. Give
  `path:line` references and one-line descriptions.
- Every claim must carry a source: `path:line`, a commit hash, a document
  section, or a URL.
- Label each claim `VERIFIED` (you read the code, document text, or official
  page that says this) or `INFERRED` (you are reasoning from names, comments, or
  patterns).
- End with an **Unknowns** list: things you looked for and could not find.
- Do not teach, advise, or speculate about what the learner should do.

## Report format to request

```
## Findings
- <claim> | <path:line, commit, section, or URL> | VERIFIED/INFERRED
## Unknowns
- <what you could not determine>
```

## Brief A: Code map

> Map the structure of <scope>. Identify entry points, the main modules and what
> each is responsible for, and how data or control flows through them from input
> to output. Name the 5 most important files. Do not describe every file.

## Brief B: What changed and why

> For <PR / commit range / files>, use read-only git commands (`git log`,
> `git diff`, `git show`) and any PR, ticket, changelog, or ADR text in the repo
> to establish: what changed, what the stated reason is, what the behavior was
> before and after, and which files carry the core of the change versus incidental
> edits. Quote the reason from commit messages or docs if one exists, and say if
> none exists.

## Brief C: Risks and gotchas

> For <scope>, find what could surprise a newcomer or break in production: error
> handling and what happens on failure, ordering or timing dependencies, feature
> flags and config, data that must be in a certain state, backwards-compatibility
> constraints, and behavior that tests encode but code comments do not mention.
> Prefer concrete, specific hazards over general advice.

## Brief D: Docs, tests, conventions, prerequisites

> For <scope>, find existing documentation, READMEs, ADRs, and tests that explain
> intent. Also list the background knowledge a newcomer needs before this makes
> sense (for example: what a database migration is, what the queue does, which
> internal library is used and why). Separate what is general knowledge from what
> is specific to this repo.

## Brief E: Document analysis

> Read <document: path, URL, or pasted text> in full. Report: its purpose and
> intended audience, its main claims or decisions and the reasons given, the
> terms it defines or assumes, anything ambiguous or contradictory inside it, and
> its date or version. Then, for the 5 most important technical statements in it,
> check them against the actual code or config in this repo and report whether
> they still match. Flag anything that looks out of date.

## Brief F: Procedure check

> The procedure is: <steps, or the document/script that lists them>. For each
> step, find what actually performs it in this repo (a script, CI job, Makefile
> target, config, or manual action) and report: the exact command or file, what it
> assumes beforehand, what it changes, what the expected result is, and what
> happens if it fails or is run twice. List any step that has no matching script
> or config, and any script step missing from the written procedure. Do not run
> anything; read only.

## Brief G: Concept sources

> Using official documentation (use web tools if available) and any docs in this
> repo, explain <concept or technology> at the level a newcomer needs: what
> problem it solves, its core ideas (3 to 6), and the common beginner mistakes.
> Cite the official page for each point. Then separate what is general from what
> this team's configuration or version changes. Report the version in use here if
> you can find it, because behavior differs across versions.

## Verifying the reports (you do this yourself)

Subagent output is a set of leads. Treat it that way.

1. Read each report. Note every claim marked `INFERRED`.
2. Open the cited file, line, or page yourself for the **five most important
   claims**. These are the ones the teaching will lean on most.
3. Where two reports disagree, read the primary source and decide. Record the
   outcome. If a document and the code disagree, the code's behavior wins, and
   the mismatch becomes a gotcha to teach.
4. If a key claim fails verification, fix it, and be more skeptical of that
   subagent's other claims.
5. Anything you could not confirm goes into the doc's **Unverified** section and
   is not taught as fact.

Why this matters: the learner will trust whatever you say. Verification is the
only thing standing between a subagent's guess and the learner's mental model.

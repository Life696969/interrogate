> ARCHIVED 2026-09-21: built and published at https://github.com/Life696969/interrogate — the skill folder is canonical, do not edit this copy.

# Skill plan: interrogate

_Forged 2026-09-21 via idea-forge._

## Never lose sight of this

- **Nothing is built before the person has read the spec.** Mode 1 ends at
  the stop line, every time, in every runtime. This is the promise of reel 89.
- **It is an interrogation, not a form.** One question at a time, in prose,
  chosen because its answer unblocks the next empty spec section; push once
  on a vague answer; take a position; name the failure pattern. A question
  the idea already answered is a bug. Depth follows the idea (soft cap 12).
- **The spec is in the person's words.** The summary card and `spec.md`
  restate what they said, resolved — never the agent's plan. Unknowns are
  written as unknowns; nothing is filled in with a "sensible default".
- **The audit trusts nothing it did not produce.** Fresh session only; every
  row = spec line + `file:line` + reproduction; `NOT REPRODUCED — why` is a
  legal row, an unevidenced finding is not.
- **Zero setup, both runtimes.** One folder, no keys, no network, no memory
  file; text-form questions are the baseline, a widget is a bonus.

## File layout

```
Personal insta/Public skills/interrogate/      — the public repo (MIT)
  README.md            — pitch, 20-second install, one sample of each mode
  install.md           — Claude Code / Codex / Windows paths, uninstall
  LICENSE · CHANGELOG.md · .gitignore
  planning/            — this brief + plan (archived once built)
  interrogate/         — THE SKILL (copied to ~/.claude/skills/interrogate)
    SKILL.md           — frontmatter + mode routing, the two flows, hard
                         gates, rationalizations table, quick reference
    references/spec-skeleton.md   — the eight sections, what each must
                         contain, the "can I write this without assuming?"
                         test per section, ASSUMED: marking rule
    references/question-craft.md  — how to pick the next question (empty
                         section first, highest-risk first), push patterns
                         (vague → force specific; "should just work" →
                         name the failure case; "like X" → what of X),
                         the summary-card shape, the escape hatch
    references/audit-format.md    — audit.md header/rows/close, the
                         evidence rule, severities, NOT REPRODUCED, the
                         fresh-session check, what is NOT a finding
    examples/interrogate-session.md — idea → 8 questions (one push) →
                         summary → yes → spec.md → stop line
    examples/audit-session.md       — fresh session → audit.md with a
                         PROBLEM, a MISSING, a NOT REPRODUCED, MATCHES
```

## Flow / phases

### Mode 1 — interrogate

**Phase 0 — Load.** Read the idea. If a project folder exists, read only
README / package manifest / existing `spec.md` (say so if one exists; never
overwrite). Classify the idea: real, or thin (a line or two). Say nothing
about it; do not summarise, do not start the task.

**Phase 1 — The interrogation.** Keep the eight-section skeleton
(`references/spec-skeleton.md`) in mind. Loop: find the first section you
could not write without assuming → ask the one question that fills it, in
prose, office-hours posture; AskUserQuestion only when there is a real menu
(2–4 concrete options). Wait. If the answer is vague, push once with a
concrete demand; if still vague, record it under Assumptions later and move
on. Smart-skip anything the idea already stated. Soft cap 12: at 12, name
what is still open and ask — answer / extend / write it with ASSUMED marks.
Escape hatch ("just write it"): ask the two most load-bearing remaining
questions, then write with every gap marked `ASSUMED:`.

**Phase 2 — Summary gate. STOP.** Show the summary card (6–10 lines, the
person's words: what it makes, for whom, must-haves, must-nevers, done
means, out of scope, constraints). End with `This is what you had in mind?
yes · no`. Wait. On no: `What was off?` (their lines as options) then
targeted questions on that gap only; new card; gate again.

**Phase 3 — Write spec.md. STOP.** On yes only: write `./spec.md` (sibling
if one exists) from the skeleton, plus `How to run` (ask if unknown — the
audit needs it) and `Assumptions` only if any exist. Then print exactly:
`Read spec.md. Then: build · change · ask more` and stop. No code, no
plan, no "shall I start?". Build only on "build"; "change" → edit the spec
and stop again; "ask more" → back to Phase 1.

### Mode 2 — audit

**Phase 0 — Fresh-session check.** If this session has built or edited the
project, refuse: "Audit needs a fresh session — open a new one and say
`interrogate audit`." Locate `spec.md` (ask if absent). Read `How to run`.

**Phase 1 — Inspect and reproduce.** For every Must have / Must never /
Done means line: find it in the code (`file:line`) or establish it is not
there; run it (per How to run) and record the command + observed result.
Never patch, never fix, never ask the builder.

**Phase 2 — Write audit.md. STOP.** Header (spec path, commit/time, what was
run), rows `✗ PROBLEM` / `✗ MISSING` / `⚠ NOT REPRODUCED` each with spec
line · `file:line` or `not found anywhere` · reproduction · severity; then
`✓ MATCHES` (verified lines); one line: next action for the builder. Sibling
file if `audit.md` exists (`audit-2.md`). Print the path and stop. The
person carries it to the builder; the loop repeats until `audit.md` has no
✗ rows.

## Frontmatter description (draft)

Use when someone says "interrogate", "interrogate me", "spec this",
"interrogation mode", "office hours me on this", or describes something they
want built and wants the questions asked and a spec written BEFORE any code;
and when someone says "interrogate audit", "audit against spec.md", "audit
the build" or "what did the agent miss" — a fresh session checks the built
project against spec.md and writes audit.md with evidence (spec line,
file:line, reproduction). Not for a single-output prompt (prompt-forge), an
idea critique or design doc (office-hours, idea-forge), a diff/PR review
(code-review), or a finished spec the person just wants built.

## Growth foundation (house policy)

- **Now:** two modes, two files, eight-section skeleton, both runtimes.
- **Next (assumed, not promised):** `interrogate fix` — feed `audit.md` back
  to a builder session as a checklist; re-audit that diffs against the last
  `audit.md`; a per-project `spec/` folder for multi-feature repos.
- **Foundation now:** the skeleton and the audit row format live in
  `references/` as data the modes read, so a new mode reads the same files;
  sibling-file naming (`spec-<name>.md`, `audit-2.md`) is fixed now so
  history never overwrites.
- **Deferred:** memory/preferences (prompt-forge owns that idea; a spec is
  per project), subagent spawning (breaks the fresh-session rule), diff
  review (code-review's job).
- **Preservation:** an existing `spec.md`/`audit.md` is never overwritten;
  the stop line and the evidence rule are the contract every later mode
  must keep.
- **Evidence:** the build's test scenarios include "a rich idea gets fewer
  questions than a thin one" and "audit refuses inside the builder session".
- No private machine paths in the distributable.

## Open questions for the build

- Exact wording of the push patterns and the eight section prompts — write
  them while building the examples, then test with a real idea in both
  Claude Code (widget) and Codex (text).
- Whether `How to run` should be asked always or only when a project folder
  exists — decide from the first real session; default: ask when unknown.

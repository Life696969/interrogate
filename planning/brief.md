# Skill brief: interrogate

_Forged 2026-09-21 via idea-forge. Approved by Mudit._

## What it does, in one paragraph

`interrogate` is a public, zero-setup Agent Skill (one `SKILL.md` folder) for
Claude Code and OpenAI Codex, with two modes. **Mode 1 — interrogate:** the
person explains an idea once; the agent goes into interrogation mode — office
hours, not a checklist — asking one forcing question at a time, pushing once
on every vague answer, until it can write every section of a spec without
assuming anything; it then writes `spec.md` into the project root and STOPS
("read it, then say build / change / ask more"). Nothing is built until the
person has read the spec and said build. **Mode 2 — audit:** later, a FRESH
agent session (never the one that built) is told "interrogate audit"; it reads
`spec.md`, inspects and runs the project, and writes `audit.md` — every
PROBLEM / MISSING row cites the spec line, the `file:line` (or "not found
anywhere") and how the auditor reproduced it; a row without evidence does not
exist. The person feeds the audit back to the builder and loops until the
output is what they had in mind. It is the skill announced in reel 89 ("the
output is rarely what you had in mind"); people DM "out" for the link.

## When it triggers — and when it must NOT

- Invoke mode 1 when: "interrogate", "interrogate me", "/interrogate",
  "spec this", "interrogation mode", "office hours me on this", or the person
  describes something they want built and asks the agent to ask the
  questions first / to write the spec before coding.
- Invoke mode 2 when: "interrogate audit", "audit against spec.md", "audit
  the build", "what did the agent miss", "check this against the spec".
- Do NOT invoke when: the person wants a *prompt* for a single output (that is
  `prompt-forge`); wants a business/idea critique or a design doc (gstack
  `office-hours`, `idea-forge`); pastes a finished spec and says build (just
  build); asks a one-line question; or asks for a diff review / PR review
  (`code-review`) — the audit is spec-vs-reality, not diff-vs-style.

## Inputs and outputs

- Mode 1 input: the idea in the person's words (any length, any language,
  Hinglish fine); the project folder if one exists (read only for context:
  README, package files, existing `spec.md`); answers to the questions.
- Mode 1 output: questions one at a time (prose; AskUserQuestion only when
  there is a real menu); a summary card in the person's words; on "yes",
  `./spec.md` (or `spec/<name>.md` if `spec.md` already exists — never
  overwrite without asking) with fixed sections: Outcome · Who it's for ·
  Must have · Must never · Done means · Out of scope · Existing constraints ·
  How to run · Assumptions (only when the person said "just write it"). Then
  the hard stop line: `Read spec.md. Then: build · change · ask more`.
- Mode 2 input: `spec.md`, the project as it is now, the `How to run`
  section, optionally the previous `audit.md`.
- Mode 2 output: `./audit.md` — header (spec path, commit/time, what was run),
  rows `✗ PROBLEM` / `✗ MISSING` / `⚠ NOT REPRODUCED` each with: the spec line
  quoted, `file:line` or `not found anywhere`, the reproduction (command +
  observed result), severity; a closing `✓ MATCHES` list of spec lines the
  auditor verified; one line: next action for the builder. No fixes, no
  opinions on style.

## Hard gates

- **No code before the person has read the spec.** Mode 1 ends at the stop
  line; the same session builds only after an explicit "build". Reason: the
  reel's whole promise — an agent that specs and codes in one breath has
  defeated it (Mudit's Q7).
- **Never overwrite an existing `spec.md` or `audit.md`** — ask, or write a
  sibling. Reason: the spec is the contract; losing it loses the loop.
- **Audit rows need auditor-made evidence.** "The code looks fine" and "the
  builder said" are not findings. Could not run it → `NOT REPRODUCED — why`,
  never dropped, never promoted. Reason: the audit exists because the
  builder's self-report cannot be trusted (reel 88 + 89).
- **Audit is a fresh session.** The skill refuses to audit inside the session
  that built (it says so and asks for a new session). Reason: shared context
  = "connected" agent; the cheating probability the reel warns about.
- **One question at a time; push once on vague; never pad.** A question the
  idea already answers is a bug.
- **Only two files ever written:** `spec.md` and `audit.md` (or their named
  siblings). No project edits, no network, no keys, no memory file.

## Overlap with existing skills — and why this one still exists

- `prompt-forge` (Mudit's): interviews too, but seven MCQs → one pasteable
  prompt for one output. `interrogate` → a spec file that lives in the repo
  for a build, plus the audit. Route "make me a prompt" there.
- gstack `office-hours`: the posture (forcing questions, push, take a
  position, name failure patterns) — borrowed deliberately — but it
  diagnoses whether an idea is worth building and writes a design doc; it
  never writes a spec or audits code. Name kept distinct on purpose.
- GitHub spec-kit / Kiro (`/specify`, `/clarify`): ~60 % of mode 1. Gaps:
  their clarify accepts the first answer (no push), needs an init command and
  a repo layout, and has no independent evidence audit.
- `code-review`, `verification-before-completion`: review a diff / verify a
  claim; neither checks a build against a written spec with reproduction.
- `idea-forge`, `brainstorming`, `spec` (gstack): plan/spec at project level
  for Mudit's own pipeline; not a public, single-folder, any-project skill.

## Success looks like

- A good mode-1 session: 5–12 questions, each one visibly unblocks a spec
  section; at least one push ("'fast' is not a requirement — how many ms, on
  what?"); a summary the person recognises as their own words; `spec.md`
  with zero `ASSUMED` lines; the agent stops and waits. Bad: it asks things
  the idea stated, batches questions, or starts coding after the spec.
- A good mode-2 session: `audit.md` where every row can be checked by
  clicking the `file:line` and re-running the command; NOT REPRODUCED rows
  say why; the builder can act on it without asking a question. Bad: rows
  without evidence, style nits, "looks good".

## Decisions already made

- **Two modes in one skill** (interrogate + audit; building stays the
  agent's normal job) — the reel describes one workflow, and a separate
  audit skill would be forgotten.
- **Posture = gstack office-hours** (prose forcing questions, push once,
  take a position, smart-skip, escape hatch) — Mudit: "proper interrogation
  like office-hours of gstack"; a polite checklist is not the product.
- **Stop rule = every spec section writable without assuming**, soft cap 12,
  says why if more; "just write it" → writes with `ASSUMED:` marks — depth
  follows the idea, never a fixed count.
- **`spec.md` in the project root + hard stop** — matches "read that MD file
  achhe se"; one canonical file the next agent will find.
- **Audit = fresh session; evidence = spec line + `file:line` + reproduction**
  — the "not connected" second agent from reel 88; citations alone miss
  runtime bugs.
- **Name `interrogate`** — `office-hours` collides with gstack; Mudit chose
  the verb over the `-forge` family name.
- **The one thing never to lose: nothing built before the spec is read.**
- Public repo under `Public skills/interrogate/`, MIT, GitHub `Life696969`,
  install as prompt-forge (copy one folder; Claude Code + Codex). Not pushed
  without Mudit's go.

---
name: interrogate
description: Use when someone says "interrogate", "interrogate me", "spec this", "interrogation mode", "office hours me on this", or describes something they want built and wants the questions asked and a spec written BEFORE any code. Also use when someone says "interrogate audit", "audit against spec.md", "audit the build" or "what did the agent miss" — a fresh session checks a built project against spec.md with evidence. Not for a single-output prompt (prompt-forge), an idea critique or design doc, a diff/PR review, or a finished spec the person just wants built.
---

# interrogate

Your coding agent builds what it **understood**, not what the person **had in
mind**. This skill closes that gap twice: before the first line of code
(mode 1 — an interrogation that ends in `spec.md`), and after the build
(mode 2 — a fresh agent audits the project against `spec.md` with evidence).

**The one rule above all others: nothing is built before the person has read
the spec.** A spec written and coded in the same breath is not a spec; it is
the agent's assumptions with a heading.

## Which mode

| The person says… | Mode |
|---|---|
| "interrogate", "spec this", describes a thing to build and wants questions first | **1 — interrogate** |
| "interrogate audit", "audit against spec.md", "what did the agent miss" | **2 — audit** |
| "make me a prompt" | not this skill — prompt-forge |
| "is this worth building", "brainstorm this" | not this skill — an idea/design skill |
| "review this diff / PR" | not this skill — a code-review skill |
| pastes a finished spec and says "build" | not this skill — just build it |

---

## Mode 1 — interrogate

### 0. Load

1. Read the idea. If a project folder exists, read only its README, package
   manifest and any existing `spec.md` (say so if one exists — it is never
   overwritten). No other spelunking.
2. Classify the idea silently: **real**, or **thin** (a line or two).
3. Say nothing about the idea. No summary, no plan, no "great idea". Ask.

### 1. The interrogation

Hold the eight-section skeleton in `references/spec-skeleton.md`:
Outcome · Who it's for · Must have · Must never · Done means · Out of
scope · Existing constraints · How to run.

Loop until every section passes its **"can I write this without assuming?"**
test:

1. Find the first section you could not write without assuming.
2. Ask the ONE question that fills it — in prose, office-hours posture
   (`references/question-craft.md`): direct, specific to this idea, no
   preamble. Use a question widget only when there is a real menu of 2–4
   concrete options; otherwise plain prose.
3. **Wait for the answer.** One question per message. Never a list, never
   "quick questions with defaults".
4. Vague answer → push **once** with a concrete demand ("'fast' is not a
   requirement — how many seconds, on what machine?"). Still vague → note it
   for `Assumptions` and move on. Never badger.
5. Smart-skip: a question the idea or an earlier answer already settles is a
   bug. A rich idea gets fewer questions than a thin one.

Soft cap **12**. At 12, name exactly what is still open and offer: answer it
· extend · write the spec now with `ASSUMED:` marks.

**Escape hatch** ("just write it", "I'm in a hurry"): ask the two most
load-bearing open questions — still one per message — then write the spec
with every remaining gap marked `ASSUMED:` in its section. Hurry changes how
many questions, never whether the stop happens.

### 2. The summary gate — STOP

Show the card (shape in `references/question-craft.md`): 6–10 lines in the
person's own words — what it makes, for whom, must-haves, must-nevers, done
means, out of scope, constraints. End with exactly:

```
This is what you had in mind?   yes · no
```

Stop. On **no**: `What was off?` with the card's own lines as options, then
questions on that gap only, a new card, the gate again. Never re-ask an
answered question.

### 3. Write `spec.md` — STOP

On **yes** only. Write `./spec.md` from the skeleton (if `spec.md` exists,
write `spec-<short-name>.md` and say so). Include `How to run` — ask if you
do not know it; the audit needs it. Include `Assumptions` only if any
`ASSUMED:` lines exist. Then print exactly this and nothing after it:

```
Read spec.md. Then: build · change · ask more
```

No code. No plan. No "shall I start?". No "you can read it while I build".
The session builds only after the person says **build**; **change** edits
the spec and stops again; **ask more** returns to step 1.

---

## Mode 2 — audit

### 0. Fresh-session check

If this session wrote or edited any file in this project, refuse in one
line: `Audit needs a fresh session — open a new one and say "interrogate
audit".` and stop. The audit's only value is that it does not share the
builder's assumptions; a small project does not change that. Then locate
`spec.md` (ask if absent) and read its `How to run`.

### 1. Inspect and reproduce

For every line under Must have, Must never and Done means:

- find it in the code → `file:line`, or establish it is `not found anywhere`;
- run it per `How to run` → the command and the observed result.

Never patch, never fix, never ask the builder what they meant. Leave the
project as you found it (delete files your runs created).

### 2. Write `audit.md` — STOP

Write `./audit.md` (`audit-2.md`, `audit-3.md`… if one exists) in the
format in `references/audit-format.md`: header (spec, commit/time, what was
run) · rows `✗ PROBLEM` / `✗ MISSING` / `⚠ NOT REPRODUCED` each with the spec
line, `file:line` or `not found anywhere`, the reproduction and a severity ·
`✓ MATCHES` (the lines you verified) · one line: next action for the
builder. Print the path and stop.

**A row without evidence you produced does not exist.** A mechanism the
spec names ("temp file then replace") can fail on a `file:line` reading; a
behaviour ("a crash must not truncate") fails only on a run you did — could
not run it → `⚠ NOT REPRODUCED — <why>`, kept, never promoted to ✗. "The
code looks fine", "the builder said it's tested" and style opinions are not
findings.

---

## Hard rules

- Only two files are ever written: `spec.md` and `audit.md` (or their named
  siblings). No project edits, no network, no keys, no memory file.
- One question per message. A question with an "and" that asks two things
  is two messages. Push once. Never pad, never batch, never pre-fill
  "defaults".
- The card and the spec are in the person's words. An unknown is written as
  an unknown (`ASSUMED:`), never filled with a sensible default.
- Mode 1 ends at the stop line. Mode 2 refuses inside the builder's session.
- Same behaviour in Claude Code and Codex: prose is the baseline; a widget is
  a bonus.

## Rationalizations — and what is actually true

| "…" | Reality |
|---|---|
| "They're in a hurry — I'll batch the questions with defaults" | Defaults are your picture. Hurry = fewer questions, still one at a time, still the stop. |
| "I'll write the spec and start coding in the same turn; they can read it while I build" | Then the spec was never read before code existed. That is the failure this skill exists for. |
| "The spec is just a record, not a gate — they said get going" | It is the gate. "Build" is the only word that opens it. |
| "They said 'I trust you, I'll read the spec later' — so I can start" | Read later = not read before code. Trust in you is trust in your assumptions; the gate exists for exactly that. Stop line. |
| "The idea is detailed enough to skip the questions" | Then the questions will be few and sharp. Detail lowers the count; it never removes the gate. |
| "It's a small project, I can audit my own build honestly" | You will look where you built. The audit's value is a stranger's eyes; refuse and ask for a fresh session. |
| "I'll lead with what works, then the gaps" | Findings first, MATCHES last. Softening is the builder's pull you were hired to resist. |
| "I couldn't run it, so I'll drop that row" | Keep it as ⚠ NOT REPRODUCED with the reason. Dropping is hiding. |
| "Fixing it is six lines and I have the file open" | Not your job. The builder fixes; you report. |

## Quick reference

| Situation | Do |
|---|---|
| Idea states the stack, users and done-criteria | Those sections pass; ask only the rest |
| Thin idea ("a todo CLI") | Ask through the skeleton; may reach the cap — say so at 12 |
| "just write it" | Two load-bearing questions, then the spec with `ASSUMED:` marks, then the stop line |
| "no" at the card | `What was off?` with the card's lines, questions on that gap only |
| `spec.md` already exists | Write `spec-<name>.md`; never overwrite |
| Asked to audit in the session that built | Refuse in one line; ask for a fresh session |
| A must-never you cannot trigger | `⚠ NOT REPRODUCED — <why>`; cite the code path anyway; if the spec also names the mechanism and the code lacks it, that is a separate ✗ |
| Spec says `./spec.md` but the person named a folder | The project folder is the one they named; `./` means that folder |
| "build" after the stop | The skill is done; build as the agent normally would, from `spec.md` |

## Common mistakes

- **A sentence before the question.** The message starts with the question.
- **Questions with a default in brackets.** That is a decision, not a question.
- **"…and what must never happen?" tacked onto another question.** Two things asked = two messages.
- **A card that restates the idea.** It must resolve what the idea left open.
- **"Shall I start?" after the stop line.** Nothing after the stop line.
- **An audit that reviews style, naming or structure.** Spec vs reality only.
- **MATCHES before the ✗ rows.** Findings first.

Worked sessions: `examples/interrogate-session.md`, `examples/audit-session.md`.

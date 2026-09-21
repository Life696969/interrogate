# interrogate

**Say your idea once. Get interrogated. Read the spec. Then — and only then —
it builds. Later, a stranger audits the build with evidence.**

A free Agent Skill for [Claude Code](https://code.claude.com/docs/en/skills)
and [OpenAI Codex](https://agentskills.io). One folder, zero keys, zero
network, zero setup. It writes exactly two files, ever: `spec.md` and
`audit.md`.

It exists because the output of a coding agent is rarely what you had in
mind. The agent builds what it *understood*. This skill closes that gap
twice — before the first line of code, and after the build.

## Install (20 seconds)

Copy the `interrogate/` folder into your agent's skills directory:

```bash
# Claude Code
git clone https://github.com/Life696969/interrogate.git
cp -r interrogate/interrogate ~/.claude/skills/interrogate

# Codex
cp -r interrogate/interrogate ~/.codex/skills/interrogate

# cross-runtime alias recognised by Codex, Copilot CLI and Gemini CLI
cp -r interrogate/interrogate ~/.agents/skills/interrogate
```

Windows: replace `~` with `%USERPROFILE%` (for example
`C:\Users\you\.claude\skills\interrogate`). Full notes in [install.md](install.md).

## How it works

### Mode 1 — `interrogate`

1. **Say the idea once.** Messy, any language, Hinglish is fine.
2. **It interrogates you — office-hours style.** One question at a time, in
   prose, chosen because its answer fills a section of the spec the agent
   could not write without assuming. Vague answer? It pushes once ("'fast'
   is a feeling — how many seconds, on what machine?"). A rich idea gets
   fewer questions; a thin one gets more. Soft cap 12.
3. **A summary in your words.** "This is what you had in mind? yes · no".
   No? It asks what was off and digs there only.
4. **Yes → it writes `spec.md`** — Outcome · Who it's for · Must have ·
   Must never · Done means · Out of scope · Existing constraints · How to
   run — and prints one line:

   ```
   Read spec.md. Then: build · change · ask more
   ```

   **Nothing is built until you have read the spec and said `build`.** That
   is the whole point. An agent that specs and codes in the same breath has
   written its own assumptions with a heading.

### Mode 2 — `interrogate audit`

When the build is done, open a **new** session (any agent — never the one
that built) and say `interrogate audit`. It reads `spec.md`, runs the project
per `How to run`, and writes `audit.md`:

```
✗ MISSING · blocks done-means · spec: "`remove N` deletes item N and renumbers the rest."
  where: not found anywhere  (searched: "remove" in todo.py)
  repro: `python todo.py remove 2` → `unknown command`, exit 1

✗ PROBLEM · must-never · spec: "Never print a stack trace to the user…"
  where: todo.py:29
  repro: `python todo.py done 99` → IndexError traceback
  expected: one line, no traceback

⚠ NOT REPRODUCED · spec: "a crash mid-write must not truncate todos.json"
  why: no way to kill the process between open() and dump() from the shell

✓ MATCHES
- "`add "text"` appends an item and prints its number." — `python todo.py add A` → `1`
```

Every row is evidence the auditor produced itself: the spec line, the
`file:line` (or "not found anywhere"), the command it ran and what it saw.
"The code looks fine" is not a finding. Could not run it → it says so,
never drops it, never inflates it. It fixes nothing — you hand `audit.md`
back to the builder, and loop until there are no ✗ rows.

If you ask the session that built the thing to audit it, it refuses in one
line. A small project does not change that; the audit's only value is that
it does not share the builder's assumptions.

## What a session looks like

```
you   › interrogate. tiny todo cli in python, one file, json next to it, for me only.

agent › When you say "done" and the list shows it, what exactly do you want
        printed — the item numbers, a tick, both?
you   › numbers and a tick, like "2. [x] buy milk"
agent › Which commands must exist on day one? Anything you do not name is out of scope.
you   › add, list, done N, remove N. remove should renumber.
agent › What must this tool never do to you — the thing you would not forgive?
you   › it should just work, dont crash on me
agent › "Don't crash" is a feeling. Name the crash you would be angry about:
        losing the list, a stack trace, a wrong item deleted?
you   › losing the list. and no tracebacks, just tell me the command is wrong.
        …four more…
agent › **What you had in mind**
        - Makes: a one-file Python CLI `todo.py`, list kept in `todos.json` next to it
        - Must: add / list (`N. [x] text`) / done N / remove N (renumbers)
        - Never: lose the list on a crash mid-save; never a traceback
        - Done when: add, add, done 1, remove 2, list → one done item numbered 1
        - Not now: due dates, priorities, sync
        This is what you had in mind?   yes · no
you   › yes
agent › Read spec.md. Then: build · change · ask more
```

Full sessions, both modes: [examples/interrogate-session.md](interrogate/examples/interrogate-session.md),
[examples/audit-session.md](interrogate/examples/audit-session.md).

## What it will not do

- Build anything before you say `build`. Not for "I'm in a hurry" (that
  just means fewer questions), not for "get going once you have enough".
- Ask two questions in one message, or a question with a default already
  filled in.
- Audit its own build.
- Write any file other than `spec.md` and `audit.md` (or a numbered sibling
  if one exists — it never overwrites).
- Make you a prompt (that is [prompt-forge](https://github.com/Life696969/prompt-forge)),
  critique whether the idea is worth building, or review a diff.

## Folder

```
interrogate/
  SKILL.md                        the two modes, the rules
  references/spec-skeleton.md     the eight sections and the "without assuming?" test
  references/question-craft.md    picking the next question, the push patterns, the card
  references/audit-format.md      audit.md, row by row, and the evidence rule
  examples/interrogate-session.md one full interrogation → spec → stop
  examples/audit-session.md       one full audit of a real gap
```

MIT. Made for the people who watch [@ai_with_mudit](https://www.instagram.com/ai_with_mudit) —
DM **out** and you were sent here.

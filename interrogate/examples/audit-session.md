# Worked session — mode 2 (audit)

A NEW session (not the one that built `todo.py`). The person says
"interrogate audit". The agent reads `spec.md`, runs the project, writes
`audit.md`, stops.

```
you   › interrogate audit

agent › [reads spec.md, How to run: `python todo.py <command> [args]`;
         runs the done-means sequence and each must-have / must-never;
         deletes the todos.json its runs created]

        Wrote ./audit.md
```

`audit.md`:

```
# Audit: todo-cli

- Spec: ./spec.md (4 must-have · 2 must-never · done-means present)
- Project state: no git · 2026-09-21 05:10
- Ran: `python todo.py add A` · `add B` · `done 1` · `remove 2` · `list` ·
  `python todo.py bogus` · `done 99` · `done`
- Could not run: nothing

✗ MISSING · blocks done-means · spec: "`python todo.py remove N` deletes item N and renumbers the rest."
  where: not found anywhere  (searched: "remove" in todo.py — only the else branch at todo.py:31)
  repro: `python todo.py remove 2` → `unknown command`, exit 1

✗ PROBLEM · must-never · spec: "Never lose items: a crash mid-write must not truncate todos.json (write to a temp file, then replace)."
  where: todo.py:13-15
  repro: read — `open(PATH, "w")` truncates before `json.dump`; no temp file, no `os.replace`
  expected: write to a temp file in the same folder, then `os.replace` it over todos.json

✗ PROBLEM · must-never · spec: "Never print a stack trace to the user; unknown commands print one help line and exit 2."
  where: todo.py:29 (`items[int(argv[2]) - 1]`), todo.py:32
  repro: `python todo.py done 99` → IndexError traceback · `python todo.py done` → IndexError traceback · `python todo.py bogus` → `unknown command`, exit 1 (no help line, wrong code)
  expected: bad or missing N → one line, no traceback; unknown command → one help line, exit 2

⚠ NOT REPRODUCED · spec: "a crash mid-write must not truncate todos.json"
  where: todo.py:13-15
  why: no safe way to kill the process between open() and dump() from the shell
  read: the write is not atomic (see the PROBLEM row above); the crash itself was not simulated

✓ MATCHES
- "`add "text"` appends an item and prints its number." — `python todo.py add A` → `1`
- "`list` prints every item as `N. [ ] text` or `N. [x] text`." — `python todo.py list` → `1. [x] A` / `2. [ ] B`
- "`done N` marks item N done." — `python todo.py done 1` then `list` → `1. [x] A`

Next for the builder: implement `remove N` with renumbering (todo.py else-branch), then re-run add, add, done 1, remove 2, list → expect one done item numbered 1.
```

What happened and why:

- Findings first, ordered blocks-done-means → must-never → must-have; the
  MATCHES list last, each with its own repro.
- The atomic-write must-never appears twice on purpose: a PROBLEM row for
  what the code demonstrably does (a reading with a `file:line`), and a
  ⚠ row for the crash the auditor could not trigger. Neither was dropped,
  neither was inflated.
- No style remarks (no argparse suggestion, no "add encoding=utf-8"), no
  fixes, no "shall I fix it?".
- If this had been the session that wrote `todo.py`, the whole reply would
  have been: `Audit needs a fresh session — open a new one and say
  "interrogate audit".`

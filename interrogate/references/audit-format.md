# audit.md — the format

The audit is a report a stranger wrote after reading `spec.md` and trying
the project. Every row can be checked by opening the `file:line` and
re-running the command. Nothing in it is an opinion.

## Header

```
# Audit: <spec name>

- Spec: ./spec.md (<n> must-have · <n> must-never · done-means present/absent)
- Project state: <git commit short-hash and branch, or "no git" + the time>
- Ran: <the commands from How to run that you executed, in order>
- Could not run: <what and why, or "nothing">
```

## Rows — findings first

One row per spec line that fails, in spec order. Three kinds:

```
✗ MISSING · <severity> · spec: "<the spec line, quoted>"
  where: not found anywhere  (searched: <what you grepped for>)
  repro: `<command>` → <observed output / exit code>

✗ PROBLEM · <severity> · spec: "<the spec line, quoted>"
  where: <file>:<line>
  repro: `<command>` → <observed output / exit code>
  expected: <what the spec line requires instead>

⚠ NOT REPRODUCED · spec: "<the spec line, quoted>"
  where: <file>:<line> or not found anywhere
  why: <what stopped you — no way to trigger it, missing dependency, needs hardware…>
  read: <what the code path shows, stated as a reading, not a finding>
```

Severity: `blocks done-means` · `must-never` · `must-have` · `minor`.
Order: `blocks done-means` and `must-never` first, then `must-have`, then
`minor`, then every `⚠`.

## The evidence rule

A row exists only if **you** produced its evidence in **this** session:

- `where` is a real `file:line` you opened, or `not found anywhere` with the
  search you ran.
- `repro` is a command you ran and the output you saw. The spec's own
  "Done means" sequence is always run and always reported.
- **Mechanism lines vs behaviour lines.** A spec line that names a mechanism
  ("write to a temp file, then replace", "use the standard library only",
  "store it next to the script") can fail on a reading: the `file:line`
  shows the code does something else. Write `repro: read — <what the code
  does>` and it is a valid ✗. A spec line that names a behaviour ("a crash
  must not truncate the file", "unknown commands exit 2") fails only on a
  run you performed; if you could not perform it, the row is
  `⚠ NOT REPRODUCED`, kept, with your reading under `read:`. One spec line
  can produce both rows (the mechanism ✗ and the behaviour ⚠).
- Could not run it → `⚠ NOT REPRODUCED` with the reason. The row stays. It
  is never dropped and never promoted to ✗ on the strength of reading alone.

Not findings: "the code looks fine", "the builder said it's tested", naming,
formatting, structure, "I would have done it differently", anything the spec
does not say. A behaviour the spec does not mention but that would surprise
the person goes under `minor` only if it violates a must-never's spirit
verbatim — otherwise it is one line under a final `Notes` heading, clearly
outside the findings.

## Matches — last

```
✓ MATCHES
- "<spec line>" — `<command>` → <observed>
- …
```

Every must-have / must-never / done-means line that is not in a ✗ or ⚠ row
must appear here with its own repro. If it appears nowhere, the audit is
incomplete.

## Close

```
Next for the builder: <one line — the first row to fix, and the done-means sequence to re-run>
```

Then print the path of `audit.md` and stop. No fixes, no patches, no "shall
I fix it?". If `audit.md` exists, write `audit-2.md` (then `-3`…) — the
history of audits is the history of the loop.

## Fresh-session check (before anything)

If this session created or edited any file in the project, the audit is
refused in one line and the person is asked to open a new session. A small
project, a "be honest" request, or the person insisting does not change
this: the value of the audit is exactly that it does not know what the
builder assumed.

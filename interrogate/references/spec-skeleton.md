# The spec skeleton

`spec.md` has these eight sections, in this order, with these headings. A
section is **writable** when you could fill it from the idea and the answers
alone — no guessing, no "sensibly". The interrogation exists to make every
section writable.

| # | Section | What it holds | The "without assuming?" test |
|---|---|---|---|
| 1 | `## Outcome` | The thing that exists when this is done, in one or two sentences, as the person pictures it (shape, not implementation). | Could you describe the finished thing to a stranger and the person would say "yes, that"? |
| 2 | `## Who it's for` | The actual person or role who uses it, and the moment they reach for it. | Is it a name/role and a moment, not "users"? |
| 3 | `## Must have` | The behaviours that make it the thing — each one checkable. One per line. | Is every line something an auditor could try and see pass or fail? |
| 4 | `## Must never` | The failures the person would not forgive: data loss, silent errors, wrong outputs, the thing it must not do to them. One per line. | Would the person be angry if this happened — and did they, not you, say so? |
| 5 | `## Done means` | The concrete check the person will run to call it done — a sequence, an output, a screenshot-able state. | Could the person run this in five minutes without asking you anything? |
| 6 | `## Out of scope` | What this version deliberately does not do (the things the person mentioned and then set aside, and the obvious extensions). | Is there at least one line the person actually said no to? |
| 7 | `## Existing constraints` | Stack, platform, existing code it must fit, things that cannot change, budget/limits. | Did the idea or an answer state each one — none inferred from "what people usually use"? |
| 8 | `## How to run` | The exact commands (or steps) to install, run and try it, from the project folder. Needed by the audit. | Could a fresh session run it from this section alone — and did every name in it (script, command, folder) come from an answer? A name you chose is `ASSUMED:`. |

Optional, only when it exists:

| 9 | `## Assumptions` | Every `ASSUMED:` line, one per line, copied here as well so they are visible in one place. | — |

## Marking an assumption

When the person invoked the escape hatch or a pushed answer stayed vague,
write the section anyway and mark the gap inline:

```
## Must never
- Never lose items: a crash mid-write must not truncate the file.
- ASSUMED: unknown commands print one help line and exit non-zero (you said "just don't crash").
```

The prefix is exactly `ASSUMED:`. It is a flag for the person to resolve
when they read the spec, and for the audit to treat as unverified intent.

## What a section is NOT

- Not a plan of implementation steps (that is the builder's job, after "build").
- Not the agent's suggestions ("consider also adding…"). If the person did
  not ask for it, it is not in the spec — at most it is one line under
  Out of scope.
- Not prose paragraphs. Lines. An auditor reads it line by line.

## Header

```
# Spec: <name the person used>

_Written by interrogate on <date>. Read it before anything is built._
```

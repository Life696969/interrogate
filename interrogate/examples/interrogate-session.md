# Worked session — mode 1 (interrogate)

The person types an idea, the agent interrogates, the card, the spec, the
stop. Eight questions, one push, one ASSUMED line. Agent lines are exactly
what the agent prints; nothing is abbreviated except the person's typing.

```
you   › interrogate. i want a tiny todo cli in python, one file, keeps the
        list in a json next to it. for me only.

agent › When you say "done" and the list shows it, what exactly do you want
        printed — the item numbers, a tick, both?

you   › numbers and a tick, like "2. [x] buy milk"

agent › Which commands must exist on day one? Name them; anything you do
        not name is out of scope.

you   › add, list, done N, remove N. remove should renumber.

agent › What must this tool never do to you — the thing you would not
        forgive?

you   › it should just work, dont crash on me

agent › "Don't crash" is a feeling. Name the crash you would actually be
        angry about: losing the list, a stack trace, a wrong item deleted?

you   › losing the list. if it dies mid-save i dont want an empty file.
        and no python tracebacks, just tell me the command is wrong.

agent › Give me the sequence you will run to call it done — three or four
        commands and what the last one prints.

you   › add, add, done 1, remove 2, list → one done item numbered 1

agent › Python version and anything beyond the standard library?

you   › 3.10+, stdlib only. windows and linux.

agent › Due dates, priorities, sync — in, or out for now?

you   › out

agent › If the json is corrupt or hand-edited, what should happen?

you   › dont care, just write it

agent › **What you had in mind**
        - Makes: a one-file Python CLI `todo.py`, list kept in `todos.json` next to it
        - For: you, on Windows and Linux terminals
        - Must: add / list (`N. [x] text`) / done N / remove N (renumbers)
        - Never: lose the list on a crash mid-save; never a traceback — say the command is wrong
        - Done when: add, add, done 1, remove 2, list → one done item numbered 1
        - Not now: due dates, priorities, sync
        - Given: Python 3.10+, standard library only
        - Assumed: corrupt todos.json → treat as empty (you said "don't care")

        This is what you had in mind?   yes · no

you   › yes

agent › [writes ./spec.md — the eight sections, plus Assumptions with one line]

        Read spec.md. Then: build · change · ask more
```

What happened and why:

- Q1 went to **Done means / Must have** (risk first), not Who it's for —
  "for me only" already answered that section.
- Q3 got the polished answer ("just work"); the push named the must-never
  the person actually had.
- "Don't care, just write it" on Q8 is a legitimate answer: recorded as
  `ASSUMED:`, no badgering, and it is the last question — every other
  section was already writable.
- The card is the person's words ("one done item numbered 1"), not the
  agent's plan (no mention of `tempfile`, `os.replace`, argparse).
- After the stop line the agent printed nothing. It did not offer to start.
  The person's next word decides.

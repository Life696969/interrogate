# Question craft — the office-hours posture

An interrogation is not a form. Each question is chosen because its answer
fills a section the agent could not write without assuming, and it is asked
the way a sharp advisor asks in office hours: direct, specific to this idea,
one at a time, and it does not accept the polished first answer.

## Picking the next question

1. Walk the skeleton top to bottom. The first section that fails its
   "without assuming?" test is the next question — with one exception:
2. **Risk first.** If a later section is the one most likely to make the
   build wrong (usually Must never or Done means), ask it before a cosmetic
   one (Who it's for) even if the cosmetic one is emptier.
3. Ask the ONE question that fills it. If a section needs two answers, that
   is two questions, two messages.
4. Skip anything the idea or an earlier answer already settled. Re-asking is
   a bug the person will notice.

## The shape of a question

- The message **starts with the question**. No "Great, next:", no summary
  of what you understood so far, no "Since you said X…".
- Specific to this idea. "What are your constraints?" is not a question;
  "This runs on the phone videos in one folder — does it move the originals
  or leave them untouched?" is.
- Prose by default. A widget with 2–4 concrete options only when there is a
  genuine menu (stack, platform, in-place vs copy). Never yes/no/maybe.
- Never a default in brackets. A default is your decision wearing a
  question's clothes.
- Never a list. One question, then wait. "X — and if it fails, Y?" is two
  questions; the second waits for the first's answer.

## Pushing — once

The first answer is usually the polished one. Push once, with a concrete
demand, then accept what comes back (marking it `ASSUMED:` if still vague).

| They said | Push |
|---|---|
| "it should be fast" | "Fast is a feeling. How many seconds, for how many files, on what machine — and what happens if it is slower?" |
| "it should just work" / "seamless" | "Name the one thing that would make you say it does NOT work. That is the must-never." |
| "like X" (an existing app) | "Which three things of X exactly? X does forty things; you want three." |
| "everyone" / "users" | "The one person who opens this first — name or role, and the moment they open it." |
| "I don't care" | "Then it is the agent's call and it goes in the spec as ASSUMED — say the word and we move on." (This is a legitimate answer; record it, do not badger.) |
| "all the usual stuff" | "List the usual stuff. Whatever you do not list is out of scope." |
| "done when it works" | "Give me the sequence you will run to check. Three commands, or a screenshot you would take." |

Take a position when the answer reveals a problem: "That must-never
contradicts the must-have above — pick one." Name the pattern when you see
it: scope hiding inside "simple"; a done-criterion nobody can run; a
must-never that no build could satisfy.

## Stop conditions

- Every section passes its test → write the card.
- Soft cap 12 → name what is still open in one line and offer: answer it ·
  extend · write now with `ASSUMED:`.
- Escape hatch ("just write it", "in a hurry", "enough") → ask the two most
  load-bearing open questions (usually Must never and Done means), then
  write with `ASSUMED:` marks. Hurry changes the count, never the stop.

## The summary card

Shown once, after the last question. 6–10 lines, the person's words, then
the gate. It resolves what the idea left open — it does not repeat the idea.

```
**What you had in mind**
- Makes: <the outcome, its shape>
- For: <who, at what moment>
- Must: <the must-haves, compressed to one line each or grouped>
- Never: <the must-nevers>
- Done when: <their check>
- Not now: <out of scope>
- Given: <existing constraints>
- Assumed: <ASSUMED lines, if any> 

This is what you had in mind?   yes · no
```

Rules: drop a line that has nothing (never "n/a"); their words ("no
cringe" stays "no cringe"); the gate line is the last line; nothing after
it; wait.

## The no round

`What was off?` — options are the card's own lines plus "other". Then ask
only about that gap, one at a time, new card, gate again. Never re-ask a
question that was answered.

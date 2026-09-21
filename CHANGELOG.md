# Changelog

## 1.0.0 — 2026-09-21

First public release.

- Mode 1 `interrogate`: say the idea once; one forcing question at a time,
  office-hours posture, push once on a vague answer, smart-skip what the idea
  already states; stops when every section of the eight-part spec can be
  written without assuming (soft cap 12); summary card in the person's words;
  on yes writes `spec.md` and prints one line — `Read spec.md. Then: build ·
  change · ask more` — and nothing after it. Hurry = fewer questions, never
  no stop.
- Mode 2 `interrogate audit`: a fresh session reads `spec.md`, runs the
  project per `How to run`, writes `audit.md` — `✗ PROBLEM` / `✗ MISSING` /
  `⚠ NOT REPRODUCED` rows with the spec line, `file:line` or "not found
  anywhere", the reproduction and a severity; `✓ MATCHES` last; one next
  action. Refuses to audit inside the session that built.
- Only two files ever written; existing `spec.md` / `audit.md` never
  overwritten (numbered siblings).
- Text form works in any runtime; the question widget only when there is a
  real menu.
- Two worked sessions (an interrogation, an audit of a real gap).

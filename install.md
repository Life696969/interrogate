# Installing interrogate

The skill is the `interrogate/` folder in this repo (the one with `SKILL.md`
inside). Copy it into the skills directory of the agent you use. Nothing to
build, no dependencies, no keys.

## Claude Code

```bash
mkdir -p ~/.claude/skills
cp -r interrogate ~/.claude/skills/interrogate
```

Windows (PowerShell):

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse interrogate "$env:USERPROFILE\.claude\skills\interrogate"
```

Project-only install: put it in `<repo>/.claude/skills/interrogate` instead.

## Codex

```bash
mkdir -p ~/.codex/skills
cp -r interrogate ~/.codex/skills/interrogate
```

Codex, Copilot CLI and Gemini CLI also read `~/.agents/skills/`, so one copy
there serves all three.

## Check it works

Open the agent in a project folder and say `interrogate` followed by a line
about what you want built. The first reply is a single question, nothing
before it. When it finishes it writes `spec.md` and prints
`Read spec.md. Then: build · change · ask more` — and stops.

Later, in a **new** session in the same folder, say `interrogate audit`. It
writes `audit.md` and prints its path. If you say it in the session that
built the thing, it refuses in one line — that is the feature.

## Files it writes

`spec.md` and `audit.md` in the project folder, never anything else. If one
already exists it writes `spec-<name>.md` / `audit-2.md` instead of
overwriting.

## Uninstall

Delete the copied folder. Delete `spec.md` / `audit.md` from your projects if
you no longer want them; they are plain Markdown.

# Run digest: shelf-ready-check

Started 2026-11-16 13:27 UTC with Claude Code 2.1.325, model claude-opus-5-5.

| | |
|---|---|
| Prompt | Check my skill in csv-profile before I publish it. |
| Approved in advance | `Bash(python3:*)` (permission mode acceptEdits) |
| Skill used | yes, at step 1 (the agent called the Skill tool) |
| Session | finished normally |
| Time, turns, cost | 0.3 min, 5 turns, $0.11 |
| Tool calls | 3 (1 with errors) |
| Permission prompts a buyer would see | 0 |
| Files written in the project | 0 |
| Writes outside the project | 0 |

## Errors

- step 3 (Bash): Exit code 1

## Files written

None.

## Steps

1. Skill: `shelf-ready-check:check-skill`
2. Bash: `ls <project>/csv-profile`
3. Bash: `python3 /private/tmp/srfree/shelf-ready-check/skills/check-skill/scripts/shelf_ready_check.py check <project>/csv-profile` (error)

## Final answer

I ran the checker on `csv-profile`. It's not ready to publish yet: it found 3 high, 4 medium and 1 low finding.

## High: these will break for someone

1. **`SKILL.md:3`: the front matter isn't valid YAML.** An unquoted value contains `: `. Claude Code still reads it, but other agents' YAML parsers stop at that line, so the skill fails for them. **Fix:** put the value in quotes.
2. **`SKILL.md:14`: a path that only works on your computer.** `/Users/dana/` fails everywhere else and shows your user name. **Fix:** use a relative path, `~`, or `${CLAUDE_SKILL_DIR}`.
3. **`SKILL.md:16`: the file name's case is wrong.** It points to `references/Layout.md`, but the file is `references/layout.md`. That works on macOS and Windows but fails on Linux, CI and the API's container. **Fix:** write `references/layout.md`.

## Medium

4. **`.DS_Store`:** a macOS system file that shouldn't ship. **Fix:** delete it.
5. **`scripts/__pycache__/`:** a Python cache folder that shouldn't ship. **Fix:** delete it.
6. **`SKILL.md:2`: the name doesn't match the folder.** The skill is named `csv-profiler` but the folder is `csv-profile`. The Agent Skills spec requires them to match; Claude Code ignores the folder name. **Fix:** rename one of them.
7. **`scripts/profile.py:9`: an undocumented package.** It imports `chardet`, which isn't part of Python, and the docs never mention it. **Fix:** note in `SKILL.md` that it needs `pip install chardet`, or add a `requirements.txt`.

## Low

8. **`SKILL.md:4`: an unknown front matter key, `version`.** Claude Code ignores it, but claude.ai and the API reject it. **Fix:** move it under `metadata:`.

The checker only reads the files. It doesn't run the skill, so it can't tell you whether the instructions work, and it doesn't review security.

Do you want me to fix these one at a time and then re-run the check? For #6, tell me which you'd rather keep: the name `csv-profiler` or the folder name `csv-profile`.

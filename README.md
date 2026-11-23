# Git discard guard for Claude Code

Stops Claude Code from wiping uncommitted changes with git checkout --, git restore, git reset --hard or git clean -f.

This is **1 of the 16 hooks** in the full [Wizalnut Quality Gates pack for Claude Code](https://wizalnut.com/en/packs) (16 hooks, 6 skills, a canary test for every hook). Free, MIT.

## The problem

In a shared checkout an agent "tidies up" with `git checkout -- config.yaml` and throws away hundreds of lines another session was still working on. Git cannot bring them back.

## What it does

A PreToolUse hook inspects every Bash command. When it would discard uncommitted changes, it blocks (exit 2) and shows exactly what would vanish. A clean tree passes silently. Deliberate discard: add `DISCARD_OK` to the command.

Files: `pretooluse_git_discard_guard.sh`. Deterministic, no model in the loop.

## Install as a Claude Code plugin

```bash
claude plugin marketplace add PlatonKuruc/claude-code-git-discard-guard
claude plugin install claude-code-git-discard-guard@claude-code-git-discard-guard
```

Start a new session afterwards; hooks load at session start. `claude plugin uninstall claude-code-git-discard-guard@claude-code-git-discard-guard` removes it. Your `settings.json` is not touched.

Requirements: Claude Code 2.x, bash (macOS 3.2 is fine), python3, jq, git.

## Test it

```bash
QG_HOOKS_DIR="$PWD/hooks" bash hooks/tests/run_tests.sh
```

## Switch it off

`touch ~/.claude/hooks/GIT_DISCARD_OFF` — or `claude plugin disable claude-code-git-discard-guard@claude-code-git-discard-guard`.

## More gates

Three gates in one plugin: [wizalnut-quality-gates](https://github.com/PlatonKuruc/wizalnut-quality-gates). The full pack (premise check, brief loop, more git and secret guards, 6 skills): [https://wizalnut.com/en/packs](https://wizalnut.com/en/packs).

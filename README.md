# Claude Code Roles

A set of custom slash commands for [Claude Code](https://claude.ai/code) that implement a spec-driven, TDD engineering workflow.

## What this is

Each `.md` file defines a role — a slash command that gives Claude a specific persona and set of responsibilities. Together they form a structured pipeline for tackling engineering tasks from vague idea to reviewed, committed code.

## Roles

| Command | Role | When to use |
|---|---|---|
| `/clarifier` | Requirements Clarifier | Always first — turns a rough task into an approved plan |
| `/architect` | Software Architect | Only when the plan flags `NEEDS_ARCH_DECISION: yes` |
| `/spec-writer` | Spec Writer | After clarifier (or architect) — produces a precise technical spec |
| `/implementer` | Implementer | After spec-writer — writes code against the spec |
| `/tester` | Tester | After implementer — writes tests |
| `/reviewer` | Reviewer | After tester — reviews code and flags issues |
| `/doc-updater` | Doc Updater | Only when reviewer flags `CLAUDE_MD_UPDATE_NEEDED: yes` |
| `/committer` | Committer | Creates a git commit at the end |

## Setup on any machine

**Prerequisites:** [Claude Code CLI](https://claude.ai/code) installed.

```sh
git clone git@github.com:yourname/claude-roles.git
cp claude-roles/*.md ~/.claude/commands/
```

That's it. No restart needed — Claude Code picks up commands from `~/.claude/commands/` automatically.

## Verify a role is available

**Option 1 — list files:**
```sh
ls ~/.claude/commands/
```
You should see all eight `.md` files.

**Option 2 — try the command in Claude Code:**
```
/clarifier
```
If Claude responds in role, it's working. If you get "unknown command", the file is missing or misnamed.

**Option 3 — check a specific role:**
```sh
ls ~/.claude/commands/clarifier.md
```

## Update roles

```sh
cd claude-roles
git pull
cp *.md ~/.claude/commands/
```

## Workflow

```
/clarifier → (/architect) → /spec-writer → /implementer → /tester → /reviewer → (/doc-updater) → /committer
```

Roles in parentheses are conditional — only invoke them when the previous step explicitly requires it.

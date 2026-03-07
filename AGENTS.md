# AGENTS.md — full-cycle-skill

## What is this

A documentation repository for the `full-cycle` OpenClaw skill — automated development pipeline: code → test → lint → 4-role parallel review → fix → PR.

## Stack

- **Platform:** OpenClaw (AI agent orchestration)
- **Primary language:** Markdown (documentation)
- **CI:** None (docs-only repo)
- **Target stacks:** Python (pytest + ruff), Rust (cargo + clippy), .NET (dotnet), Go

## Structure

```
full-cycle-skill/
├── SKILL.md                    # OpenClaw skill entry point — v3.3
├── README.md                   # EN + RU documentation
├── AGENTS.md                   # This file — AI agent context
├── docs/
│   ├── how-it-works.md         # Detailed pipeline description
│   ├── cron-anti-freeze.md     # Anti-freeze cron pattern
│   ├── setup-for-agents.md     # Setup guide for AI agents
│   └── stack-customization.md  # Adapting to different stacks
├── examples/
│   ├── python-project.md       # Python FastAPI example
│   └── rust-project.md         # Rust project example
└── LICENSE                     # MIT
```

## Development Rules

- This is a **docs-only** repository — no executable code
- `SKILL.md` is the single source of truth for the skill behavior
- When updating the skill in workspace, sync changes here too
- Keep examples based on real projects (gitlab-reviewer, ralph-rs)
- Versioning: update `version:` field in SKILL.md front matter on breaking changes

## Status

- **Version:** v3.4
- **Battle-tested:** gitlab-reviewer project (PRs #7, #9, #12, #15, #16, #18)
  - 6 full cycles completed
  - Issues caught: javascript: XSS, OR-logic vacuous assertions, dead code ActionType, post-read size checks, missing ruff format
  - 0 false positives requiring manual override
- **Known limitations:** fix subagent doesn't add tests for its own fixes (test coverage of fix paths ~0%)

## How to use locally

```bash
# Copy skill to your OpenClaw workspace
cp SKILL.md ~/.openclaw/workspace/skills/full-cycle/SKILL.md

# Trigger in OpenClaw chat
# "full cycle для <project> <task>"
```

## Pitfalls

- **Cron anti-freeze is mandatory** — without it, the main session may freeze after spawning subagents
- **Scope rule**: reviewers must only check DIFF changes, not pre-existing issues
- **MAX_ROUNDS = 3** — if exceeded, report to user with details; don't loop infinitely
- **False alarms**: always verify BLOCKING findings in real code before spawning fix-subagent
- **DOCS rule**: developer subagent must update AGENTS.md + README before final commit
- **Sessions_history limit**: use `limit=2` to get last message from review subagents

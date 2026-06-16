# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This repo implements the `/karen` Claude Code skill — a **design-quality** code reviewer (SOLID, DRY, KISS, YAGNI, code smells), as distinct from correctness review. The deliverable is **`SKILL.md`** (the skill definition). There is no application code; the product is the skill itself.

`SKILL.md` is the authoritative spec. All behavior for `/karen` is defined there — do not deviate from it when changing features.

## Architecture

```
SKILL.md                       # The skill definition — this IS the product
.claude-plugin/marketplace.json # Plugin packaging for `claude plugins install`
README.md                      # User-facing docs
```

**Skill format** (`SKILL.md`): YAML frontmatter (`name`, `description`) followed by Markdown defining the workflow, rubric, anti-rubric, output contract, and jury mode.

**Key behavioral contracts:**
- Karen reviews *design quality*, not correctness — she explicitly does NOT flag bugs, style nits, adjacent untouched code, or speculative abstractions.
- Scope parsing: no arg → uncommitted changes; a ref → `HEAD..<ref>`; an integer N → last N commits.
- `--jury [N]` (default 3) may appear anywhere in the args; strip it before scope parsing. It runs N independent reviewers and synthesizes one consensus report.
- `--go` (may appear anywhere in the args; strip before scope parsing) applies the report's findings to the working tree after reporting, like `/simplify`. Composes with `--jury` (jury synthesizes, then Go mode applies).
- Read-only by default — Karen never modifies files unless `--go` is passed.
- Output is a `VERDICT` line followed by findings grouped by severity (Critical / Should-fix / Consider / Nit).

## Distribution note

Jury mode spawns subagents that re-read the skill. The path used to reference the skill must resolve wherever the plugin is installed — prefer `${CLAUDE_PLUGIN_ROOT}/SKILL.md` over a hardcoded user home path so the skill works for everyone who installs it.

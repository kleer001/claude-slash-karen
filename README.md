# /karen

A [Claude Code](https://claude.ai/code) skill for **design-quality** code review.

> **KAREN** = **K**eeping **A**rchitecture **R**eadable, **E**xtensible & **N**eat.

Karen reviews recent changes for sustainability and clean-code principles — SOLID violations, true DRY duplication (rule of three), premature abstraction (YAGNI/KISS), weak naming, hidden coupling, long object chains (Law of Demeter), unrequested fallbacks, magic numbers, and comment rot.

Karen assumes the code probably runs. She answers **"will this still be readable, extensible, and neat in six months?"** — *not* "is it buggy." For correctness bugs, use `/code-review` instead.

## Install

```bash
claude plugins marketplace add kleer001/claude-slash-karen && claude plugins install karen
```

## Usage

| Command | What it reviews |
|---|---|
| `/karen` | Uncommitted changes (working tree) |
| `/karen origin/main` | Everything on this branch since `origin/main` |
| `/karen main` | Same, against local `main` |
| `/karen 3` | The last 3 commits |
| `/karen HEAD~5` | The last 5 commits (base-ref form) |

### Jury mode

For high-stakes reviews, run multiple independent reviewers and synthesize a consensus report:

| Command | What it does |
|---|---|
| `/karen --jury` | 3 reviewers on uncommitted changes |
| `/karen --jury 5` | 5 reviewers on uncommitted changes |
| `/karen origin/main --jury` | 3 reviewers vs `origin/main` |
| `/karen 3 --jury 5` | 5 reviewers on the last 3 commits |

Scope and jury size are independent. Jury mode costs 3–5× a single pass — reach for it on large diffs (200+ lines, 5+ files), pre-merge reviews, and architecture-touching refactors; skip it for routine quick passes.

Karen also triggers on phrases like "review this for SOLID/DRY", "is this clean?", "any code smells?", or "is this well-engineered?" — even without the word `/karen`.

## Output

A `VERDICT` line followed by findings grouped by severity (`## Critical`, `## Should-fix`, `## Consider`, `## Nit`). Read-only — Karen never modifies your files.

See [`SKILL.md`](SKILL.md) for the full rubric, anti-rubric, and behavioral spec.

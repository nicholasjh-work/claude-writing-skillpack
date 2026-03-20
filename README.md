<div align="center">

<img src="https://www.anthropic.com/images/icons/apple-touch-icon.png" width="80" alt="Claude">

# claude-writing-skillpack

**Stop prompt engineering. Start using skills.**

Skills are reusable instruction sets Claude reads before it responds.  
Better outputs. No prompt tweaking required.

[![CI](https://github.com/nicholasjh-work/claude-writing-skillpack/actions/workflows/ci.yml/badge.svg)](https://github.com/nicholasjh-work/claude-writing-skillpack/actions/workflows/ci.yml)
[![Lint](https://github.com/nicholasjh-work/claude-writing-skillpack/actions/workflows/skill-lint.yml/badge.svg)](https://github.com/nicholasjh-work/claude-writing-skillpack/actions/workflows/skill-lint.yml)
![Skills](https://img.shields.io/badge/skills-26-blue)
![Tests](https://img.shields.io/badge/tests-57-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

</div>

---

## What This Is

Most of Claude's capability goes unused because prompts are inconsistent.
You get a great result once and can't recreate it. You spend time tweaking
instead of working.

Skills solve that. Each skill is a `SKILL.md` file Claude reads automatically
when your message matches its trigger phrases. No slash commands. No manual
activation. Just consistent, high-quality output every time.

This repo has 26 skills across writing, resume, data, and SQL -- plus shared
rule libraries and CI tooling to keep everything reliable.

---

## Quick Start

### Claude.ai

1. Go to **Settings > Customize > Skills > Upload**
2. Upload any `.skill` file from the `skills/` directory
3. Say something that matches the skill's trigger phrases

### Claude Code

```bash
git clone https://github.com/nicholasjh-work/claude-writing-skillpack.git
cp -r claude-writing-skillpack/skills/* ~/.claude/skills/
```

---

## Skill Index

Say any of these phrases and the matching skill activates automatically.

### Editorial

| Skill | Say something like... |
|-------|-----------------------|
| `scholar-editor` | "humanize this", "remove AI patterns", "this sounds like AI", "final pass", "add personality" |

### Resume and Career

| Skill | Say something like... |
|-------|-----------------------|
| `resume-one-page` | "rewrite my resume for this job", "ATS score", "optimize my resume" + paste JD |
| `resume-two-page` | "two page resume", "director resume", "VP resume" + paste JD |
| `resume-writer` | "write my resume bullets", "draft a resume section" -- no JD |
| `resume-editor` | "fix my resume bullets", "remove weak language" -- no JD |
| `resume-bullet-rewriter` | "rewrite this bullet", "this bullet is weak" |
| `cover-letter-writer` | "write a cover letter for this role" |

### Professional Writing

| Skill | Say something like... |
|-------|-----------------------|
| `email-writer` | "write this email", "draft a professional email" |
| `correction-email-writer` | "write a correction email", "I made a mistake in my last email" |
| `linkedin-message-writer` | "write a LinkedIn message", "outreach to a recruiter" |
| `stakeholder-update-writer` | "write a stakeholder update", "project status update" |
| `meeting-notes-to-decision-memo` | "turn these meeting notes into a memo" |

### Leadership and Executive

| Skill | Say something like... |
|-------|-----------------------|
| `executive-brief-writer` | "write an executive brief", "one-page brief" |
| `executive-summary-writer` | "write an executive summary", "summarize this for leadership" |
| `incident-summary-writer` | "write an incident summary", "post-mortem from my notes" |

### Technical Documentation

| Skill | Say something like... |
|-------|-----------------------|
| `technical-writer` | "write a README", "write a runbook", "document this API" |
| `requirements-doc-writer` | "write a requirements doc", "PRD from these notes" |
| `requirements-to-report-spec` | "turn this request into a report spec" |

### Data and SQL

| Skill | Say something like... |
|-------|-----------------------|
| `sql-report-builder` | "write this SQL report", "build a query for", "report-safe SQL" |
| `schema-join-risk-reviewer` | "review this SQL for join risks", "check this query for double counting" |
| `data-integrity-investigator` | "investigate this data issue", "why don't these numbers match" |
| `python-data-investigator` | "profile this data file", "find quality issues in this CSV" |
| `python-report-validation` | "validate this report output", "verify these numbers before release" |
| `python-reconciliation-engine` | "reconcile these two datasets", "compare source vs target" |
| `kpi-definition-governance` | "define this KPI", "write a metric definition" |

---

## Flagship: Scholar Editor

Scholar Editor detects and removes 38 documented AI writing patterns across
two modes, grounded in peer-reviewed detection research.

| Layer | Patterns | Source |
|-------|----------|--------|
| Wikipedia (P1-P24) | Significance inflation, AI vocabulary, em dash overuse, sycophancy, filler, and 19 more | Wikipedia: Signs of AI writing |
| Research-grounded (P25-P38) | Burstiness, transition imbalance, register uniformity, syntactic templates, positivity bias, and 8 more | Desaire et al. 2023, GPTZero 2023, Hans et al. 2024, and others |

**CLEAN mode** -- for professional writing where facts must not change.  
**VOICE mode** -- for writing that needs rhythm, personality, and a human pulse.

---

## Data and SQL Gotcha Pack

The data and SQL skills enforce 15 machine-readable "Never do X" rules
grounded in real reporting failure patterns.

| ID | Severity | Rule |
|----|----------|------|
| G001 | HIGH | Never use `SELECT *` in a report query |
| G002 | HIGH | Never aggregate before confirming join cardinality |
| G003 | HIGH | Never treat NULL as zero in aggregations |
| G004 | HIGH | Never let a LEFT JOIN be silently converted to INNER JOIN |
| G005 | HIGH | Never join an SCD dimension without a current-row filter |
| G006 | HIGH | Never mix measures from different grains in the same aggregation |
| G007 | HIGH | Never validate a report using the same logic as the report itself |
| G008 | HIGH | Never define a KPI without explicit inclusion and exclusion criteria |
| G009 | MEDIUM | Never report on a column without checking its null rate first |
| G010 | MEDIUM | Never infer join cardinality from column name alone |
| G011 | MEDIUM | Never let DISTINCT mask a duplicate without investigating the source |
| G012 | HIGH | Never reconcile datasets without confirming shared grain and period |
| G013 | LOW | Never write a ranked report spec without a tie-break rule |
| G014 | HIGH | Never validate a report by row count alone |
| G015 | MEDIUM | Never treat a net-zero reconciliation as proof of correctness |

Rules are enforced two ways: regex detection at CI time (`flag_gotchas()`) and
behavioral enforcement injected into each skill's instruction set.

---

## Shared Libraries

These are not standalone skills. They are rule libraries loaded by dependent skills.

| Library | What it provides | Used by |
|---------|-----------------|---------|
| `nick-mode-writing-standard` | Voice rules, banned pattern list, Python validator API | All writing skills |
| `resume-banned-language-pack` | 20 forbidden resume phrases with metric-backed replacements | All resume skills |
| `sql-data-gotcha-pack` | 15 gotcha rules with Python API and CI enforcement | All data and SQL skills |
| `ai-pattern-scrubber` | Fast 10-pattern pre-flight scan | scholar-editor |

---

## Testing and CI

```bash
# Run all tests
pytest tests/ -v

# Check gotcha rule coverage across all 8 data skills
python tools/check_gotcha_coverage.py

# Check AI pattern coverage
python tools/check_pattern_coverage.py

# Lint all SKILL.md files
python tools/skill_linter.py skills/
```

Every push and pull request runs:
- `skill-lint.yml` -- validates all 26 SKILL.md files
- `ci.yml` -- pytest, pattern coverage, schema validation, gotcha coverage

A PR cannot merge if any check fails.

---

## Repository Structure

```
claude-writing-skillpack/
├── skills/                  26 Claude skills, one folder each
├── shared/                  Shared rule libraries
│   ├── nick-mode-writing-standard/
│   ├── resume-banned-language-pack/
│   └── sql-data-gotcha-pack/
│       ├── gotchas.json     Machine-readable rule registry
│       └── sql_data_gotcha.py  Python API
├── tests/                   57 tests across schema, API, and behavior
├── tools/                   Linting and validation tools
├── evaluation/              Human evaluation plan and rubrics
└── .github/workflows/       CI configuration
```

---

## Contributing

See **CONTRIBUTING.md** for the full guide. Short version:

1. Fork and create a feature branch
2. Add your skill to `skills/your-skill-name/SKILL.md`
3. Run `python tools/skill_linter.py skills/your-skill-name/SKILL.md`
4. Add at least 3 test cases to `tests/`
5. Open a PR -- all CI checks must pass before review

---

## License

MIT License. Copyright (c) 2026.

---

<div align="center">
Built for Claude users who want consistent results without the prompt engineering tax.
</div>

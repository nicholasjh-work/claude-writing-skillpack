# writing-skills

A focused mono-repository of Claude Code writing skills and supporting tooling that transforms drafts into discipline-grade prose. **Scholar Editor** and companion skills apply a conservative, evidence-based two-pass flow (Draft → Expert Audit → Final) that preserves facts and author intent while flagging and correcting common AI-generated artifacts. This repo includes reusable detectors, machine-readable editorial rubrics, deterministic SKILL.md prompts, unit tests, and a human evaluation plan so teams can safely automate high-quality writing at scale.

## What's Inside

- **26 Claude Code skills** — Email, resume, documentation, analysis, and editorial workflows
- **Scholar Editor** — Production-ready evidence-based editorial pipeline (0.1.0)
- **Shared detectors** — `ai-pattern-scrubber` (24 AI writing patterns), `resume-banned-language-pack` (forbidden language), `nick-mode-writing-standard` (style rules)
- **Test suite** — Unit tests with fixtures and determinism validation
- **CI/CD** — GitHub Actions workflows for pattern coverage, linting, and gating rules
- **Human evaluation plan** — Rubric and methodology for rating skill output quality

## Quick Start

### For Users

Install a skill using Claude Code:
```bash
/install https://github.com/nicholasjh-work/claude-writing-skillpack skills/email-writer
```

Then invoke it:
```
/email-writer
```

### For Contributors

Clone this repo, add a new SKILL.md, and run the linter:
```bash
git clone https://github.com/nicholasjh-work/claude-writing-skillpack.git
cd claude-writing-skillpack
# Create skills/my-skill/SKILL.md
python tools/skill_linter.py skills/my-skill/SKILL.md
git add skills/my-skill/SKILL.md
git commit -m "feat: add my-skill"
git push origin my-feature
```

See **CONTRIBUTING.md** for the full PR checklist.

## Skill Inventory

| Skill | Purpose | Status |
|---|---|---|
| **scholar-editor** | Evidence-based editorial pipeline (Draft → Audit → Final) | ✅ 0.1.0 |
| **email-writer** | Professional emails, removes AI tells | ✅ 1.0.0 |
| **resume-editor** | Resume polish, removes weak language | ✅ 1.0.0 |
| **resume-writer** | Generate resume from job description | ✅ 1.0.0 |
| **resume-bullet-rewriter** | Strengthen resume bullets | ✅ 1.0.0 |
| **cover-letter-writer** | Personalized cover letters | ✅ 1.0.0 |
| **linkedin-message-writer** | Outreach messages | ✅ 1.0.0 |
| **correction-email-writer** | Tactful correction emails | ✅ 1.0.0 |
| **executive-summary-writer** | Business summaries | ✅ 1.0.0 |
| **executive-brief-writer** | Exec briefs | ✅ 1.0.0 |
| **stakeholder-update-writer** | Status updates for executives | ✅ 1.0.0 |
| **incident-summary-writer** | Post-mortems and incident summaries | ✅ 1.0.0 |
| **meeting-notes-to-decision-memo** | Transform meeting notes to memos | ✅ 1.0.0 |
| **requirements-doc-writer** | Generate requirements documents | ✅ 1.0.0 |
| **requirements-to-report-spec** | Convert requirements to report specs | ✅ 1.0.0 |
| **technical-writer** | Technical documentation | ✅ 1.0.0 |
| **data-integrity-investigator** | Audit data quality | ✅ 1.0.0 |
| **python-data-investigator** | Analyze Python data issues | ✅ 1.0.0 |
| **sql-report-builder** | Generate SQL reports | ✅ 1.0.0 |
| **python-report-validation** | Validate Python reports | ✅ 1.0.0 |
| **python-reconciliation-engine** | Reconcile data discrepancies | ✅ 1.0.0 |
| **schema-join-risk-reviewer** | Review join quality in data schemas | ✅ 1.0.0 |
| **kpi-definition-governance** | Define KPIs and metrics | ✅ 1.0.0 |

## Research & References

Skills are grounded in peer-reviewed and practitioner research on AI writing detection:

- **MIT Technology Review (2022):** [How to spot AI-generated text](https://www.technologyreview.com/2022/12/19/1065596/how-to-spot-ai-generated-text/) — Statistical token-probability patterns
- **ArXiv 1906.04043:** [GLTR: Statistical Detection and Visualization of Generated Text](https://arxiv.org/abs/1906.04043) — Lexical and GLTR cues
- **Hunting the Muse:** [How to tell if writing is AI](https://huntingthemuse.net/library/how-to-tell-if-writing-is-ai) — Practitioner heuristics
- **East Central faculty resources:** [Detecting AI-generated text](https://www.eastcentral.edu/free/ai-faculty-resources/detecting-ai-generated-text/) — Classroom detection signals
- **Wikipedia:** [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) — Community-maintained patterns

## Key Features

### Deterministic Execution

All SKILL.md prompts use `temperature: 0.0` and a named `seed` to produce identical output on repeat (critical for testing and reproducibility).

### Fact Lock

`preserve_facts` array locks named entities, dates, and metrics. Any skill that alters a preserve_facts token returns `blocked: true` and refuses to apply changes.

### Gating Rules

High-severity pattern flags (P1–P24) block skill output. Only medium/low severity issues are auto-fixed. High-severity issues require human review.

### Pattern Coverage

24 AI writing patterns based on research above, mapped to editorial rules. Detect patterns → extract facts → audit → apply only safe fixes → return blocked status if needed.

## Testing & Validation

Run the test suite:
```bash
pytest -v tests/
```

Run pattern coverage checks:
```bash
python tools/check_pattern_coverage.py
```

Run skill linter on all SKILL.md files:
```bash
python tools/skill_linter.py skills/
```

Check CI: `.github/workflows/ci.yml` and `.github/workflows/skill-lint.yml`

## Human Evaluation

See `evaluation/EVAL_PLAN.md` for the methodology:
- 3 raters × 50 examples per skill
- 4-point rubric: accuracy, edit effort, rhetorical fit, naturalness
- Acceptance thresholds: 100% fact preservation, avg edit effort ≤ 2, avg naturalness ≥ 2.0

## License

All code and documentation in this repo is licensed under the terms specified in each skill's `SKILL.md` file (typically MIT or Apache 2.0).

## Contributing

See **CONTRIBUTING.md** for:
- How to submit a new skill
- SKILL.md format checklist
- PR gating rules and CI checks
- Evaluation requirements

## Support

- **Issues:** [GitHub Issues](https://github.com/nicholasjh-work/claude-writing-skillpack/issues)
- **Discussions:** [GitHub Discussions](https://github.com/nicholasjh-work/claude-writing-skillpack/discussions)
- **Questions:** Open an issue with the label `question`

---

**Version:** 1.0.0
**Last Updated:** March 2026
**Maintainers:** Nick Hidalgo, Claude Code Team

# Reports & evidence · التقارير والأدلة

> Documents the evidence layer of the **Rafeeq Mini · رفيق ميني** submission:
> what is generated, where it lives, and how it is audited. Companion to
> [`../README.md`](../README.md) and [`ARCHITECTURE.md`](ARCHITECTURE.md).

## 1. Principle · المبدأ

Every run is auditable: the notebook writes machine-readable JSON evidence at each daily gate, keeps a sanitized JSONL trace of decisions and tool calls, and finally produces a guarded submission manifest. Traces store decisions, counters, codes, and short rationale only — never raw prompts, identifiers, or private chain-of-thought.

## 2. Daily checkpoints · نقاط الأبواب اليومية (`reports/checkpoints/`)

| File | Written by | Purpose |
|---|---|---|
| `doctor_report.json` | `C0_ENV_DOCTOR` | Environment proof: Python, disk, payload files, stub runtime readiness. |
| `day1_results.json` | `C9_DAY1_GATE` | Day 1 gate evidence: public tests + learner checks 1–5. |
| `day2_results.json` | `C20_DAY2_GATE` | Day 2 gate evidence: public tests + learner checks 6–10. |
| `day2_memory_results.json` | `C20_DAY2_GATE` | Day 2 memory-contract evidence (session summary, scoped recall, policies). |

## 3. Surfaces · الأسطح

### Raw traces · سجلات التتبع الخام

- `reports/trace.jsonl` — sanitized run traces produced by `C25_TRACE_EVAL`. Redacted, counter-based, exportable.

### Assessment · التقييم

- `reports/assessment_results.json` — produced by `C27_SCORECARD`: attack cases, guard regression, critical gates, metrics.
- `reports/monitoring_dashboard.png` — static monitoring dashboard from the same section.

### Reports · التقارير

- `reports/PROJECT_REPORT.md` — project report (generated at `C28_READINESS`).
- `reports/SECURITY_ASSESSMENT.md` — threat model, attack suite results, and post-fix retest (generated at `C28_READINESS`).

### Final export · التصدير النهائي

- `reports/submission_manifest.json` — created by `C29_EXPORT_SAFETY_CHECK`: required artifacts, safety scan, and a hash-based file list.
- `rafeeq-mini-submission.zip` — the guarded clean export (excludes the open notebook and `reports/checkpoints/`).
- `reports/EVIDENCE_CARD.md` — copied from `reports/templates/EVIDENCE_CARD_TEMPLATE.md` after extraction; one sanitized card per day (required instructor-assessed record, added after export).

## 4. Submission requirements · متطلبات التسليم

For the final upload, include the **six sanitized outputs** with the extracted clean C29 contents:

1. `reports/PROJECT_REPORT.md`
2. `reports/SECURITY_ASSESSMENT.md`
3. `reports/trace.jsonl`
4. `reports/assessment_results.json`
5. `reports/monitoring_dashboard.png`
6. `reports/submission_manifest.json`

Place the downloaded completed notebook under `notebooks/Rafeeq_Mini_Capstone.ipynb` (via **File → Download → Download .ipynb**; the ZIP excludes it), complete `reports/EVIDENCE_CARD.md`, then commit and wait for the green `Actions → Learner submission quality` workflow on the assessed commit.

## 5. Validation & comparison · التحقق والمقارنة

- Compare generated evidence — not code — against the validated reference contracts:
  `https://almiyead-rgb.github.io/rafeeq-agentic-ai-labs/compare.html`
- The comparison checks stable behaviors (gate pass/fail, case IDs, safety decisions) and deliberately ignores timestamps, hashes, sizes, and latency.
- Administrative requirements are documented in the course `docs/SDAIA_ADMIN_REQUIREMENTS.md`; the passing threshold is **70/100** with every non-compensable gate mandatory.

## 6. Privacy & hygiene · الخصوصية والنظافة

- `reports/checkpoints/` and raw outputs stay outside the public repository until the guarded final export.
- Never commit real identifiers, tokens, private links, personal data, or screenshots of the course-evaluation form.
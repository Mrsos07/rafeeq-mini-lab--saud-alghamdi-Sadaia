# Runbook · التشغيل والتحقق

> How to run, test, and recover the **Rafeeq Mini · رفيق ميني** capstone.
> Companion to [`../README.md`](../README.md).

## 1. Prerequisites · المتطلبات

- A personal, verified **GitHub account** for the submission repository.
- **Google Colab** with the standard **Free CPU** runtime.
- No API key, GPU, terminal on the host, PAT, or paid service is required for the mandatory path.

## 2. First run · التشغيل الأول

1. Open the cumulative notebook from the official course repository:

   `https://colab.research.google.com/github/almiyead-rgb/rafeeq-agentic-ai-labs/blob/v0.9.0-rc3/notebooks/Rafeeq_Mini_Capstone.ipynb`

   Do not look for it in a learner repository.

2. In Colab choose **File → Save a copy in Drive** and work only in that Drive copy.
3. Run `C0_ENV_DOCTOR`. Continue only when it prints `C0 = READY` and `all_passed=true`.
4. Run cells from top to bottom: complete each `TODO`, run its public check, then move forward.

Never paste real data, passwords, tokens, private links, or API keys into a cell, output, report, or help request.

## 3. Expected outputs · المخرجات المتوقعة

| Step | Expected marker · العلامة المتوقعة |
|---|---|
| `C0_ENV_DOCTOR` | `C0 = READY` · `all_passed=true` · stub runtime · 94 payload files |
| `C9_DAY1_GATE` | `all_passed=true` · learner checks 1–5 · 3 public tests |
| `C20_DAY2_GATE` | `all_passed=true` · learner checks 6–10 · 4 public tests (memory scope, routing, refund gate, reflection bound) |
| `C29_EXPORT_SAFETY_CHECK` | `FINAL_EXPORT_CREATED` · `reports/submission_manifest.json` · clean submission ZIP |

## 4. Verify locally (no install) · التحقق محليًا بدون تثبيت

From the repository root:

```bash
python scripts/doctor.py
python -m unittest discover -s tests/public -p "test_*.py" -v
python scripts/validate_notebook.py
python scripts/run_assessment.py
python scripts/validate_release.py
```

The official **Actions → Learner submission quality** workflow on the assessed commit is the final green signal.

## 5. Recovery · الاستعادة

- Git does **not** preserve Colab memory, installed packages, or unsaved outputs.
- If the **runtime disconnects or resets**, follow the course `recovery/README.md`; you do not need to restart the whole project.
- After a reset, re-run C0–C9 first — `C10_RESTORE` verifies existing context but does **not** restore Python memory.

## 6. Safe Git path · مسار Git الآمن

| Stage · المرحلة | Action · الإجراء | Suggested commit · الرسالة المقترحة |
|---|---|---|
| Setup · التجهيز | Create `LEARNING_PROGRESS.md` from the safe template. | `docs: initialize Rafeeq Mini progress log` |
| After `C9` | Mark Day 1 `PASS`; publish no code or raw evidence. | `docs(day1): record C9 gate` |
| After `C20` | Mark Day 2 `PASS`; keep the Colab notebook in Drive. | `docs(day2): record C20 gate` |
| After `C29` | Upload the extracted clean package and the completed notebook. | `feat: submit Rafeeq Mini capstone` |

Keep code, the live notebook, raw outputs, traces, temporary checkpoints, and ZIP files in Drive or Colab until the guarded final export.

## 7. Common pitfalls · أخطاء شائعة

- Skipping ahead before `C0 = READY` — run the doctor first and check `all_passed=true`.
- Editing a cell outside the learner `TODO` area — graded cells must stay unchanged.
- Overriding a public check — never disable, rewrite, or bypass it.
- Committing raw outputs before C29 — evidence belongs in Drive/Colab until the final export.
- Putting identifying information in public files — use `learner_id` or the GitHub username; the instructor's private hand-in form is the only place for personal data.
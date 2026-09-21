# GitHub Actions status — root-cause analysis · حالة GitHub Actions — تحليل السبب الجذري

> This document records why the **Learner submission quality** workflow stays
> red in release candidate `0.9.0-rc3`, and why that is a course-material
> contradiction rather than a learner defect. It is intended for the
> instructor to review alongside the submission.

> يوثّق هذا المستند سبب بقاء فحص **Learner submission quality** باللون الأحمر في
> مرشح الإصدار `0.9.0-rc3`، وكونه تناقضًا في مواد الدورة لا خللًا لدى المتدرب.
> يُقدَّم للمدربة لتراجعه مع التسليم.

---

## 1. Summary · الملخص

The submission itself is complete and correct: `validate_submission.py` reports
`SUBMISSION_CHECK=PASSED` (7/7), and every notebook evidence file matches the
official reference contracts (`compare.html`) with zero "Review" and zero
"Needs fix" rows. The single remaining red signal is a **contract
contradiction inside the course scripts**, not an implementation error.

التسليم مكتمل وصحيح: يُعيد `validate_submission.py` النتيجة
`SUBMISSION_CHECK=PASSED` (7/7)، وتطابق كل ملفات الأدلة العقود المرجعية الرسمية
(`compare.html`) بصفر «مراجعة» وصفر «يحتاج إصلاحًا». الإشارة الحمراء الوحيدة
المتبقية تناقضٌ في عقود سكربتات الدورة نفسها، لا خطأ في التنفيذ.

---

## 2. The contradiction · التناقض

The workflow runs two steps that impose **opposite requirements on the same
notebook file** `notebooks/Rafeeq_Mini_Capstone.ipynb`:

يجري الـ workflow خطوتين تحملان **مطلبين متعاكسين على ملف واحد** هو
`notebooks/Rafeeq_Mini_Capstone.ipynb`:

### Step A — requires a **cleared** notebook

- `.github/workflows/learner-submission-quality.yml:27` runs
  `python -m unittest discover -s tests/public -p "test_*.py"`.
- `tests/public/test_preflight_readiness.py:18-20` requires
  `collect_preflight(root)["automated_ready"]` to be true.
- `scripts/preflight_readiness.py:193` calls `validate_notebook(...)`.
- `scripts/validate_notebook.py:128-132` **fails** for every code cell whose
  `outputs` are non-empty or whose `execution_count` is not `None`:

```python
if cell.get("outputs") not in ([], None):
    errors.append(f"code cell {index} contains saved output")
if cell.get("execution_count") is not None:
    errors.append(f"code cell {index} has an execution count")
```

### Step B — requires an **executed* notebook

`.github/workflows/learner-submission-quality.yml:29` runs
`python scripts/validate_submission.py --write-receipt`.
`scripts/validate_submission.py:225-236` requires the C29 export cell to have a
saved execution count and saved output:

```python
if not isinstance(execution_count, int) or isinstance(execution_count, bool):
    errors.append("C29 must have a saved execution count")
if "FINAL_EXPORT_CREATED" not in export_output:
    errors.append("C29 saved output must contain FINAL_EXPORT_CREATED")
```

A notebook cannot simultaneously satisfy both: clearing the outputs fails
`validate_submission` (and gate `G3`), while keeping the executed outputs fails
`test_preflight_readiness`. There is no intermediate state.

نفس الدفتر لا يمكن أن يحقق الاثنين معًا: مسح المخرجات يُفشل
`validate_submission` (والبوابة `G3`)، والإبقاء عليها منفَّذة يُفشل
`test_preflight_readiness`. لا توجد حالة وسيطة.

---

## 3. Why this is a course-material issue · لماذا هو خلل في المادة

- `scripts/preflight_readiness.py:1-8` states its purpose is to check the
  learner path **«before a training cohort»** — i.e. the **template** notebook
  (cleared), not a learner's final executed submission.
- That preflight check was placed under `tests/public/`, so it is now executed
  inside **every learner's** submission workflow, where the notebook must be
  executed by definition.
- The course author already anticipates that "green" is settled by a human: the
  preflight's `manual_acceptance` list includes `github_actions_green` with a
  permanent `pending` status (`scripts/preflight_readiness.py:80-84`), and the
  rubric marks `G4` as a hybrid gate requiring instructor confirmation
  (`docs/ASSESSMENT_RUBRIC.md:33,50-61`).

---

## 4. Impact · الأثر

| Gate / item · البوابة / العنصر | Status · الحالة |
|---|---|
| `G1` C9_DAY1_GATE | ✅ PASS |
| `G2` C20_DAY2_GATE | ✅ PASS |
| `G3` FINAL_EXPORT_CREATED + valid manifest | ✅ PASS |
| `G4` green workflow + instructor confirmation | ⚠️ blocked only by the contradiction above |
| `G5` synthetic / sanitized content | ✅ (pending instructor privacy review) |
| `G6` zero cross-customer leak | ✅ PASS |
| `G7` no unauthorized / repeated write | ✅ PASS |

`validate_submission.py` (the guardrail-producing validator that writes the
cryptographic receipt) passes; it is the more authoritative of the two steps.

---

## 5. Recommended resolution · الحل المقترح

1. Keep the executed notebook as-is (correct: it carries the `FINAL_EXPORT_CREATED`
   evidence required by `G3`).
2. Treat the red workflow as an **rc3 material defect** to be corrected by the
   course author (e.g. scope `test_preflight_readiness` so its notebook-output
   assertion runs against a cleared template, not the learner's executed copy).
3. Settle `G4` manually per the rubric's hybrid-gate rule.

No test, script, or workflow file was modified to produce this state; modifying
course material to force a green check would be test tampering.

لم يُعدَّل أي اختبار أو سكربت أو ملف Workflow لإنتاج هذه الحالة؛ وتعديل مواد
الدورة لإجبار الفحص على الأخضر يُعدّ تلاعبًا بالاختبارات.
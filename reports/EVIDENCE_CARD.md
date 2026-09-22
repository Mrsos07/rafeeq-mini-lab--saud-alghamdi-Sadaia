# Required evidence cards · بطاقات الأدلة الإلزامية

Completed from `reports/templates/EVIDENCE_CARD_TEMPLATE.md` after extracting the clean C29 package. One concise card per day, using only the public learner ID, synthetic case IDs, actual cell/gate results, and the assessment `run_id`.

أُكمل من القالب بعد فك حزمة C29 النظيفة. بطاقة مختصرة لكل يوم، باستخدام معرف المتدرب العام فقط ومعرفات الحالات المصطنعة ونتائج الخلايا/البوابات الفعلية و`run_id` للتقييم.

## Submission identity · هوية التسليم

| Field | Entry · الإدخال |
|---|---|
| Public learner ID · معرف المتدرب العام | `Mrsos07` |
| Assessment `run_id` · معرف تشغيل التقييم | `run-68643c9545a84b47` |
| Final clean run date (UTC) · تاريخ التشغيل النظيف | `2026-09-21` |

## EV-D1 · Day 1 core and tools · نواة اليوم الأول وأدواته

| Field | Entry · الإدخال |
|---|---|
| Testable claim · الادعاء القابل للاختبار | The agent runs with typed state, bounded 6/12/2/1 flow, a four-stage ReAct cycle, narrow tool schemas, and a real local MCP `stdio` connection. |
| Cell/gate · الخلية/البوابة | `C1–C9` / `C9_DAY1_GATE` |
| Public case or metric · الحالة العامة أو المقياس | Gate `all_passed=true`; public tests `test_state_contract`, `test_tool_scope`, `test_mcp_smoke` = 3/3 |
| Expected result · النتيجة المتوقعة | `all_passed = true`, learner checks 1–5 complete |
| Actual result · النتيجة الفعلية | `all_passed = true`, `learner_checks: {1..5: true}`, 3/3 test files OK |
| Status · الحالة | `PASS` |
| Public artifact path · مسار الدليل العام | `reports/assessment_results.json` (`learning_gates.day1_gate=true`) |
| Reproduce · إعادة التنفيذ | 1. Run `C0_ENV_DOCTOR` → `C0 = READY`. 2. Run C1–C9 in order. 3. Run `C9_DAY1_GATE`. |

Safe observation · الملاحظة الآمنة: `route/outcome` deterministic; MCP `stdio` boundary connected; no model-controlled tool argument expands authority.

## EV-D2 · Day 2 memory and orchestration · ذاكرة اليوم الثاني وتنسيقه

| Field | Entry · الإدخال |
|---|---|
| Testable claim · الادعاء القابل للاختبار | Memory recall is owner/activity/expiry filtered before ranking, policy retrieval returns only the active version, and two specialists route through typed handoffs with human approval above SAR 500. |
| Cell/gate · الخلية/البوابة | `C10–C20` / `C20_DAY2_GATE` |
| Public case or metric · الحالة العامة أو المقياس | Session recall `TW-26003`; scoped recall `MEM-004` only; active policy `REF-03`; tests `test_memory_scope`, `test_routing`, `test_refund_gate`, `test_reflection_bound` = 4/4 |
| Expected result · النتيجة المتوقعة | `all_passed = true`, learner checks 6–10 complete |
| Actual result · النتيجة الفعلية | `all_passed = true`, `learner_checks: {6..10: true}`, 4/4 test files OK |
| Status · الحالة | `PASS` |
| Public artifact path · مسار الدليل العام | `reports/assessment_results.json` (`learning_gates.day2_gate=true`) |
| Reproduce · إعادة التنفيذ | 1. Restore context (`C10 = READY`). 2. Run C11–C19. 3. Run `C20_DAY2_GATE`. |

Safe observation · الملاحظة الآمنة: scoped recall returns `MEM-004` only; `raw_messages_stored=false` (references only, no raw customer content retained).

## EV-D3 · Day 3 security and evidence · أمن اليوم الثالث وأدلته

| Field | Entry · الإدخال |
|---|---|
| Testable claim · الادعاء القابل للاختبار | A new synthetic threat is blocked by a repaired local guard with no safe-input regression, reflection stays bounded, the trace is redacted, and one measured optimization is guarded — ending in a safe export. |
| Cell/gate · الخلية/البوابة | `C21–C29` / `C29_EXPORT_SAFETY_CHECK` |
| Public case or metric · الحالة العامة أو المقياس | Attack `L-SEC-09` blocked (`credential_token`); security retest 8/8; functional 8/8; `unauthorized_writes=0`; 214 redacted trace events; optimization `current_policy_cache` 1.666→0.223 ms (499/500 hits) |
| Expected result · النتيجة المتوقعة | `ready=true`, `FINAL_EXPORT_CREATED`, learner checks 11–14 complete |
| Actual result · النتيجة الفعلية | `ready=true`, 11/11 critical gates, `FINAL_EXPORT_CREATED` (`export-d00ab0a4431be69e`), `precheck 20/20`, 96 files |
| Status · الحالة | `PASS` |
| Public artifact path · مسار الدليل العام | `reports/assessment_results.json`, `reports/SECURITY_ASSESSMENT.md`, `reports/trace.jsonl`, `reports/submission_manifest.json` |
| Reproduce · إعادة التنفيذ | 1. Run C21–C28. 2. Complete TODO-14 and set `FINAL_EXPORT=True`. 3. Run `C29_EXPORT_SAFETY_CHECK`. |

Safe observation · الملاحظة الآمنة: weak baseline exposed → repaired guard passed; `customer_data_in_key=false`; residual risk documented in `SECURITY_ASSESSMENT.md`.

## Required redaction declaration · إقرار التنقيح الإلزامي

- [x] I used only the instructor-assigned `learner_id` or GitHub username. · استخدمت `learner_id` الذي تقدمه المدربة أو اسم مستخدم GitHub فقط.
- [x] All identifiers are supplied synthetic fixtures. · جميع المعرفات من الحالات المصطنعة المرفقة.
- [x] No password, token, API key, cookie, private link, or environment value appears. · لا توجد كلمة مرور أو رمز وصول أو مفتاح API أو Cookie أو رابط خاص أو قيمة بيئة.
- [x] No real person, customer, employee, order, payment, support, or trainee data appears. · لا توجد بيانات حقيقية لشخص أو عميل أو موظف أو طلب أو دفعة أو دعم أو متدرب.
- [x] No copied solution, instructor note, answer key, scoring rule, hidden test, or private chain-of-thought appears. · لا يوجد حل منسوخ أو ملاحظة مدربة أو مفتاح إجابة أو قاعدة درجات أو اختبار خفي أو تفكير داخلي خاص.
- [x] Every declared `PASS` matches an actual final-run result. · كل نتيجة `PASS` معلنة تطابق نتيجة فعلية من التشغيل النهائي.

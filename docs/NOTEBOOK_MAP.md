# Notebook map · خريطة الدفتر

> Maps the cumulative Colab notebook `Rafeeq_Mini_Capstone.ipynb` (30 named
> sections, C0–C29, with 14 learner TODOs). The notebook itself is the
> execution authority — graded cells must not be renamed, reordered, or
> deleted. Companion to [`../README.md`](../README.md).

## The single notebook · دفتر واحد

The learner release uses **one notebook**: `Rafeeq_Mini_Capstone.ipynb`. You build the same project in small, tested steps from `C0` to `C29` on the standard **Google Colab Free CPU** runtime with `LLM_MODE=stub` — no paid model, GPU, terminal, token, or API key.

> يبني المتدرب المشروع نفسه بخطوات صغيرة ومختبرة من `C0` إلى `C29` على بيئة كولاب المجانية مع الوضع `LLM_MODE=stub` دون أي مفتاح API أو خدمة مدفوعة.

## Cell map · خريطة الخلايا

### Day 1 — Core and tools · اليوم الأول — النواة والأدوات

| Section | Learning outcome · الناتج التعليمي |
|---|---|
| `C0_ENV_DOCTOR` | Verify runtime, files, versions, and safe defaults. · التحقق من البيئة والملفات والإعدادات الآمنة. |
| `C1_ARCHITECTURE` | Read the Rafeeq flow, components, boundaries, and safe operating assumptions. |
| `C2_TYPED_STATE` | Define the agent state and its required fields. |
| `C3_BOUNDED_GRAPH` | Build a graph with explicit routes, a step limit, and safe termination. |
| `C4_REASONING_TRACES` | Record observable decisions, tool calls, results, and counters — not private chain-of-thought. |
| `C5_REACT_ORDERS` | Apply bounded ReAct to synthetic order requests. |
| `C6_TOOL_SCHEMA` | Validate local-tool inputs and structured outputs. |
| `C7_MCP_SERVER` | Start the simulated local MCP server with an explicit allow-list. |
| `C8_MCP_CLIENT` | Connect the client and inspect the allowed MCP capabilities. |
| `C9_DAY1_GATE` | Prove the Day 1 flow and save the first checkpoint. |

### Day 2 — Memory and orchestration · اليوم الثاني — الذاكرة والتنسيق

| Section | Learning outcome · الناتج التعليمي |
|---|---|
| `C10_RESTORE` | Restore the supplied public files and verify the existing C0–C9 context and the Day 1 gate. After a runtime reset, rerun C0–C9 first; C10 does not restore Python memory. |
| `C11_SESSION_MEMORY` | Keep short-term context inside one session. |
| `C12_SCOPED_RECALL` | Retrieve long-term memory only inside the correct customer scope. |
| `C13_POLICY_RETRIEVAL` | Retrieve only the active, relevant policy chunk. |
| `C14_SPECIALISTS` | Define exactly two bounded agents — `OrdersAgent` and `RefundAgent`. |
| `C15_SUPERVISOR` | Route each validated task to the correct specialist. |
| `C16_TYPED_HANDOFF` | Apply typed delegation with validated task and result packets. |
| `C17_PLAN_EXECUTE` | Separate a short plan from controlled step-by-step execution. |
| `C18_REFUND_GATE` | Pause simulated refunds above SAR 500 for documented human approval. |
| `C19_INTERRUPT_RESUME` | Interrupt safely for approval, then resume without duplicating the action. |
| `C20_DAY2_GATE` | Prove memory isolation, routing, and approval behavior. |

### Day 3 — Security, quality, and submission · اليوم الثالث — الأمن والجودة والتسليم

| Section | Learning outcome · الناتج التعليمي |
|---|---|
| `C21_THREAT_MODEL` | Identify assets, trust boundaries, threats, and controls. |
| `C22_ATTACK_SUITE` | Run the supplied public injection, misuse, budget, and approval-bypass cases. |
| `C23_GUARD_FIX_RETEST` | Strengthen the guardrail and rerun the same public attack cases. |
| `C24_REFLECTION_GATE` | Apply one bounded reflection cycle only when the gate allows it. |
| `C25_TRACE_EVAL` | Evaluate sanitized traces and export `reports/trace.jsonl`. |
| `C26_ONE_OPTIMIZATION` | Apply one measured performance or cost optimization and compare before/after. |
| `C27_SCORECARD` | Produce assessment results and the monitoring dashboard. |
| `C28_READINESS` | Generate the two reports and complete final readiness checks. |
| `C29_EXPORT_SAFETY_CHECK` | Block unsafe files, verify required outputs, then create `reports/submission_manifest.json` and the submission ZIP when final export is enabled. |

## Observed runtime map · مواقع التنفيذ المؤكدة

Kernel cell indices observed in this executed copy (the notebook itself remains the authority):

- C0 doctor and the Day 1 gate (C9) both print `all_passed=true` with the Day 1 learner checks 1–5.
- Day 2 kernel cells run in order C10 → C20; the learner TODOs sit in C11 (TODO-6), C12 (TODO-7), C13 (TODO-8), C16 (TODO-9), and C18 (TODO-10).
- Day 2 and Day 3 gate cells write the evidence files listed in [`REPORTS_AND_EVIDENCE.md`](REPORTS_AND_EVIDENCE.md).

## Daily gates and checkpoint messages · البوابات والرسائل

| Gate | Continue only when · لا تنتقل إلا بعد | Checkpoint label · تسمية النقطة |
|---|---|---|
| `C9_DAY1_GATE` | Day 1 public checks and the bounded MCP capability list produce `all_passed=true`. | `docs(day1): record C9 gate` |
| `C20_DAY2_GATE` | Scoped recall, typed delegation, routing, refund-gate, and interrupt/resume tests produce `all_passed=true`. | `docs(day2): record C20 gate` |
| `C29_EXPORT_SAFETY_CHECK` | Both reports and all required artifacts exist, the safety scan passes, and the cell prints `FINAL_EXPORT_CREATED`. | `feat: submit Rafeeq Mini capstone` |

Save the notebook in Drive after every completed section. Git does **not** preserve Colab memory, installed packages, or unsaved outputs.

## Learner rules · قواعد المتدرب

- Change only learner `TODO` areas unless the instructor explicitly identifies another cell.
- Keep exactly one validator-counted `TODO-N` marker per exercise.
- Do not disable, rewrite, or bypass a public check.
- Do not add solutions, answer keys, or hidden evaluations.
- Keep all examples synthetic — refunds and tool actions are simulations only.
- Store short decision records, never hidden reasoning or chain-of-thought.
- If the runtime resets, follow the course `recovery/README.md`; you do not need to restart the whole project.
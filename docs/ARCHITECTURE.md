# Architecture · المعمارية

> Linked technical documentation for the **Rafeeq Mini · رفيق ميني** submission.
> Companion to [`../README.md`](../README.md). See also
> [`NOTEBOOK_MAP.md`](NOTEBOOK_MAP.md), [`RUNBOOK.md`](RUNBOOK.md), and
> [`REPORTS_AND_EVIDENCE.md`](REPORTS_AND_EVIDENCE.md).

## 1. Overview · نظرة عامة

Rafeeq Mini is a compact, end-to-end demonstration of **agentic AI systems engineering**. A customer request in Arabic or English is taken apart, understood, verified, and acted upon by an artificial agent — without a single external API call and with every decision left open to inspection.

It is a training simulation built on a **deterministic stub runtime**: fully offline, deterministic, and reproducible, so the entire system — reasoning, memory, tools, and gates — can be studied as pure engineering.

> Synthetic simulation only. It does not contact any real delivery, payment, or customer system.

## 2. Pipeline · مسار الوكيل

```text
Understand → Verify → Retrieve → Route → Approve → Act → Prove
فهم        ← تحقق    ← استرجاع  ← توجيه ← موافقة ← تنفيذ ← إثبات
```

1. **Understand / فهم** — detect the intent and extract the order number from an Arabic or English message.
2. **Verify / تحقق** — ownership is checked before anything is read or written; no cross-customer access is possible.
3. **Retrieve / استرجاع** — load the active policy and the order context from scoped memory.
4. **Route / توجيه** — hand the task to a specialist agent through typed handoffs.
5. **Approve / موافقة** — high-value actions (refunds above SAR 500) pause for documented human approval.
6. **Act / تنفيذ** — write the side effect once, idempotently, within the tool's narrow schema.
7. **Prove / إثبات** — trace the whole run to JSON evidence and a daily gate.

## 3. Components · المكونات الجوهرية

| Component · المكوّن | Responsibility · المسؤولية |
|---|---|
| **Typed learner state** | A bounded state object (route, status, and step counters) that the agent carries between cycles. |
| **Bounded graph** | Explicit routes with a hard step limit and safe termination. |
| **Bounded ReAct cycle** | Repeats _decision → action → observation_; stops when budget is exhausted or the goal is met. |
| **Reasoning traces** | Record observable decisions, tool calls, results, and counters — never private chain-of-thought. |
| **Local tools** | Narrow, schema-scoped tools (e.g. `get_delivery_eta`) with read-only guarantees and no unpermitted writes. |
| **Local MCP server** | A simulated `stdio` JSON-RPC server exposing tools through a standard protocol with an explicit allow-list. |
| **Scoped memory** | Session memory (C11), per-customer recall (C12), and active-policy retrieval (C13). |
| **Specialists** | Exactly two bounded agents — `OrdersAgent` (read only) and `RefundAgent` (write once, gated). |
| **Guardrails & approval** | Budget limits (steps / transitions / handoffs / reflections) and a human gate above SAR 500. |
| **Evidence layer** | JSONL traces and day-gate reports under `reports/` that make every run auditable. |

## 4. Budget limits · حدود التشغيل

The agent can never run away: each run is capped at **6 steps**, **12 state transitions**, **2 handoffs**, and **1 reflection** — exceeding any limit stops the cycle safely, not silently.

## 5. Data model & scope · نموذج البيانات والنطاق

- **Orders** — synthetic orders owned by customers; reads are ownership-checked before any retrieval or write.
- **Policies** — active policy chunks retrieved only when relevant (`locale`, `category`).
- **Memory** — short-term session memory; long-term recall isolated per customer scope.
- **Refunds** — simulated. Require a delivery delay greater than two days; amounts above SAR 500 pause for explicit human approval; re-running a write stays safe through deterministic idempotency.

## 6. Safety invariants · ضوابط السلامة

- **No leakage** — reads are ownership-checked before any retrieval or write.
- **No stray writes** — writes are single, idempotent, and gated.
- **No secrets** — all data is synthetic; identifiers are learner IDs only.
- **Redacted traces** — raw messages, reasoning, and identifiers never appear in logs.

## 7. Three-day cumulative build · بناء تراكمي على ثلاثة أيام

| Day · اليوم | Focus · المحور | Sections · الأقسام | Gate · البوابة |
|---|---|---|---|
| 1 | Core & bounded single agent · النواة ووكيل محدود | C0–C9 | `C9_DAY1_GATE` |
| 2 | Memory & specialist orchestration · الذاكرة والتنسيق | C10–C20 | `C20_DAY2_GATE` |
| 3 | Security, proving & export · التأمين والإثبات | C21–C29 | `C29_EXPORT_SAFETY_CHECK` |

## 8. Metrics · المقاييس

- **Budget per run:** 6 steps · 12 state transitions · 2 handoffs · 1 reflection.
- **Day gates:** public contract tests plus the learner TODOs (1–14) must all pass.
- **Security retest:** the public injection, misuse, budget-exhaustion, and approval-bypass cases must all pass after the guard fix (C23).
- **Final export:** readiness checks pass, the safety scan blocks unsafe files, `reports/submission_manifest.json` is written, and the cell prints `FINAL_EXPORT_CREATED`.

## 9. Limitations · القيود

- Supervised engineering simulation — not a production deployment.
- Deterministic stub runtime — no real LLM, network, or API keys.
- Privacy boundary — real names, emails, IDs, and credentials must never appear in the repository; use `learner_id` or the GitHub username.
- Colab memory is not preserved by Git or by checkpoint files — after a runtime reset, re-run C0–C9 first (`C10` does not restore Python memory).
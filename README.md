# Rafeeq Mini · رفيق ميني

_A bilingual agentic operations assistant for a fictional delivery company._

_مساعد عمليات وكيلي ثنائي اللغة لشركة توصيل افتراضية._

![Colab Free CPU](https://img.shields.io/badge/Colab-Free%20CPU-blue)
![Deterministic stub](https://img.shields.io/badge/Mode-Deterministic%20stub-lightgrey)
![API-free](https://img.shields.io/badge/API-Key%20free-success)
![AR + EN](https://img.shields.io/badge/Lang-AR%20%2B%20EN-orange)

---

## The project · المشروع

Rafeeq Mini is a compact, end-to-end demonstration of **agentic AI systems engineering**. It shows how a customer request in Arabic or English can be taken apart, understood, verified, and acted upon by an artificial agent — without a single external API call and with every decision left open to inspection.

It is a training simulation built on a **deterministic stub runtime**: fully offline, deterministic, and reproducible, so the entire system — reasoning, memory, tools, and gates — can be studied as pure engineering.

> This is a synthetic simulation. It does not contact any real delivery, payment, or customer system.

---

## The architecture · المعمارية

The system is a **bounded agent pipeline**: the request flows through typed stages, and at every stage the agent is constrained by explicit state, hard budget limits, and an auditable record.

### Agent flow · مسار الوكيل

```text
Understand → Verify → Retrieve → Route → Approve → Act → Prove
فهم        ← تحقق    ← استرجاع  ← توجيه ← موافقة ← تنفيذ ← إثبات
```

1. **Understand / فهم** — the agent detects the intent and extracts the order number from an Arabic or English message.
2. **Verify / تحقق** — ownership is checked before anything is read or written; no cross-customer access is possible.
3. **Retrieve / استرجاع** — the active policy and the order context are loaded from scoped memory.
4. **Route / توجيه** — the task is handed to a specialist agent through typed handoffs.
5. **Approve / موافقة** — high-value actions (refunds above the threshold) pause for documented human approval.
6. **Act / تنفيذ** — the side effect is written once, idempotently, within the tool's narrow schema.
7. **Prove / إثبات** — the whole run is traced to JSON evidence and a daily gate.

### Core components · المكونات الجوهرية

| Component · المكوّن | Responsibility · المسؤولية |
|---|---|
| **Typed learner state** | A bounded state object (route, status, and step counters) that the agent carries between cycles. |
| **Bounded ReAct cycle** | Repeats _decision → action → observation_ and stops when budget is exhausted or the goal is met. |
| **Local tools** | Narrow, schema-scoped tools (e.g. `get_delivery_eta`) with read-only guarantees and no unpermitted writes. |
| **Local MCP server** | A stdio JSON-RPC server exposing the tools through a standard protocol. |
| **Scoped memory** | Session memory, per-customer recall, and active-policy retrieval. |
| **Guardrails & approval** | Budget limits (steps / transitions / handoffs / reflections), attack tests, and human gate above SAR 500. |
| **Evidence layer** | JSONL traces and day-gate reports (`reports/checkpoints/`) that make every run auditable. |

### Budget limits · حدود التشغيل

The agent can never run away: each run is capped at **6 steps**, **12 state transitions**, **2 handoffs**, and **1 reflection** — exceeding any limit stops the cycle safely, not silently.

### Safety invariants · ضوابط السلامة

- **No leakage** — reads are ownership-checked before any retrieval or write.
- **No stray writes** — writes are single, idempotent, and gated.
- **No secrets** — all data is synthetic; identifiers are learner IDs only.
- **Redacted traces** — raw messages, reasoning, and identifiers never appear in logs.

---

## The outcome · النتائج

The project is delivered as a **three-day cumulative lab**: each day builds one layer of the system and closes with a **gate** that verifies the work and writes machine-readable evidence.

| Day · اليوم | Focus · المحور | Gate · البوابة |
|---|---|---|
| 1 | Core & bounded single agent · النواة ووكيل محدود | `C9_DAY1_GATE` |
| 2 | Memory & specialist orchestration · الذاكرة والتنسيق | `C20_DAY2_GATE` |
| 3 | Security, proving & export · التأمين والإثبات | `C29_EXPORT_SAFETY_CHECK` |

---

## Repository · المستودع

```text
rafeeq-mini-<yourname>/
├── README.md                 ← this file
├── LEARNING_PROGRESS.md      ← progress log (updated at C9, C20, C29)
└── notebooks/
    └── Rafeeq_Mini_Capstone.ipynb   ← the executed notebook (final export)
```

---

**Course · الدورة:** *Advanced Agentic AI Systems Engineering* — [SDAIA Academy](https://github.com/SDAIAAcademy) · Instructor: Meaad Al-Marri · ميعاد المري

**License · الترخيص:** © SDAIA Academy — learner submission. No real customer data is used. · تسليم متدرب؛ لا تُستخدم أي بيانات عملاء حقيقية.
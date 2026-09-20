# rafeeq-mini-lab--saud-alghamdi-sadaia
# Rafeeq Mini · رفيق ميني

> A bilingual, safe and auditable **agentic operations assistant** for a fictional delivery company — built over three days as a cumulative, zero-cost, offline Colab lab.

> مساعد عمليات **وكيلي** ثنائي اللغة، آمن وقابل للتدقيق، لشركة توصيل افتراضية — بُني خلال ثلاثة أيام في دفتر Colab تراكمي، دون أي مفتاح API.

![Colab Free CPU](https://img.shields.io/badge/Colab-Free%20CPU-blue)
![Offline stub](https://img.shields.io/badge/Mode-Deterministic%20stub-lightgrey)
![No API key](https://img.shields.io/badge/API-Key%20free-success)
![AR + EN](https://img.shields.io/badge/Lang-AR%20%2B%20EN-orange)
![14 guided exercises](https://img.shields.io/badge/Exercises-14-yellow)

---

## Table of Contents · الفهرس

| EN | AR |
|---|---|
| [About the project](#about-the-project) | [عن المشروع](#عن-المشروع) |
| [How it works](#how-it-works) | [كيف يعمل](#كيف-يعمل) |
| [Getting started](#getting-started) | [البدء](#البدء) |
| [Repository structure](#repository-structure) | [بنية المستودع](#بنية-المستودع) |
| [Safety & auditability](#safety--auditability) | [السلامة وقابلية التدقيق](#السلامة-وقابلية-التدقيق) |
| [Progress](#progress) | [سجل التقدم](#سجل-التقدم) |
| [Technical stack](#technical-stack) | [التقنيات](#التقنيات) |
| [References](#references) | [المراجع](#المراجع) |

---

## About the project · عن المشروع

**Rafeeq Mini** is a miniature end-to-end demonstration of **advanced agentic AI systems engineering**. Following the *Rafeeq Mini Labs* course, it turns a customer request into a safe, human-overviewable action through a bounded agent flow:

**رفيق ميني** هو عرض مصغّر متكامل لهندسة أنظمة الذكاء الاصطناعي التوكيلي المتقدمة. عبر دورة *لابات رفيق المصغّر*، يحوّل طلب العميل إلى إجراء آمن وتحت إشراف بشري عبر تدفق وكيلي محدود:

```
Understand → Verify → Retrieve → Route → Approve → Act → Prove
فهم ← تحقق ← استرجاع ← توجيه ← موافقة ← تنفيذ ← إثبات
```

The agent detects **intent and order number** from an Arabic or English request, **verifies ownership**, **retrieves the active policy**, **routes** the task to a specialist agent, and **pauses for documented human approval** on any refund above **SAR 500**.

يحدد الوكيل **الهدف ورقم الطلب** من رسالة عربية أو إنجليزية، ويتحقق من **الملكية**، ويسترجع **السياسة السارية**، ويوجّه المهمة إلى **وكيل متخصص**، ويوقف أي استرداد يتجاوز **500 ريال** لموافقة بشرية موثقة.

> **Safety boundary · حد الأمان:** This is a synthetic training simulation. No real customers, orders, payments, or systems are contacted. · هذه محاكاة تدريبية ببيانات اصطناعية؛ لا توجد بيانات أو معاملات أو أنظمة حقيقية.

---

## How it works · كيف يعمل

The notebook grows with the learner across three days — every day ends at a clear gate with inspectable evidence:

ينمو الدفتر مع المتدرب عبر ثلاثة أيام — ينتهي كل يوم ببوابة واضحة وأدلة قابلة للفحص:

| Day · اليوم | Sections · الأقسام | Focus · المحور | Gate · البوابة |
|---|---|---|---|
| **1 — Core & Tools** · النواة والأدوات | C0–C9 | Typed state, bounded ReAct, decision traces, local tools, MCP | `C9_DAY1_GATE` |
| **2 — Memory & Orchestration** · الذاكرة والتنسيق | C10–C20 | Scoped memory, policy retrieval, specialist agents, typed handoffs, human approval | `C20_DAY2_GATE` |
| **3 — Secure & Prove** · التأمين والإثبات | C21–C29 | Threat tests, guardrails, bounded reflection, evals, tracing, safe export | `C29_EXPORT_SAFETY_CHECK` |

### Design pillars · أركان التصميم

- **Bounded agent flow · تدفق وكيلي محدود** — typed state, explicit routing, step limits (6/12/2/1), safe termination.
- **Tools & MCP · الأدوات وMCP** — narrow tool schemas and a local stdio MCP server.
- **Multi-agent coordination · تنسيق متعدد الوكلاء** — supervisor + specialist agents with typed handoffs.
- **Safety & approval · السلامة والموافقة** — guardrails, attack tests, idempotent writes, approval above SAR 500.
- **Scoped memory · ذاكرة معزولة** — session memory, per-customer recall, active-policy retrieval.
- **Evidence & monitoring · الأدلة والمراقبة** — traces, evaluations, metrics, dashboard, and a submission manifest.

---

## Getting started · البدء

**Requirements · المتطلبات:** a Google account (for Colab) and a GitHub account. No Git install, no API key, no GPU.

متطلبات: حساب Google (لكولاب) وحساب GitHub. لا يلزم تثبيت Git أو مفتاح API أو GPU.

1. **Open the notebook · افتح الدفتر:** open the cumulative notebook below and save a copy in your Drive (`File → Save a copy in Drive`). Never edit the canonical copy.  
   افتح الدفتر التراكمي أدناه واحفظ نسخة في Drive؛ لا تعدّل النسخة الأصلية.

   📓 **`Rafeeq_Mini_Capstone.ipynb`** → [Run on Google Colab ↗](https://colab.research.google.com/github/almiyead-rgb/rafeeq-agentic-ai-labs/blob/v0.9.0-rc3/notebooks/Rafeeq_Mini_Capstone.ipynb)

2. **Run the environment doctor · شغّل فاحص البيئة:** run the `C0_ENV_DOCTOR` cell and continue only on:  
   شغّل خلية `C0_ENV_DOCTOR` ولا تتابع إلا عند:

   ```text
   ✓ python_runtime
   ✓ cpu_mode
   ✓ stub_mode
   C0 = READY
   all_passed=true
   ```

3. **Work the daily gates · اعمل عبر بوابات الأيام** in order (C1→C9, C10→C20, C21→C29). Do **not** skip ahead while a gate is blocked.  
   بالترتيب، ولا تنتقل للأمام عند تعثر أي بوابة.

---

## Repository structure · بنية المستودع

Structure reflects the course's **safe submission policy**: public files carry **only** the learner ID, and code/evidence stay out of the repository until the clean `C29` export.

تعكس البنية **سياسة التسليم الآمنة** للدورة: الملفات العامة تحمل معرف المتدرب فقط، ويبقى الكود والأدلة خارج المستودع حتى التصدير النظيف في C29.

```text
rafeeq-mini-<yourname>/
├── README.md                 ← this file
├── LEARNING_PROGRESS.md      ← progress log (updated at C9, C20, C29)
└── notebooks/
    └── Rafeeq_Mini_Capstone.ipynb   ← the executed notebook (final export)
```

> The full generated package (`src/`, `reports/`, tests, scripts, manifest) is produced by the notebook itself at `C29_EXPORT_SAFETY_CHECK` and uploaded as the extracted contents next to `LEARNING_PROGRESS.md`.

---

## Safety & auditability · السلامة وقابلية التدقيق

Key guarantees demonstrated in the lab (and enforced by its evidence contracts):

- **No cross-customer leakage · لا تسريب بين العملاء** — reads are ownership-checked before any retrieval or write.
- **No unauthorized writes · لا كتابة غير مصرح بها** — refund writes are single, idempotent, and gated.
- **Human approval above SAR 500 · موافقة بشرية فوق 500 ريال** — `requires_human_approval` + `high_value` risk flag.
- **No secrets or real data · لا أسرار ولا بيانات حقيقية** — all data is synthetic and public-facing names are learner IDs only.
- **Redacted traces · تتبعات منقّحة** — raw messages, reasoning, and identifiers never appear in logs.

---

## Progress · سجل التقدم

Course progress is tracked in [`LEARNING_PROGRESS.md`](LEARNING_PROGRESS.md) at every day gate.

يُوثَّق تقدم الدورة في [`LEARNING_PROGRESS.md`](LEARNING_PROGRESS.md) عند كل بوابة يوم.

| Day · اليوم | Status · الحالة |
|---|---|
| Day 1 · اليوم الأول | ⬜ PENDING / قيد التنفيذ |
| Day 2 · اليوم الثاني | ⬜ PENDING / قيد التنفيذ |
| Day 3 · اليوم الثالث | ⬜ PENDING / قيد التنفيذ |

---

## Technical stack · التقنيات

| Layer · الطبقة | Approach · النهج |
|---|---|
| Model · النموذج | Deterministic stub · محاكاة حتمية (no API key) |
| Retrieval · الاسترجاع | Local TF-IDF & cosine similarity · محلي |
| MCP · إم-سي-بي | Local stdio server (JSON-RPC) · محلي عبر stdio |
| Tracing · التتبع | Local JSONL traces & simulated metrics · محلي |
| Delivery · التسليم | Auditable GitHub package · حزمة GitHub قابلة للتدقيق |

---

## References · المراجع

- **Repository authority · مرجع الدورة:** [almiyead-rgb/rafeeq-agentic-ai-labs](https://github.com/almiyead-rgb/rafeeq-agentic-ai-labs)
- **SDAIA Academy · أكاديمية سدايا:** [github.com/SDAIAAcademy](https://github.com/SDAIAAcademy)
- **Training program · البرنامج التدريبي:** Advanced Agentic AI Systems Engineering · هندسة أنظمة الذكاء الاصطناعي التوكيلي المتقدمة
- **Instructor · المدربة:** Meaad Al-Marri · ميعاد المري

---

## License · الترخيص

© SDAIA Academy — this repository is a learner submission for the *Rafeeq Mini Labs* course. All rights belong to their respective owners. No real customer data is used.

© أكاديمية سدايا — هذا المستودع تسليم متدرب لدورة *لابات رفيق المصغّر*. جميع الحقوق ملك أصحابها. لا تُستخدم أي بيانات عملاء حقيقية.

# Rafeeq Mini Project Report | تقرير مشروع رفيق ميني

- Training program | البرنامج التدريبي: Advanced Agentic AI Systems Engineering · هندسة أنظمة الذكاء الاصطناعي التوكيلي المتقدمة
- SDAIA Academy GitHub external reference | مرجع أكاديمية سدايا على GitHub: https://github.com/SDAIAAcademy

## Run and outcome | التشغيل والنتيجة
- Assessment run ID | معرّف تشغيل التقييم: `run-68643c9545a84b47`
- Generated UTC | وقت الإنشاء: 2026-09-21T08:47:45.604497+00:00
- Decision | القرار: READY
- Evidence cells | خلايا الأدلة: C9, C20, C23, C26, C27, C28

## Gates | البوابات
| Gate | Passed |
|---|---:|
| Day 1 gate | True |
            | Day 2 gate | True |
            | Security + learner regression gate | True |
            | Readiness gate | True |

## Public evidence and metrics | الأدلة والمقاييس العامة
- Functional case IDs | معرّفات الحالات الوظيفية: EVAL-AR-01, EVAL-AR-02, EVAL-AR-03, EVAL-AR-04, EVAL-EN-01, EVAL-EN-02, EVAL-EN-03, EVAL-EN-04
- Security case IDs | معرّفات الحالات الأمنية: SEC-01, SEC-02, SEC-03, SEC-04, SEC-05, SEC-06, SEC-07, SEC-08
- Functional accuracy | الدقة الوظيفية: 100%
- Security pass rate | نسبة اجتياز الأمن: 100%
- Median latency | وسيط الزمن: 1.738 ms
- Trace records | سجلات التتبع: 214
- Trace parent integrity | سلامة روابط التتبع: True
- Runtime | بيئة التشغيل: offline deterministic stub on free CPU

## Architecture | المعمارية
Thin supervisor, OrdersAgent, RefundAgent, scoped memory, current-policy retrieval, MCP stdio tools, human approval gate and redacted traces.

منسق خفيف، وكيلا الطلبات والاسترداد، ذاكرة محددة النطاق، استرجاع السياسة السارية، أدوات MCP عبر stdio، بوابة موافقة بشرية، وتتبعات منقحة.

## Learner security evidence | دليل أمن المتدرب
- New threat case metadata | بيانات الحالة الجديدة: `{"asset": "Tool output | مخرجات الأداة", "case_id": "L-SEC-09", "control": "Output guard | حاجز إخراج يفحص المخرجات عن أشكال بيانات الاعتماد ويمنعها", "expected_flag": "credential_token", "payload_length": 107}`
- Weak local baseline exposed | كشف خط الأساس الضعيف: True
- Repaired guard regression passed | نجاح اختبار الحاجز المُصلح: True

## Optimization evidence | دليل التحسين
- Optimization | التحسين: current_policy_cache
- Before | قبل: 1.666 ms / 500 iterations
- After | بعد: 0.223 ms / 500 iterations
- Cache hits / misses | إصابات / إخفاقات التخزين: 499 / 1
- Learner trade-off and guardrail | مقايضة وضابط المتدرب: Before: 500 uncached policy lookups took 1.666 ms after: cached path dropped to 0.223 ms with 499 cache hits. Guardrail: the shared key holds locale, category and active policy version only; customer data is excluded from every cache key.

## Engineering decisions | القرارات الهندسية

### 1. Authority and trust boundaries · حدود الصلاحية والثقة
The model proposes a route and a tool call; the runtime validates both against a typed contract; the MCP server re-verifies ownership, eligibility, approval and idempotency at execution time. `customer_id` is attached by the trusted runtime and never travels through the model; money amounts, permission flags and de-duplication keys are never model-controlled arguments. A rule placed in a prompt is not authorization — every write is checked again in the server, even if the prompt and the supervisor were bypassed.

يقترح النموذج المسار واستدعاء الأداة، ويتحقق وقت التشغيل من كليهما مقابل عقد مُنمّط، ويعيد خادم MCP التحقق من الملكية والأهلية والموافقة ومنع التكرار لحظة التنفيذ. يُضاف `customer_id` من وقت التشغيل الموثوق ولا يمر عبر النموذج؛ ولا تكون المبالغ وأعلام الصلاحية ومفاتيح منع التكرار وسائط يتحكم بها النموذج. القاعدة داخل النص ليست تفويضًا — تُعاد مراجعة كل كتابة في الخادم حتى لو تجاوز الهجوم النص والمنسق.

### 2. Reasoning pattern choices · اختيار أنماط الاستدلال
- **ReAct** (orders path): a short read-only task with a known budget — the model picks the next permitted step inside the allowlist. Trade-off: flexible where needed, but only because permissions are narrow and read-only.
- **Plan-and-Execute** (refund path): a multi-step flow (order facts → active policy → eligibility → approval → respond) where the plan is inspected before any action; re-planning happens only on a transient read failure or a changed fact. Trade-off: more structure and testability at the cost of less run-time flexibility.
- **Bounded Self-Critique** (high-impact replies): one review against stated criteria, at most one revision, no new permissions and no tool calls. Trade-off: catches obvious defects without granting the critic authority.

- **ReAct** (مسار الطلبات): مهمة قراءة قصيرة بميزانية معلومة — يختار النموذج الخطوة المسموح بها داخل قائمة السماح. المقايضة: مرونة عند الحاجة لأن الصلاحيات ضيقة وللقراءة فقط.
- **Plan-and-Execute** (مسار الاسترداد): تدفق متعدد الخطوات (بيانات الطلب ← السياسة السارية ← الأهلية ← الموافقة ← الرد) تُفحص فيه الخطة قبل أي تنفيذ؛ ولا يُعاد التخطيط إلا عند فشل قراءة عابر أو تغيّر حقيقة. المقايضة: هيكلة وقابلية اختبار أعلى مقابل مرونة تشغيلية أقل.
- **النقد الذاتي المحدود** (الردود عالية الأثر): مراجعة واحدة مقابل معايير معلنة وبحد أقصى مراجعة واحدة وبلا صلاحيات جديدة وبلا استدعاء أدوات. المقايضة: يكشف عيوبًا واضحة دون منح الناقد سلطة.

### 3. Lab simulation versus production · المحاكاة مقابل الإنتاج
Proven here: Colab Free CPU, `LLM_MODE=stub`, local `stdio` MCP, synthetic data, sound environment/files/versions, passing graph + MCP smoke test, `health_check()` and `run_demo.py` pass, reports written to approved paths. Not proven here (production blueprint): real identity and authorization, durable storage, secret management, central observability, CI/CD, and load/recovery testing.

مُثبت هنا: Colab مجاني، نمط `stub`، MCP محلي عبر stdio، بيانات اصطناعية، سلامة البيئة والملفات والإصدارات، نجاح بناء الرسم وفحص MCP، ونجاح `health_check()` و`run_demo.py`. غير مُثبت هنا (خطة الإنتاج): هوية وتفويض حقيقيان، تخزين دائم، إدارة أسرار، مراقبة مركزية، CI/CD، واختبارات الحمل والاستعادة.

### 4. One decision traced end-to-end · قرار واحد مُتتبَّع من طرف إلى طرف
Decision: "refunds above SAR 500 pause for human approval, enforced in flow code and the server — never in the prompt."

| Stage | Value |
|---|---|
| Cell | C18 `REFUND_GATE` / TODO-10 |
| Case | TW-26017 (740 SAR) and SEC-02 `approval_bypass` |
| Metric | `human_approval_above_500 = true` · `unauthorized_write = 0` |
| Result | `requires_human_approval` → on approval `created`, written exactly once (idempotency key) |

القرار: «الاسترداد فوق 500 ريال يتوقف للموافقة البشرية، ويُفرض في كود التدفق والخادم — لا في النص.»

| المرحلة | القيمة |
|---|---|
| الخلية | C18 `REFUND_GATE` / TODO-10 |
| الحالة | TW-26017 (740 ريال) و SEC-02 `approval_bypass` |
| المقياس | `human_approval_above_500 = true` · `unauthorized_write = 0` |
| النتيجة | `requires_human_approval` ← عند الموافقة `created` بكتابة واحدة (مفتاح منع التكرار) |

## Residual risks and limitations | المخاطر المتبقية والقيود
Synthetic public data only; no real delivery, payment or customer system; production identity, policy, secrets and operations are out of scope.

بيانات عامة اصطناعية فقط؛ لا اتصال بأنظمة توصيل أو دفع أو عملاء حقيقية؛ والهوية والسياسات والأسرار وعمليات الإنتاج خارج النطاق.

The deterministic stub does not measure live-model quality, rate limits or provider cost. Local approval and memory stores are training simulations, not durable production controls.

لا يقيس النمط الحتمي جودة نموذج حي أو حدود المعدل أو تكلفة المزود، كما أن مخازن الموافقة والذاكرة المحلية محاكاة تدريبية وليست ضوابط إنتاج دائمة.

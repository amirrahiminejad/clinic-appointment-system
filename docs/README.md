# ساختار مستندات

این سند ساختار و اصول نگهداری مستندات پروژه، نیازمندی‌ها، مستندات معماری، تصمیمات معماری، استانداردها، پرامپت‌های هوش مصنوعی و سایر artefactهای مرتبط را در قالب **Docs as Code** تعریف می‌کند.

اصل اصلی این ساختار:

> **هر چیزی که کد نیست، زیرمجموعه `docs/` قرار می‌گیرد.**

به این ترتیب مستندات در کنار Source Code و تحت Version Control نگهداری می‌شوند، در حالی که مرز مشخصی بین مستندات و پیاده‌سازی وجود دارد.

---

## ساختار Repository

```text
repo/
├── README.md
│
├── docs/
│   ├── requirements/
│   │   ├── srs/
│   │   │   └── srs.md
│   │   │
│   │   └── interviews/
│   │       ├── pm/
│   │       │   ├── 2026-09-01-product-requirements.md
│   │       │   └── 2026-09-02-pricing.md
│   │       │
│   │       └── stakeholders/
│   │           ├── 2026-09-03-payment-team.md
│   │           └── 2026-09-04-security-team.md
│   │
│   ├── architecture/
│   │   ├── overview.md
│   │   ├── system-context.md
│   │   ├── components.md
│   │   └── deployment.md
│   │
│   ├── decisions/
│   │   ├── ADR-001-xxx.md
│   │   ├── ADR-002-xxx.md
│   │   └── ...
│   │
│   ├── standards/
│   │   ├── architecture.md
│   │   ├── api.md
│   │   └── security.md
│   │
│   ├── references/
│   │   ├── technologies/
│   │   └── business/
│   │
│   ├── ai/
│   │   ├── prompts/
│   │   │   ├── architecture-review.md
│   │   │   ├── system-design.md
│   │   │   ├── requirements-analysis.md
│   │   │   └── adr-generation.md
│   │   │
│   │   └── context/
│   │       └── project-context.md
│   │
│   └── templates/
│       ├── srs.md
│       ├── interview.md
│       ├── adr.md
│       ├── architecture.md
│       └── prompt.md
│
└── src/
    └── ...
```

---

## مسئولیت پوشه‌ها

### `docs/requirements/`

این بخش شامل نیازمندی‌های کسب‌وکار و فنی سیستم است که مبنای طراحی معماری قرار می‌گیرند.

### `docs/requirements/srs/`

شامل **Software Requirements Specification (SRS)** و مستندات رسمی نیازمندی‌های سیستم است.

```text
docs/requirements/srs/
└── srs.md
```

SRS باید نماینده‌ی نیازمندی‌های **تجمیع‌شده، تحلیل‌شده و تأییدشده** سیستم باشد.

---

### `docs/requirements/interviews/`

این بخش شامل خروجی جلسات و مصاحبه‌های معمار با افراد مختلف مانند:

* Product Manager
* Business Owner
* Stakeholder
* Domain Expert
* تیم‌های فنی
* تیم‌های امنیت، زیرساخت و عملیات

است.

```text
docs/requirements/interviews/
├── pm/
└── stakeholders/
```

برای مثال:

```text
docs/requirements/interviews/
├── pm/
│   ├── 2026-09-01-product-requirements.md
│   └── 2026-09-02-pricing.md
│
└── stakeholders/
    ├── 2026-09-03-payment-team.md
    └── 2026-09-04-security-team.md
```

مصاحبه‌ها باید تا حد امکان **منبع اولیه اطلاعات** را حفظ کنند و صرفاً خلاصه‌ی نهایی نیازمندی نباشند.

جریان معمول به شکل زیر است:

```text
مصاحبه با Stakeholder
        ↓
تحلیل نیازمندی
        ↓
SRS
        ↓
معماری
```

---

## `docs/architecture/`

شامل مستنداتی است که معماری فعلی سیستم را توصیف می‌کنند.

نمونه مستندات:

```text
docs/architecture/
├── overview.md
├── system-context.md
├── components.md
└── deployment.md
```

بسته به اندازه و پیچیدگی سیستم می‌توان مستندات دیگری نیز اضافه کرد، مانند:

* Integration Architecture
* Data Architecture
* Security Architecture
* API Architecture
* Runtime Architecture
* Infrastructure Architecture

مستندات این بخش پاسخ می‌دهند به سؤال:

> **سیستم چگونه طراحی شده و چگونه کار می‌کند؟**

---

## `docs/decisions/`

این بخش شامل **Architecture Decision Record (ADR)** است.

ADR برای ثبت تصمیمات مهم معماری استفاده می‌شود.

هر ADR بهتر است حداقل موارد زیر را مشخص کند:

* مسئله یا Context
* گزینه‌های بررسی‌شده
* تصمیم نهایی
* دلایل انتخاب
* پیامدهای تصمیم

مثال:

```text
docs/decisions/
├── ADR-001-use-event-driven-architecture.md
├── ADR-002-use-kafka.md
└── ADR-003-api-versioning.md
```

تفاوت Architecture و ADR:

```text
Architecture
    → سیستم چگونه طراحی شده است؟

ADR
    → چرا این طراحی یا تکنولوژی را انتخاب کردیم؟
```

اگر یک تصمیم معماری در آینده تغییر کند، بهتر است ADR قبلی حذف یا بازنویسی نشود؛ بلکه یک ADR جدید ایجاد شود که تصمیم قبلی را supersede کند.

---

## `docs/standards/`

شامل استانداردها، اصول و دستورالعمل‌هایی است که در طراحی و توسعه سیستم باید رعایت شوند.

مثال:

```text
docs/standards/
├── architecture.md
├── api.md
└── security.md
```

نمونه موضوعات:

* Architecture Principles
* API Guidelines
* Security Guidelines
* Integration Guidelines
* Coding Standards
* Documentation Standards

تفاوت Standard و ADR:

```text
Standard
    → چه چیزی باید رعایت شود؟

ADR
    → در این پروژه چه تصمیمی گرفتیم و چرا؟
```

---

## `docs/references/`

شامل اطلاعات مرجع و دانش پشتیبان پروژه است که مستقیماً در دسته نیازمندی، معماری یا تصمیم قرار نمی‌گیرد.

```text
docs/references/
├── technologies/
└── business/
```

برای مثال:

* مستندات داخلی تکنولوژی‌ها
* اطلاعات مربوط به Domain
* اصطلاحات کسب‌وکار
* Protocolها
* Technology Reference
* اطلاعات تکمیلی مورد استفاده در طراحی

---

## `docs/ai/`

تمام artefactهای مرتبط با استفاده از **AI در فرآیند تحلیل، طراحی و مستندسازی** در این بخش قرار می‌گیرند.

```text
docs/ai/
├── prompts/
└── context/
```

### `docs/ai/prompts/`

پرامپت‌های قابل استفاده مجدد و Version Controlled در این بخش نگهداری می‌شوند.

مثال:

```text
docs/ai/prompts/
├── architecture-review.md
├── system-design.md
├── requirements-analysis.md
└── adr-generation.md
```

پرامپت‌ها باید به‌عنوان یک **Engineering Asset** در نظر گرفته شوند، نه صرفاً متن‌های موقتی که در ChatGPT یا سایر ابزارهای AI استفاده شده‌اند.

---

### `docs/ai/context/`

اطلاعاتی که برای دادن Context مناسب به AI استفاده می‌شوند در این بخش قرار می‌گیرند.

مثلاً:

```text
docs/ai/context/
└── project-context.md
```

این Context می‌تواند شامل مواردی مانند:

* Business Context
* Technical Context
* Architecture Principles
* Technology Constraints
* Project Constraints
* Domain Context

باشد.

---

## `docs/templates/`

Templateهای استاندارد برای ایجاد مستندات جدید در این بخش نگهداری می‌شوند.

```text
docs/templates/
├── srs.md
├── interview.md
├── adr.md
├── architecture.md
└── prompt.md
```

هدف Templateها این است که مستندات مختلف ساختار یکسان و قابل پیش‌بینی داشته باشند.

---

# چرخه مستندات

ساختار پیشنهادی از چرخه زیر پشتیبانی می‌کند:

```text
                  PM / Stakeholders
                         │
                         ▼
                    Interviews
                         │
                         ▼
                 Requirement Analysis
                         │
                         ▼
                        SRS
                         │
                         ▼
                    Architecture
                         │
                         ▼
                        ADRs
                         │
                         ▼
                        Code
```

در مراحل مختلف می‌توان از AI کمک گرفت:

```text
Interviews ───────┐
Requirements ─────┤
Architecture ─────┼──→ AI Prompts / AI Context
ADRs ─────────────┘
```

با این حال، خروجی تولیدشده توسط AI نباید بدون بررسی انسانی به‌عنوان سند رسمی پروژه پذیرفته شود.

---

# ارتباط بین Artefactها

هدف اصلی این ساختار ایجاد امکان Traceability بین بخش‌های مختلف است.

به‌صورت مفهومی:

```text
Stakeholder
     │
     ▼
Interview
     │
     ▼
Requirement
     │
     ▼
SRS
     │
     ▼
Architecture
     │
     ▼
ADR
     │
     ▼
Implementation
```

در صورت نیاز، هر مرحله می‌تواند به مرحله قبلی خود Reference داشته باشد.

برای مثال یک ADR می‌تواند مشخص کند که تصمیم موردنظر در پاسخ به کدام Requirement گرفته شده است.

---

# اصول اصلی

## 1. مستندات باید Version Controlled باشند

مستندات بخشی از پروژه هستند و باید همانند Source Code در Git نگهداری شوند.

تغییرات مهم مستندات باید از طریق Pull Request بررسی شوند.

---

## 2. Architecture یک سند زنده است

مستندات معماری باید نمایانگر وضعیت فعلی سیستم باشند و همزمان با تغییر سیستم به‌روزرسانی شوند.

---

## 3. تاریخچه تصمیمات حفظ شود

ADRها بخشی از تاریخچه معماری هستند.

اگر تصمیمی تغییر کرد، بهتر است ADR قبلی حذف نشود؛ بلکه یک تصمیم جدید ثبت شود که تصمیم قبلی را supersede می‌کند.

---

## 4. اطلاعات خام و اطلاعات نهایی از هم جدا باشند

مصاحبه‌ها منبع اولیه اطلاعات هستند.

SRS نیازمندی‌های تحلیل و تجمیع‌شده را نگهداری می‌کند.

Architecture طراحی سیستم را توصیف می‌کند.

ADR دلیل تصمیمات مهم معماری را ثبت می‌کند.

در نتیجه:

```text
Interview
    ↓
Requirement
    ↓
SRS
    ↓
Architecture
    ↓
ADR
    ↓
Implementation
```

---

## 5. Promptهای AI نیز باید Version Controlled باشند

پرامپت‌هایی که در فرآیند مهندسی نرم‌افزار ارزش ایجاد می‌کنند باید در Repository نگهداری و به مرور بهبود داده شوند.

به این ترتیب می‌توان تغییرات Promptها و تأثیر آن‌ها بر فرآیند Architecture و Documentation را نیز دنبال کرد.

---

# خلاصه ساختار

| مسیر                            | مسئولیت                  |
| ------------------------------- | ------------------------ |
| `docs/requirements/`            | نیازمندی‌ها              |
| `docs/requirements/srs/`        | SRS                      |
| `docs/requirements/interviews/` | مصاحبه‌ها و جلسات        |
| `docs/architecture/`            | معماری فعلی سیستم        |
| `docs/decisions/`               | تصمیمات معماری و ADR     |
| `docs/standards/`               | استانداردها و Guidelines |
| `docs/references/`              | اطلاعات مرجع             |
| `docs/ai/prompts/`              | پرامپت‌های AI            |
| `docs/ai/context/`              | Context مورد استفاده AI  |
| `docs/templates/`               | Templateهای مستندات      |
| `src/`                          | Source Code              |

## اصل نهایی

این Repository باید به‌عنوان **Single Source of Truth برای دانش و تصمیمات پروژه** در نظر گرفته شود.

ساختار اصلی به شکل زیر است:

```text
repo/
│
├── docs/              ← تمام مستندات و دانش پروژه
│   ├── requirements/
│   ├── architecture/
│   ├── decisions/
│   ├── standards/
│   ├── references/
│   ├── ai/
│   └── templates/
│
└── src/               ← پیاده‌سازی
```

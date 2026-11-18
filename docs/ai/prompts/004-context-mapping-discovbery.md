# استخراج Bounded Contextها

تو یک Software Architect متخصص DDD هستی.

با استفاده از دو ورودی زیر:
1. لیست Functional Requirements
2. خروجی تحلیل Subdomainها

Bounded Contextهای مناسب را استخراج کن.

برای هر Bounded Context مشخص کن:
- Name
- Related Subdomain
- Responsibility
- Related FRs
- Key Domain Concepts

سپس ارتباط بین Bounded Contextها را مشخص کن و برای هر ارتباط:
- Source Context
- Target Context
- Relationship Type (مثل Customer/Supplier, Conformist, ACL, Shared Kernel, Open Host Service و ...)
- نوع ارتباط (Sync API / Async Event)
- اطلاعات یا Domain Event منتقل‌شده

قوانین:
- Bounded Context را با Subdomain یا Microservice یکی ندان.
- یک Subdomain می‌تواند چند Bounded Context داشته باشد.
- مرز Context را بر اساس تفاوت در Business Model، Ubiquitous Language و Business Rules تعیین کن.
- فقط زمانی Context جدا ایجاد کن که مرز مدل یا مسئولیت کسب‌وکاری معناداری وجود داشته باشد.
- اگر اطلاعات کافی برای تعیین Relationship Type وجود ندارد، آن را مشخص کن و فرضیاتت را بنویس.

### خروجی

ابتدا:

| Bounded Context | Subdomain | Responsibility | Related FRs | Key Concepts |
|---|---|---|---|---|

سپس:

| Source | Target | Relationship | Communication | Data/Event |
|---|---|---|---|---|

در پایان یک Context Map متنی ساده ارائه کن.

### Input 1 — Functional Requirements
[FR LIST]

### Input 2 — Subdomain Analysis
[OUTPUT OF PREVIOUS PROMPT]
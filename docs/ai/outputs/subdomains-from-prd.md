حتماً. من FRها را از زاویه **Business Problem / Capability** نگاه کردم، نه از زاویه Entity یا API. بنابراین مثلاً `Doctor`، `Appointment` و `Slot` را خودبه‌خود Subdomain فرض نکردم.

مبنای تحلیل همین سند فعلی `fr-from-srs.md` است؛ خود سند هم اشاره می‌کند که هسته‌ی `Appointment / Slot / Doctor / Schedule` نسبتاً خوب پوشش داده شده، ولی وضعیت Appointment و برخی اختیارات Actorها هنوز ابهام دارند. ([GitHub][1])

## استخراج Subdomainها

| Subdomain                              | Type       | Responsibility                                                                                | Related FRs                                    | Value      | Complexity  | Differentiation | Reason                                                                                                                                                                                                                          |
| -------------------------------------- | ---------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------- | ---------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Appointment Scheduling**             | **Core**   | مدیریت ظرفیت زمانی پزشک، ایجاد Slot، نمایش ظرفیت قابل رزرو و هماهنگی Schedule با Availability | FR-N12–N14, FR-N20–N23, FR-I01–I03             | High       | High        | High            | مسئله اصلی محصول، فراهم‌کردن و مدیریت ظرفیت نوبت پزشک است. منطق Schedule، Slot، Availability و محدودیت‌های زمانی نسبتاً پیچیده است و مستقیماً روی ارزش اصلی سیستم اثر دارد.                                                     |
| **Appointment Management**             | **Core**   | ایجاد، رزرو، لغو، تغییر و مدیریت چرخه عمر Appointment                                         | FR-N24–N39, FR-N44–N47, FR-N69, FR-N72, FR-I05 | **High**   | **High**    | **High**        | مهم‌ترین جریان کسب‌وکار سیستم همین تبدیل ظرفیت به نوبت و مدیریت چرخه عمر آن است. هم ارزش کسب‌وکاری بالاست و هم Business Ruleهای مهمی مثل Cancellation Window، Atomic Reschedule و جلوگیری از Double Booking دارد.               |
| **Provider Management**                | Supporting | مدیریت پزشکان، وضعیت فعال/غیرفعال و اطلاعات پایه آن‌ها                                        | FR-N05–N07                                     | Medium     | Medium      | Low             | وجود پزشک برای سیستم ضروری است، اما طبق FRهای فعلی مزیت رقابتی مستقیمی ایجاد نمی‌کند. بیشتر یک قابلیت پشتیبان برای Scheduling و Appointment است.                                                                                |
| **Specialty & Provider Discovery**     | Supporting | مدیریت تخصص‌ها و فراهم‌کردن امکان پیدا کردن پزشک مناسب                                        | FR-N08–N11, FR-N15–N19                         | Medium     | Medium      | Medium          | Discovery برای تجربه بیمار مهم است، اما در Scope فعلی منطق پیچیده یا مزیت رقابتی مشخصی ندارد. اگر در آینده Matching، Ranking یا Recommendation اضافه شود، ارزیابی این Subdomain می‌تواند تغییر کند.                             |
| **Patient Visit Management**           | Supporting | ثبت و مدیریت وضعیت مراجعه بیمار از انتظار تا تکمیل یا عدم مراجعه                              | FR-N48–N52, FR-I05                             | Medium     | Medium–High | Medium          | این بخش بعد از رزرو اتفاق می‌افتد و ارزش عملیاتی دارد، اما محصول فعلی بیشتر روی Appointment متمرکز است. ضمن اینکه سند هنوز مشخص نکرده `WAITING/IN_PROGRESS` متعلق به Appointment است یا یک Visit/Encounter مستقل. ([GitHub][1]) |
| **Notification & Reminder Management** | Generic    | ایجاد و ارسال Notification و Reminder از طریق کانال‌های مختلف                                 | FR-N53–N58, FR-N71                             | Medium     | Medium      | Low             | Notification تقریباً یک قابلیت عمومی در بسیاری از سیستم‌هاست و معمولاً مزیت رقابتی محسوب نمی‌شود. SMS/Email/Push بیشتر کانال‌های ارائه سرویس هستند تا منطق اصلی کسب‌وکار.                                                       |
| **Identity & Access Management**       | Generic    | ثبت‌نام، ورود، خروج، بازیابی رمز و کنترل دسترسی کاربران                                       | FR-N01–N04, FR-N70                             | Medium     | Medium      | Low             | این قابلیت برای سیستم ضروری است اما مسئله اختصاصی کسب‌وکار Clinic Appointment نیست و راهکارهای استاندارد زیادی برای آن وجود دارد.                                                                                               |
| **Appointment Reporting**              | Supporting | ارائه گزارش و شاخص‌های عملیاتی مربوط به Appointmentها                                         | FR-N59–N67                                     | Medium     | Low–Medium  | Low             | گزارش‌گیری برای مدیریت کلینیک ارزشمند است، اما در وضعیت فعلی بیشتر مصرف‌کننده داده‌های سایر Subdomainهاست و منطق دامنه‌ای پیچیده‌ای ندارد.                                                                                      |
| **Audit & Compliance Tracking**        | Generic    | ثبت رویدادها و عملیات حساس برای Traceability و Audit                                          | FR-N68, FR-I04                                 | Low–Medium | Medium      | Low             | Audit برای کنترل و ردیابی عملیات مهم است، ولی مسئله اختصاصی Appointment نیست و معمولاً یک قابلیت عمومی در سیستم‌های سازمانی است.                                                                                                |

### یک نکته مهم درباره مرز دو Subdomain اصلی

به نظرم **Appointment Scheduling** و **Appointment Management** را نباید یکی کنیم؛ هرچند به‌شدت به هم وابسته‌اند.

تفاوت Business Problem آنها این است:

**Scheduling می‌پرسد:**

> «چه زمانی و با چه ظرفیتی می‌توانیم به بیمار سرویس بدهیم؟»

**Appointment Management می‌پرسد:**

> «وقتی بیمار یک ظرفیت را انتخاب کرد، چرخه عمر این نوبت چگونه مدیریت شود؟»

مثلاً:

`Schedule → Slot → Availability`

بیشتر متعلق به **Appointment Scheduling** است.

اما:

`Book → Cancel → Reschedule → Status → No-Show`

بیشتر متعلق به **Appointment Management** است.

این تفکیک از نظر DDD خیلی ارزشمند است، چون بعداً می‌تواند به دو مدل دامنه متفاوت منجر شود، حتی اگر در MVP نهایتاً هر دو داخل یک Modular Monolith باشند.

---

## 1. کدام FRها بین چند Subdomain مشترک‌اند؟

چند دسته FR عملاً **Cross-Domain Behavior** هستند:

### FR-N05–N07 — Doctor Management

مثلاً غیرفعال‌شدن پزشک مستقیماً روی Appointment Scheduling و Appointment Management اثر دارد.

به‌خصوص:

* `FR-N07`
* `FR-N23`
* `FR-N31`

همگی به مفهوم **Doctor Active/Inactive** متکی هستند.

بنابراین Doctor Management مالک وضعیت پزشک است، ولی Scheduling و Appointment Management از آن به‌عنوان یک Business Rule استفاده می‌کنند.

---

### FR-N12–N14 — Schedule / Slot

این‌ها مرز بسیار جالبی دارند:

* Schedule → Scheduling
* Slot → Scheduling
* اما Slot در Appointment Management مصرف می‌شود.

مثلاً:

`FR-N24 — رزرو Slot`

از نظر Business Action متعلق به Appointment Management است، ولی Slot خودش محصول Scheduling است.

این دقیقاً یکی از جاهایی است که در مرحله Bounded Context باید **Ownership** را مشخص کنیم.

---

### FR-N20–N23

نمایش Slotهای آزاد همزمان به دو مفهوم وابسته است:

* Availability → Scheduling
* Appointment Status → Appointment Management

خصوصاً:

`FR-N22 — Slot دارای Appointment فعال نمایش داده نشود`

یک Rule بین دو Subdomain است.

---

### FR-N24–N39

این مجموعه عمدتاً Appointment Management است، اما تقریباً همه آن‌ها به Scheduling وابسته‌اند.

مثلاً Reschedule:

`Appointment Management → نیازمند Availability از Scheduling`

بنابراین Dependency وجود دارد، ولی لزوماً به معنای یکی بودن Subdomain نیست.

---

### FR-N53–N58

Notificationها با Appointment Lifecycle trigger می‌شوند.

مثلاً:

`Appointment Booked → Notification`

یا:

`Appointment Cancelled → Notification`

پس Notification یک **Supporting/Generic capability مستقل** است که از Appointment Management event دریافت می‌کند.

---

### FR-N59–N67

Reporting تقریباً از همه Subdomainهای عملیاتی داده می‌گیرد:

* Doctor
* Specialty
* Schedule
* Appointment
* Visit

بنابراین Reporting را نباید مالک Business Data اصلی در نظر گرفت.

---

## 2. کدام FRها احتمالاً Feature هستند نه Subdomain؟

اینجا اتفاقاً چند مورد خیلی واضح داریم.

### Search

این‌ها:

* FR-N11
* FR-N15
* FR-N16
* FR-N17
* FR-N18
* FR-N19

به‌تنهایی Subdomain نیستند.

این‌ها **Featureهای Discovery** هستند که روی Provider و Specialty داده می‌خوانند.

پس:

> `Doctor Search` ≠ Subdomain

بلکه بخشی از:

> **Specialty & Provider Discovery**

است.

---

### Appointment History

این‌ها:

* FR-N40
* FR-N41
* FR-N42
* FR-N43

به‌احتمال زیاد Subdomain مستقل نیستند.

این‌ها Query/Featureهای Appointment Management هستند.

---

### Appointment Reporting Filters

این‌ها:

* FR-N60
* FR-N61
* FR-N62
* FR-N63

قطعاً نباید Subdomain شوند.

این‌ها Featureهای Reporting هستند.

---

### Notification Channels

این‌ها:

* FR-N56 — SMS
* FR-N57 — Email
* FR-N58 — Push

سه Subdomain نیستند؛ حتی لزوماً سه Capability مستقل هم نیستند.

این‌ها **Channelهای Notification** هستند.

---

### Appointment Statuses

مثل:

* `WAITING`
* `IN_PROGRESS`
* `COMPLETED`
* `NO_SHOW`

هم Subdomain نیستند.

این‌ها Business Concepts / Stateهای مربوط به Appointment یا احتمالاً Visit هستند.

البته اینجا یک **ابهام دامنه‌ای مهم** وجود دارد: خود سند هم مشخص نکرده که این وضعیت‌ها متعلق به Appointment هستند یا یک مفهوم مستقل مثل Visit/Encounter. ([GitHub][1])

---

### Concurrency Rules

مثل:

* FR-N27
* FR-N28
* FR-N29

این‌ها Subdomain نیستند.

این‌ها **Business Rules / Invariants** در Appointment Management هستند.

مثلاً:

> یک Slot نمی‌تواند همزمان دو Appointment فعال داشته باشد.

این Rule ارزش معماری بالایی دارد، ولی Subdomain نیست.

---

## 3. برای کدام Subdomainها پیشنهاد می‌کنم Bounded Context تعریف شود؟

اینجا باید کمی محافظه‌کار باشیم.

من از روی این FRها **برای همه Subdomainها Bounded Context تعریف نمی‌کنم**.

### اولویت 1 — Appointment Management

**حتماً پیشنهاد می‌کنم.**

این مهم‌ترین candidate برای Bounded Context است.

چون:

* Business Rule زیاد دارد.
* Lifecycle دارد.
* Invariantهای مهم دارد.
* Concurrency دارد.
* Cancellation Policy دارد.
* Reschedule دارد.
* Actorهای مختلف دارد.
* بیشترین Business Value را دارد.

در واقع اینجا احتمالاً مهم‌ترین **Domain Model** پروژه شکل می‌گیرد.

---

### اولویت 2 — Appointment Scheduling

**بسیار محتمل.**

به‌خصوص به دلیل تفاوت مفهومی:

`Schedule → Slot → Availability`

با:

`Appointment → Cancellation → Reschedule`

و همچنین اینکه Availability باید با Appointment هماهنگ شود.

اما در MVP می‌توان این دو Context را داخل یک **Modular Monolith** نگه داشت و فقط مرز ماژولار آنها را روشن کرد.

---

### اولویت 3 — Patient Visit Management

**فعلاً مشروط.**

اگر تصمیم بگیریم:

> Appointment و Visit دو مفهوم مستقل هستند

آن‌وقت جداکردن Bounded Context منطقی می‌شود.

مثلاً:

```text
Appointment
    ↓
Visit / Encounter
    ↓
Waiting
    ↓
In Progress
    ↓
Completed / No Show
```

ولی اگر `WAITING/IN_PROGRESS` صرفاً Stateهای Appointment باشند، ایجاد Context جدا احتمالاً Over-engineering است.

و خود SRS نیز همین موضوع را به‌عنوان ابهام مطرح کرده است. ([GitHub][1])

---

### اولویت 4 — Provider Management

**فعلاً نه.**

Provider Management از نظر Business Concept قابل تشخیص است، اما برای MVP لزوماً ارزش ایجاد Bounded Context مستقل ندارد.

می‌تواند یک ماژول داخل Modular Monolith باشد.

---

### اولویت 5 — Notification

**فعلاً نه.**

اگر سیستم بعداً تبدیل به یک Platform شود و Notification برای چندین Domain استفاده شود، آن‌وقت جداسازی آن منطقی‌تر می‌شود.

---

# جمع‌بندی معماری پیشنهادی

اگر بخواهم این تحلیل را به یک **Domain Map اولیه** تبدیل کنم، فعلاً این تصویر را پیشنهاد می‌دهم:

```text
                    ┌───────────────────────┐
                    │ Provider Management   │
                    │      Supporting       │
                    └───────────┬───────────┘
                                │
                                ▼
┌──────────────────────┐   ┌────────────────────────┐
│ Provider Discovery   │──▶│ Appointment Scheduling │
│     Supporting       │   │         CORE           │
└──────────────────────┘   └───────────┬────────────┘
                                       │
                                       │ Availability
                                       ▼
                              ┌──────────────────────┐
                              │ Appointment          │
                              │ Management           │
                              │        CORE          │
                              └───────┬───────┬──────┘
                                      │       │
                         ┌────────────┘       └─────────────┐
                         ▼                                  ▼
                ┌──────────────────┐              ┌──────────────────┐
                │ Patient Visit    │              │ Notification     │
                │   Supporting     │              │     Generic      │
                └──────────────────┘              └──────────────────┘

                           ┌──────────────────┐
                           │ Reporting        │
                           │   Supporting     │
                           └──────────────────┘
```

### نتیجه مهم من

اگر هدف این تحلیل، آماده‌شدن برای **DDD + Architecture Design** است، من فعلاً این را به‌عنوان مرزهای اصلی نگه می‌دارم:

**Core**

1. Appointment Scheduling
2. Appointment Management

**Supporting**
3. Provider Management
4. Provider Discovery
5. Patient Visit Management — *مشروط به تصمیم درباره Appointment/Visit*
6. Reporting

**Generic**
7. Identity & Access
8. Notification
9. Audit

و یک نکته مهم: **Core بودن را اینجا با Complexity قاطی نکردم.** مثلاً Notification می‌تواند از نظر فنی پیچیده شود، ولی این باعث Core شدنش نمی‌شود. Core جایی است که **مسئله کسب‌وکار اصلی و تمایز محصول** قرار دارد؛ در این سیستم فعلی، آن مرکز ثقل واضحاً Scheduling و Appointment Management است.

همچنین FRهای پیشنهادی `FR-P02` (Waitlist)، `FR-P03` (Appointment Confirmation) و `FR-P10` (چرخه کامل Status) اگر توسط Product Owner تأیید شوند، می‌توانند مرز و اهمیت **Appointment Management** را حتی پررنگ‌تر کنند؛ ولی من عمداً آنها را در طبقه‌بندی فعلی وارد نکردم چون خود سند آنها را Requirement قطعی نمی‌داند. ([GitHub][1])

[1]: https://github.com/amirrahiminejad/clinic-appointment-system/blob/main/ai/outputs/fr-from-srs.md "clinic-appointment-system/ai/outputs/fr-from-srs.md at main · amirrahiminejad/clinic-appointment-system · GitHub"

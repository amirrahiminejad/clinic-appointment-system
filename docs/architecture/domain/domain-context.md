# Domain Context

## هدف

این سند مسئله دامنه، حدود مسئله، قابلیت‌های کسب‌وکاری و تفکیک مسئولیت بین contextهای اصلی سامانه نوبت‌دهی کلینیک را توضیح می‌دهد.

---

## 1. مسئله دامنه

کلینیک باید بتواند ظرفیت پزشکان را به‌صورت Slotهای قابل رزرو عرضه کند، بیماران باید بتوانند Slot مناسب را پیدا کرده و نوبت خود را بدون تعارض ثبت کنند، و کارکنان کلینیک باید بتوانند lifecycle نوبت‌ها را مدیریت کنند.

ارزش اصلی سیستم این است که:

- رزرو اشتباه یا double booking رخ ندهد.
- بیمار تجربه ساده و شفاف برای گرفتن نوبت داشته باشد.
- کلینیک بتواند برنامه پزشکان و نوبت‌ها را با کمترین عملیات دستی کنترل کند.

---

## 2. Capability Map

| Capability | شرح | Context مالک |
| --- | --- | --- |
| Patient Registration & Profile | ثبت‌نام بیمار و نگه‌داری پروفایل | `Patient` |
| Patient Lookup | جستجو و بازیابی بیمار برای عملیات رزرو | `Patient` |
| Doctor Management | ایجاد/ویرایش/غیرفعال‌سازی پزشک | `Doctor` |
| Specialty Management | مدیریت تخصص پزشکان | `Doctor` |
| Schedule Management | تعریف برنامه کاری و استثناها | `Doctor` |
| Slot Offering | تولید/تعریف/غیرفعال‌سازی Slotهای قابل رزرو | `Doctor` |
| Appointment Booking | رزرو نوبت روی Slot آزاد | `Appointment` |
| Appointment Cancellation | لغو نوبت بر اساس policy | `Appointment` |
| Appointment Rescheduling | انتقال نوبت به Slot معتبر دیگر | `Appointment` |
| Appointment Tracking | مشاهده تاریخچه و وضعیت نوبت | `Appointment` |
| Visit Status Tracking | ثبت وضعیت مراجعه و انجام/عدم‌مراجعه | `Appointment` |
| Booking Notification | اعلان ثبت/لغو/جابجایی | `Notification` |
| Reminder Delivery | ارسال Reminder قبل از موعد نوبت | `Notification` |

---

## 3. Core Domain Narrative

هسته اصلی دامنه از جایی شروع می‌شود که یک ظرفیت رزرو (`Slot`) توسط کلینیک عرضه شده و باید به‌صورت اتمیک به یک `Appointment` معتبر تبدیل شود. 

بنابراین هسته پیچیدگی دامنه در این ناحیه قرار دارد:

- اعتبارسنجی Slot برای رزرو
- جلوگیری از double booking
- هماهنگی بین قوانین لغو و جابه‌جایی
- حفظ consistency بین `Appointment` و وضعیت واقعی عرضه Slot

به همین دلیل `Appointment` به‌عنوان Core Domain در نظر گرفته می‌شود و `Doctor` و `Patient` ورودی‌های معتبر این تصمیم را فراهم می‌کنند.

---

## 4. Context Boundaries

### `Patient`

مسئول نگه‌داری اطلاعات بیمار، وضعیت بیمار و داده‌های تماس است. این context مالک اطلاعات هویتی/کسب‌وکاری بیمار است، نه قوانین رزرو.

### `Doctor`

مسئول نگه‌داری پزشک، تخصص، schedule و Slotهای عرضه‌شده است. این context درباره اینکه چه ظرفیتی واقعاً برای رزرو موجود است تصمیم می‌گیرد.

### `Appointment`

مسئول تبدیل Slot معتبر به نوبت، مدیریت وضعیت نوبت، لغو، جابه‌جایی و تاریخچه رزرو است. این context قانون‌گذار اصلی رزرو است.

### `Notification`

مسئول ایجاد و ارسال اعلان‌ها در پاسخ به eventهای کسب‌وکاری است. این context نباید مالک تصمیمات رزرو یا schedule باشد.

---

## 5. High-Level Domain Model

| Concept | توضیح | Context |
| --- | --- | --- |
| `Patient` | هویت و پروفایل بیمار | `Patient` |
| `Doctor` | پزشک قابل رزرو | `Doctor` |
| `Specialty` | تخصص پزشک | `Doctor` |
| `Schedule` | تعریف بازه‌های کاری و استثناها | `Doctor` |
| `Slot` | واحد عرضه قابل رزرو | `Doctor` |
| `Appointment` | تعهد رزرو روی یک Slot | `Appointment` |
| `AppointmentStatus` | وضعیت چرخه عمر نوبت | `Appointment` |
| `VisitStatus` | وضعیت مراجعه/حضور بیمار | `Appointment` |
| `Notification` | پیام یا اعلان قابل ارسال | `Notification` |
| `Reminder` | اعلان زمان‌دار قبل از موعد | `Notification` |

---

## 6. Aggregate Ownership

| Aggregate | Context مالک | دلیل |
| --- | --- | --- |
| `Patient` | `Patient` | منبع حقیقت درباره بیمار |
| `Doctor` | `Doctor` | منبع حقیقت درباره پزشک |
| `Schedule` | `Doctor` | برنامه کاری از هویت پزشک جدا نیست |
| `Slot` | `Doctor` | ظرفیت رزرو از برنامه کاری و سیاست عرضه می‌آید |
| `Appointment` | `Appointment` | رزرو و lifecycle نوبت باید atomically مدیریت شود |
| `Notification` | `Notification` | پیام‌رسانی concerns جداگانه دارد |

---

## 7. Invariants در سطح دامنه

- هر `Slot` فعال در یک لحظه حداکثر یک `Appointment` فعال دارد.
- برای پزشک غیرفعال رزرو جدید مجاز نیست.
- برای Slot غیرفعال یا گذشته رزرو جدید مجاز نیست.
- Slot گذشته قابل رزرو نیست.
- لغو بیمار باید با policy زمانی معتبر انجام شود.
- جابه‌جایی نوبت باید اتمیک باشد و در صورت failure نوبت قبلی حفظ شود.
- بیمار می‌تواند چند نوبت فعال داشته باشد، اما حداکثر یک نوبت فعال با یک پزشک در یک روز تقویمی مجاز است.
- تغییر داده‌های `Patient` یا `Doctor` نباید تاریخچه نوبت‌های قبلی را مخدوش کند.

---

## 8. Domain Decisions for This Baseline

- `Slot` در context `Doctor` قرار می‌گیرد، نه `Appointment`، چون نماینده عرضه ظرفیت کلینیک است.
- `Appointment` برای نمایش و گزارش می‌تواند snapshot حداقلی از `Doctor` و `Patient` نگه دارد، اما مالک منبع حقیقت آن‌ها نیست.
- `VisitStatus` از `AppointmentStatus` جدا در نظر گرفته می‌شود تا مفهوم مراجعه از مفهوم رزرو تفکیک شود.
- `Notification` downstream است و صرفاً مصرف‌کننده eventهای دامنه است.

---

## 9. Scope Notes

- `Identity/Access` فعلاً به‌صورت context مستقل مدل نشده و در baseline دامنه خارج از مرز این سند است.
- `Reporting` فعلاً capability ثانویه است و به‌عنوان consumer read model از contextهای اصلی دیده می‌شود.
- `Payment` و `Multi-Clinic` در MVP خارج از محدوده هستند و در این مدل دامنه وارد نشده‌اند.

---

## 10. Access Boundary Notes

- `Patient` و `Doctor` با ownership-based access کنترل می‌شوند.
- `Receptionist` و `Clinic Admin` role-based business actors هستند.
- `System Admin` بازیگر فنی/امنیتی است و به‌صورت پیش‌فرض در flowهای روزمره booking وارد نمی‌شود.
- مرجع کامل role/permission matrix در `docs/architecture/domain/access-control-matrix.md` قرار دارد.
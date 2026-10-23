# Ubiquitous Language

این سند واژگان مشترک پروژه را برای استفاده یکسان بین محصول، تحلیل، معماری، توسعه و تست تعریف می‌کند.

---

## واژگان اصلی

| Term | تعریف مشترک | Context اصلی |
| --- | --- | --- |
| `Patient` | شخصی که برای دریافت خدمات پزشکی نوبت می‌گیرد | `Patient` |
| `Patient Profile` | مجموعه اطلاعات قابل نگه‌داری درباره بیمار مانند نام، موبایل، تاریخ تولد و اطلاعات تماس | `Patient` |
| `Doctor` | پزشکی که در کلینیک حضور دارد و می‌توان برای او نوبت رزرو کرد | `Doctor` |
| `Specialty` | حوزه تخصصی پزشک مانند قلب، داخلی یا پوست | `Doctor` |
| `Schedule` | تعریف زمان‌های کاری پزشک و استثناهای آن | `Doctor` |
| `Schedule Exception` | تعطیلی یا تغییر موقتی در برنامه کاری پزشک | `Doctor` |
| `Slot` | یک بازه زمانی مشخص که واقعاً برای رزرو عرضه شده است | `Doctor` |
| `Available Slot` | Slot فعال، آینده و بدون نوبت فعال | `Doctor` / `Appointment` |
| `Appointment` | رزرو ثبت‌شده برای یک بیمار روی یک Slot | `Appointment` |
| `Booking` | عمل تبدیل یک Slot آزاد به Appointment معتبر | `Appointment` |
| `Reschedule` | انتقال یک Appointment موجود به Slot آزاد دیگر | `Appointment` |
| `Cancellation` | لغو Appointment طبق policyهای سیستم یا کلینیک | `Appointment` |
| `Cancellation Window` | بازه زمانی مجاز برای لغو توسط بیمار پیش از زمان نوبت | `Appointment` |
| `Appointment Status` | وضعیت چرخه عمر نوبت مثل `BOOKED`, `CANCELLED`, `COMPLETED` | `Appointment` |
| `Visit Status` | وضعیت مراجعه بیمار مثل `WAITING`, `IN_PROGRESS`, `NO_SHOW` | `Appointment` |
| `Reminder` | اعلان زمان‌دار قبل از زمان نوبت | `Notification` |
| `Notification` | پیام کسب‌وکاری ارسالی به بیمار یا ذی‌نفع دیگر | `Notification` |
| `Clinic Staff` | کاربران داخلی مانند منشی یا پذیرش که عملیات اجرایی را انجام می‌دهند | Cross-Context |
| `Clinic Admin` | نقش مدیریتی برای اداره پزشکان، برنامه‌ها، کاربران و گزارش‌ها | Cross-Context |
| `System Admin` | نقش فنی/سیستمی برای تنظیمات و دسترسی‌ها | Cross-Context |

---

## Terms That Must Not Be Confused

| Term A | Term B | تفاوت |
| --- | --- | --- |
| `Schedule` | `Slot` | Schedule الگوی کاری است؛ Slot عرضه واقعی و قابل رزرو است. |
| `Appointment Status` | `Visit Status` | اولی وضعیت رزرو است؛ دومی وضعیت مراجعه/حضور در زمان خدمت. |
| `Doctor Inactive` | `Slot Inactive` | اولی وضعیت پزشک است؛ دومی وضعیت عرضه یک بازه خاص است. |
| `Cancellation` | `Reschedule` | Cancellation رزرو را خاتمه می‌دهد؛ Reschedule رزرو را به Slot جدید منتقل می‌کند. |
| `Patient Account` | `Patient Profile` | Account بیشتر جنبه دسترسی/ورود دارد؛ Profile داده کسب‌وکاری بیمار است. |

---

## Recommended Status Vocabulary

### Appointment Status

- `BOOKED`: رزرو با موفقیت ثبت شده است.
- `CANCELLED`: رزرو لغو شده است.
- `COMPLETED`: نوبت انجام شده است.
- `NO_SHOW`: بیمار مراجعه نکرده است.

در baseline فعلی، `CONFIRMED` در MVP استفاده نمی‌شود و برای نسخه‌های بعدی رزرو شده است.

### Visit Status

- `NOT_STARTED`: مراجعه هنوز آغاز نشده است.
- `WAITING`: بیمار پذیرش شده و منتظر خدمت است.
- `IN_PROGRESS`: خدمت‌دهی در حال انجام است.
- `COMPLETED`: خدمت به پایان رسیده است.
- `NO_SHOW`: بیمار مراجعه نکرده است.

### Slot Status

- `AVAILABLE`: قابل رزرو است.
- `RESERVED`: به دلیل وجود Appointment فعال دیگر قابل رزرو نیست.
- `INACTIVE`: عمداً از عرضه رزرو خارج شده است.
- `EXPIRED`: زمان رزرو آن گذشته است.
- `HELD` یا `PENDING` در baseline فعلی استفاده نمی‌شود مگر در طراحی فنی بعدی.

---

## Language Rules

- در اسناد دامنه از واژه `Slot` برای عرضه رزرو استفاده شود، نه «نوبت».
- از واژه `Appointment` فقط برای رزرو ثبت‌شده استفاده شود.
- برای وضعیت حضور بیمار از `Visit Status` استفاده شود، نه `Appointment Status`.
- `Doctor Availability` به معنی خروجی قابل استفاده برای رزرو است، نه صرفاً حضور فیزیکی پزشک.
- `Notification` شامل پیام رزرو، لغو، جابه‌جایی و Reminder است.

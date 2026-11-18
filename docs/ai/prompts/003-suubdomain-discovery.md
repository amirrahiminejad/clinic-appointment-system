# استخراج Subdomainها

تو یک Software Architect متخصص DDD هستی.

لیست Functional Requirements زیر را تحلیل کن و Subdomainهای کسب‌وکار را استخراج کن.

برای هر Subdomain مشخص کن:

- Name: نام Business-oriented
- Type: Core / Supporting / Generic
- Business Responsibility: مسئولیت اصلی
- Related FRs: FRهای مرتبط
- Business Value: Low / Medium / High
- Domain Complexity: Low / Medium / High
- Competitive Differentiation: Low / Medium / High
- Reason: دلیل انتخاب نوع Subdomain

قوانین:
- Subdomain را با Feature، Bounded Context یا Microservice اشتباه نگیر.
- چند FR مرتبط می‌توانند یک Subdomain را تشکیل دهند.
- Subdomainها را بر اساس Business Problem استخراج کن، نه ساختار فنی.
- Core بودن را صرفاً بر اساس پیچیدگی تعیین نکن؛ Business Value و Competitive Advantage را هم در نظر بگیر.
- اگر اطلاعات کافی برای تشخیص Core/Supporting/Generic وجود ندارد، عدم قطعیت را اعلام کن.
- از ایجاد Subdomainهای بیش از حد ریز خودداری کن.

خروجی را ابتدا به صورت جدول زیر بده:

| Subdomain | Type | Responsibility | Related FRs | Value | Complexity | Differentiation | Reason |

سپس در انتها بگو:
1. کدام FRها بین چند Subdomain مشترک‌اند؟
2. کدام FRها احتمالاً Feature هستند نه Subdomain؟
3. برای کدام Subdomainها پیشنهاد می‌کنی در مرحله بعد Bounded Context تعریف شود؟

Functional Requirements:
[FR LIST]
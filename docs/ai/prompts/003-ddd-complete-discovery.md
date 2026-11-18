تو یک **Domain-Driven Design Architect** ارشد هستی.

من Functional Requirementهای یک محصول نرم‌افزاری را در ادامه ارائه می‌کنم.

وظیفه تو این است که بر اساس FRها، **Bounded Contextها، Aggregateها، Aggregate Rootها، Entityها و Value Objectهای اصلی دامنه** را استخراج و مدل‌سازی کنی.

هدف، ساخت یک **Domain Model واقعی و قابل پیاده‌سازی** است؛ نه صرفاً تبدیل اسم‌های موجود در FR به Entity.

### قوانین DDD

1. ابتدا **Subdomainهای منطقی** را از FRها شناسایی کن.
2. سپس بر اساس مرز مدل، زبان مشترک و مسئولیت‌های کسب‌وکار، Bounded Contextهای مناسب را پیشنهاد بده.
3. برای هر Bounded Context مشخص کن:

   * هدف
   * مسئولیت
   * مفاهیم اصلی
   * خارج از محدوده
4. Aggregateها را بر اساس **Business Invariant و Transaction Boundary** استخراج کن.
5. هر Aggregate باید دقیقاً یک Aggregate Root داشته باشد.
6. هر Entity را صرفاً به دلیل وجود یک ID به Aggregate تبدیل نکن.
7. مشخص کن کدام Entity داخل Aggregate قرار می‌گیرد و چرا.
8. Value Objectها را از مفاهیمی که هویت مستقل ندارند استخراج کن.
9. برای هر Aggregate، Invariantهای مهم را مشخص کن.
10. مشخص کن چه عملیاتی فقط از طریق Aggregate Root مجاز است.
11. Reference بین Aggregateها را با ID مدل کن، نه Object Reference.
12. اگر یک Rule نیازمند همکاری چند Aggregate است، بررسی کن آیا:

    * Domain Service
    * Application Service
    * Domain Event
      مناسب‌تر است.
13. Aggregateها را بیش از حد بزرگ نکن.
14. از ایجاد Aggregate صرفاً برای CRUD جلوگیری کن.
15. اگر یک مفهوم بهتر است در یک Bounded Context دیگر قرار گیرد، دلیل آن را توضیح بده.
16. اگر مرزبندی Microservice از روی FRها قابل تعیین نیست، Microservice را با Bounded Context یکی فرض نکن.
17. اگر اطلاعات کافی برای یک تصمیم وجود ندارد، آن را به‌عنوان فرض یا ابهام مشخص کن.
18. بین **Domain Concept، Entity، Aggregate و Aggregate Root** تفاوت را حفظ کن.

### فرمت خروجی

#### 1. Domain Overview

ابتدا Domain و Subdomainهای قابل تشخیص را توضیح بده.

#### 2. Bounded Contextها

| Bounded Context | هدف | مسئولیت | مفاهیم اصلی | مرز با Contextهای دیگر |
| --------------- | --- | ------- | ----------- | ---------------------- |

#### 3. Context Map

ارتباط Bounded Contextها را توضیح بده و یک دیاگرام Mermaid ارائه کن.

#### 4. Aggregateها

| Bounded Context | Aggregate | Aggregate Root | مسئولیت | Invariantهای اصلی |
| --------------- | --------- | -------------- | ------- | ----------------- |

#### 5. ساختار هر Aggregate

برای هر Aggregate ساختار زیر را ارائه کن:

```text
Aggregate Root
├── Entity
├── Entity
├── Value Object
└── Value Object
```

سپس مسئولیت و Boundary آن را توضیح بده.

#### 6. Entityها و Value Objectها

| مفهوم | نوع | متعلق به Aggregate | دلیل انتخاب |
| ----- | --- | ------------------ | ----------- |

#### 7. Business Invariantها

برای هر Aggregate مهم‌ترین Invariantها را به صورت دقیق بنویس.

#### 8. Domain Serviceها

فقط در صورتی Domain Service پیشنهاد بده که منطق واقعاً متعلق به یک Entity/Aggregate مشخص نباشد.

#### 9. Domain Eventها

Eventهای مهم دامنه را استخراج کن و مشخص کن:

| Event | Trigger | Publisher | Consumer احتمالی | دلیل |
| ----- | ------- | --------- | ---------------- | ---- |

#### 10. ابهامات و فرضیات

مواردی را که برای تصمیم قطعی نیاز به اطلاعات بیشتری دارند مشخص کن.

### Functional Requirements

[FRها را اینجا قرار بده]

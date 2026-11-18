تو یک **Software Architect و Data Architect ارشد** هستی که در طراحی Data Model مبتنی بر DDD تخصص دارد.

در ادامه Functional Requirementهای یک محصول نرم‌افزاری ارائه می‌شود.

وظیفه تو این است که بر اساس FRها و Domain Model استخراج‌شده، یک **Data Model منطقی و قابل پیاده‌سازی** طراحی کنی.

هدف، طراحی Database Schema نیست؛ ابتدا باید **Logical Data Model** را استخراج کنی و سپس در صورت نیاز آن را به Relational Model تبدیل کنی.

### قوانین

1. Data Model باید از نیازمندی‌های کسب‌وکار و Domain Model استخراج شود.
2. بین Domain Model و Persistence Model تفاوت قائل شو.
3. هر Aggregate Boundary را در طراحی Data Model در نظر بگیر.
4. Entityهای اصلی، Attributeها و ارتباط‌های آن‌ها را مشخص کن.
5. برای هر Entity مشخص کن:

   * Primary Key
   * Business Key در صورت وجود
   * Attributeهای اصلی
   * وضعیت‌ها
   * Timestampهای مهم
6. Value Objectها را بررسی کن و مشخص کن:

   * به صورت Embedded ذخیره شوند
   * یا به صورت Table مستقل ذخیره شوند.
7. Reference بین Aggregateها را در Data Model مشخص کن.
8. از ایجاد Foreign Keyهایی که با Aggregate Boundary در تضاد هستند خودداری کن، مگر اینکه دلیل مشخصی وجود داشته باشد.
9. Cardinality تمام ارتباط‌ها را مشخص کن.
10. Constraintهای مهم کسب‌وکاری را از FRها استخراج کن.
11. Unique Constraintهای لازم را مشخص کن.
12. برای جلوگیری از Race Condition و نقض Invariantهای مهم، Constraintهای دیتابیس یا مکانیزم مناسب را پیشنهاد بده.
13. اگر وضعیت یک Entity به State Machine نیاز دارد، Stateها و Transitionها را مشخص کن.
14. Audit Fieldهای ضروری را در صورت نیاز پیشنهاد بده، ولی موارد غیرضروری اضافه نکن.
15. Soft Delete را فقط در صورت وجود نیاز کسب‌وکاری پیشنهاد بده.
16. اطلاعاتی که از FRها قابل استنتاج نیستند را به‌عنوان فرض مشخص کن.
17. تکنولوژی خاص Database را فرض نکن؛ ابتدا مدل منطقی ارائه کن.
18. در نهایت اگر مناسب بود، یک Relational Model پیشنهادی نیز ارائه کن.

### فرمت خروجی

#### 1. موجودیت‌های اصلی

| Entity | Aggregate | Purpose | Primary Key |
| ------ | --------- | ------- | ----------- |

#### 2. Attributeهای هر Entity

برای هر Entity:

```text
Entity
├── id
├── attribute
├── attribute
└── timestamps
```

نوع داده منطقی هر Attribute را نیز مشخص کن.

#### 3. روابط

| Entity A | Relationship | Entity B | Cardinality | توضیح |
| -------- | ------------ | -------- | ----------- | ----- |

#### 4. Business Constraints

| Constraint | سطح اعمال | توضیح |
| ---------- | --------- | ----- |

سطح اعمال را مشخص کن:

* Domain
* Application
* Database

#### 5. ER Diagram

یک ER Diagram با Mermaid ارائه کن.

#### 6. Relational Model پیشنهادی

جداول، PK، FK، Unique Constraint و Indexهای مهم را پیشنهاد بده.

#### 7. Indexهای پیشنهادی

برای Queryهای مهم سیستم Indexهای مناسب را مشخص کن.

#### 8. Consistency و Concurrency

مشخص کن برای Invariantهای مهم چه مکانیزمی برای حفظ Consistency لازم است.

#### 9. Data Model Decisions

تصمیمات مهم طراحی و دلیل آن‌ها را توضیح بده.

#### 10. ابهامات و فرضیات

### Functional Requirements

[FRها را اینجا قرار بده]

### Domain Model در صورت وجود

[خروجی DDD / Aggregateها را اینجا قرار بده]

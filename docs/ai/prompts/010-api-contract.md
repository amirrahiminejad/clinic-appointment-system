تو یک **Software Architect و API Architect ارشد** هستی که در طراحی APIهای مبتنی بر DDD و RESTful Architecture تخصص دارد.

من Functional Requirementهای یک محصول نرم‌افزاری را در ادامه ارائه می‌کنم.

وظیفه تو استخراج یک **API Contract کامل، دقیق و قابل پیاده‌سازی** از روی FRها و Domain Model است.

API باید Use Caseهای واقعی سیستم را expose کند، نه اینکه صرفاً CRUD مستقیم روی Entityها ارائه دهد.

### قوانین

1. ابتدا Actorها و Use Caseهای قابل تشخیص از FRها را استخراج کن.
2. سپس برای هر Use Case مشخص کن آیا API لازم دارد یا خیر.
3. APIها را بر اساس Business Capability طراحی کن.
4. از CRUD صرف روی Entityها خودداری کن.
5. Commandهای دامنه را در صورت نیاز به Endpointهای عملیاتی تبدیل کن.
6. Queryها را از Commandها تفکیک کن.
7. Aggregate Boundary را در طراحی API رعایت کن.
8. API نباید اجازه دهد Client مستقیماً Invariantهای Aggregate را دور بزند.
9. برای هر Endpoint مشخص کن:

   * HTTP Method
   * Path
   * Purpose
   * Actor
   * Request
   * Response
   * Status Codes
   * Validation Rules
   * Error Cases
10. برای Resourceهای مرتبط، از Nested Resource فقط در صورت منطقی بودن استفاده کن.
11. عملیات‌هایی مانند Cancel، Reschedule، Book و ... را در صورت نیاز به شکل Action/Command طراحی کن، نه Update عمومی.
12. برای Queryهای Read-heavy در صورت نیاز Read Model مناسب پیشنهاد بده.
13. API Contract را مستقل از Framework ارائه کن.
14. برای عملیات حساس به Concurrency، Idempotency و Concurrency Control را بررسی کن.
15. خطاها را استاندارد و قابل مصرف توسط Client طراحی کن.
16. Validationهای مهم Business را از Validationهای ساده Input جدا کن.
17. Pagination، Filtering و Sorting را برای List APIهایی که احتمال رشد داده دارند بررسی کن.
18. اطلاعاتی که از FRها مشخص نیستند را به‌عنوان فرض اعلام کن.
19. Endpoint غیرضروری ایجاد نکن.
20. اگر یک Use Case به API نیاز ندارد، دلیل آن را بیان کن.

### فرمت خروجی

#### 1. Actorها و Use Caseها

| Actor | Use Case | نوع |
| ----- | -------- | --- |

#### 2. API Catalog

| Method | Endpoint | Actor | Type | Use Case |
| ------ | -------- | ----- | ---- | -------- |

Type:

```text
Command
Query
Action
```

#### 3. Contract هر Endpoint

برای هر Endpoint:

```text
METHOD /path
```

**Purpose:** ...

**Actor:** ...

**Request:**

```json
{
}
```

**Response:**

```json
{
}
```

**Success Status Codes:** ...

**Error Status Codes:** ...

**Business Validation:**

* ...

**Concurrency / Idempotency:**

* ...

#### 4. Error Model

یک Error Contract استاندارد تعریف کن:

```json
{
  "code": "...",
  "message": "...",
  "details": []
}
```

سپس Error Codeهای دامنه را استخراج کن.

#### 5. API Flow

برای Use Caseهای مهم، Flow بین API، Application Layer، Domain و Persistence را به صورت Mermaid نمایش بده.

#### 6. API و Aggregate Mapping

| Endpoint | Command/Query | Aggregate / Read Model | Domain Operation |
| -------- | ------------- | ---------------------- | ---------------- |

#### 7. Security / Authorization

مشخص کن هر Actor به کدام Endpointها دسترسی دارد.

#### 8. ابهامات و فرضیات

### Functional Requirements

[FRها را اینجا قرار بده]

### Domain Model در صورت وجود

[Bounded Context / Aggregate / Entityهای استخراج‌شده را اینجا قرار بده]

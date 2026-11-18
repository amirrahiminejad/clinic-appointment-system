تو یک متخصص Domain-Driven Design (DDD) هستی.

FRهای زیر را تحلیل کن و از داخل آن‌ها موارد زیر را استخراج کن:

1. **Entity**
2. **Value Object**
3. **Aggregate**
4. **Aggregate Root**

برای هر مورد، یک دلیل کوتاه بر اساس FR ارائه بده.

### قوانین:

* هر چیزی که فقط یک اسم در FR است، الزاماً Entity نیست.
* اگر یک مفهوم Identity مستقل دارد و در طول زمان تغییر می‌کند → Entity.
* اگر فقط با مقدارش شناخته می‌شود و Identity مستقل ندارد → Value Object.
* اگر چند Entity و Value Object به یک Business Rule یا Transaction مربوط هستند → Aggregate.
* مهم‌ترین Entity در Aggregate را به عنوان Aggregate Root مشخص کن.
* اگر برای تصمیم‌گیری اطلاعات کافی نیست، آن مورد را **نامشخص** اعلام کن و فرض بی‌دلیل نداشته باش.

### خروجی:

| Concept | Type                              | Aggregate | Reason |
| ------- | --------------------------------- | --------- | ------ |
| ...     | Entity / Value Object / Aggregate | ...       | ...    |

سپس Aggregateها را به شکل زیر نمایش بده:

**Aggregate: Order**

* Root: Order
* Entities: OrderItem
* Value Objects: Money, Address
* Reason: ...

### FRها:

[FRها را اینجا قرار بده]

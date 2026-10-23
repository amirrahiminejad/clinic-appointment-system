# Doctor Context Language

## Terms

| Term | Meaning |
| --- | --- |
| `Doctor` | پزشک قابل مدیریت و رزرو در سیستم |
| `Specialty` | تخصص منتسب به پزشک |
| `Schedule` | الگوی کاری پزشک |
| `Schedule Exception` | تغییر یا تعطیلی موقت برنامه |
| `Slot` | بازه زمانی عرضه‌شده برای رزرو |
| `Available Slot` | Slot فعال و قابل رزرو از منظر عرضه |
| `Doctor Availability` | خروجی نهایی قابل استفاده برای رزرو |

## Clarifications

- `Schedule` با `Slot` یکسان نیست.
- `Doctor Availability` ممکن است از schedule، exception و policyهای عرضه ساخته شود.
- `Deactivate Doctor` به معنی حذف تاریخچه یا Slotهای گذشته نیست.

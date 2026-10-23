# Appointment Context Language

## Terms

| Term | Meaning |
| --- | --- |
| `Appointment` | نوبت ثبت‌شده برای بیمار روی یک Slot مشخص |
| `Booking` | ثبت موفق نوبت |
| `Cancellation` | لغو نوبت |
| `Reschedule` | انتقال نوبت به Slot جدید |
| `Appointment Status` | وضعیت رزرو |
| `Visit Status` | وضعیت مراجعه بیمار |
| `Active Appointment` | نوبتی که هنوز لغو یا خاتمه نهایی نشده است |
| `Cancellation Window` | بازه مجاز لغو توسط بیمار |

## Forbidden Ambiguities

- `Appointment` با `Slot` یکی نیست.
- `Visit Status` بخشی از حضور/ارائه خدمت است، نه صرفاً رزرو.
- `Completed` در این context به معنی پایان مراجعه یا خدمت است، نه صرفاً ثبت موفق رزرو.

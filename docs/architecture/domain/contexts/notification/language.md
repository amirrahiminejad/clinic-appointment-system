# Notification Context Language

## Terms

| Term | Meaning |
| --- | --- |
| `Notification` | اعلان ایجادشده برای یک رویداد کسب‌وکاری |
| `Reminder` | اعلان زمان‌دار قبل از موعد Appointment |
| `Channel` | روش ارسال مانند SMS، Email یا Push |
| `Delivery Attempt` | یک تلاش مشخص برای ارسال اعلان |
| `Template` | قالب پیام برای یک نوع event |

## Clarifications

- `Notification` معادل event دامنه نیست؛ از event دامنه ساخته می‌شود.
- `Reminder` نوعی notification است، نه خود Appointment.

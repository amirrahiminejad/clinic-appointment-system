# Notification Context Specification

## Purpose

`Notification` context مسئول تولید، زمان‌بندی، ارسال و پیگیری اعلان‌های کسب‌وکاری ناشی از رزرو، لغو، جابه‌جایی و Reminder است.

---

## Business Responsibilities

- ساخت اعلان بعد از eventهای کسب‌وکاری معتبر
- انتخاب کانال مناسب ارسال
- زمان‌بندی Reminder قبل از موعد نوبت
- retry و failure handling ارسال
- نگه‌داری وضعیت ارسال برای audit و پیگیری

---

## Owned Concepts

- `Notification`
- `NotificationTemplate`
- `NotificationChannel`
- `ReminderSchedule`
- `DeliveryAttempt`

---

## Aggregate Design

### Aggregate: `Notification`

- `notificationId`
- `recipientId`
- `recipientChannel`
- `templateType`
- `payload`
- `status`
- `scheduledAt`
- `sentAt`
- `failureReason`

---

## Invariants

- Notification بدون trigger معتبر کسب‌وکاری ایجاد نمی‌شود.
- Reminder باید به زمان نوبت وابسته باشد، نه به زمان ایجاد بیمار یا پزشک.
- failure در ارسال نباید transaction اصلی رزرو را invalid کند، مگر policy دیگری تعریف شود.

---

## Commands

- `CreateBookingNotification`
- `CreateCancellationNotification`
- `CreateRescheduleNotification`
- `ScheduleReminder`
- `DispatchNotification`
- `RetryNotification`

---

## Domain Events

- `NotificationCreated`
- `NotificationScheduled`
- `NotificationSent`
- `NotificationFailed`
- `ReminderScheduled`

---

## Upstream Dependencies

- `AppointmentBooked`
- `AppointmentCancelled`
- `AppointmentRescheduled`
- `ScheduleChanged`
- `SlotDeactivated`
- patient contact data from `Patient`

---

## Boundaries

- این context درباره اعتبار رزرو تصمیم نمی‌گیرد.
- این context مالک داده اصلی `Patient`, `Doctor` یا `Appointment` نیست.
- این context می‌تواند payload snapshot نگه دارد، اما منبع حقیقت حوزه‌های دیگر نیست.

# Appointment Context Specification

## Purpose

`Appointment` context مسئول ثبت، نگه‌داری و کنترل چرخه عمر نوبت‌ها است و هسته اصلی جلوگیری از double booking و اعمال policyهای رزرو، لغو و جابه‌جایی به شمار می‌رود.

---

## Business Responsibilities

- رزرو Slot آزاد برای بیمار معتبر
- جلوگیری از ایجاد بیش از یک نوبت فعال برای یک Slot
- لغو نوبت بر اساس policy
- جابه‌جایی نوبت به Slot معتبر دیگر به‌صورت اتمیک
- مشاهده نوبت‌ها و تاریخچه نوبت‌ها
- ثبت و مدیریت `Appointment Status` و `Visit Status`

---

## Owned Concepts

- `Appointment`
- `AppointmentStatus`
- `VisitStatus`
- `BookingPolicy` در حد rule consumption
- `CancellationDecision`
- `RescheduleDecision`

---

## Upstream Dependencies

### From `Patient`

- `PatientId`
- `PatientStatus`
- `PatientContactInfo`

### From `Doctor`

- `DoctorId`
- `SlotId`
- `SlotAvailability`
- `DoctorStatus`
- `SpecialtySnapshot`

---

## Aggregate Design

### Aggregate: `Appointment`

**Identity:** `AppointmentId`

**Suggested Fields:**

- `appointmentId`
- `patientId`
- `doctorId`
- `slotId`
- `specialtyId` یا `specialtySnapshot`
- `scheduledStart`
- `scheduledEnd`
- `status`
- `visitStatus`
- `bookedAt`
- `cancelledAt`
- `cancellationReason`
- `createdBy`
- `lastModifiedBy`

**Invariants:**

- برای هر `slotId` حداکثر یک `Appointment` فعال وجود دارد.
- `CANCELLED` دیگر نباید به `COMPLETED` تبدیل شود.
- Reschedule فقط به Slot معتبر و آزاد انجام می‌شود.
- لغو توسط بیمار باید داخل `Cancellation Window` باشد.
- بیمار نمی‌تواند بیش از یک `Appointment` فعال با یک `Doctor` در یک روز تقویمی داشته باشد.

---

## Commands

- `BookAppointment`
- `CancelAppointment`
- `RescheduleAppointment`
- `MarkVisitWaiting`
- `MarkVisitInProgress`
- `MarkAppointmentCompleted`
- `MarkAppointmentNoShow`
- `GetPatientAppointments`
- `GetDoctorAppointments`

---

## Domain Events

- `AppointmentBooked`
- `AppointmentCancelled`
- `AppointmentRescheduled`
- `AppointmentCompleted`
- `AppointmentMarkedNoShow`
- `VisitStatusChanged`

---

## Policies and Rules

- booking باید atomically انجام شود.
- در تعارض همزمانی، فقط یک درخواست رزرو باید موفق شود.
- درخواست‌های بازنده در تعارض همزمانی با `APPOINTMENT_SLOT_UNAVAILABLE` رد می‌شوند.
- failure در reschedule نباید نوبت قبلی را از بین ببرد.
- در لغو، تاریخچه حذف نمی‌شود.
- Slot فقط وقتی پس از لغو دوباره available می‌شود که آینده باشد و خود Slot و Doctor فعال باشند.
- بیمار فقط تا ۲ ساعت قبل از زمان نوبت مجاز به لغو یا reschedule است؛ کاربران داخلی مجاز می‌توانند این بازه را override کنند.
- در queryهای نمایش زمان آزاد، Appointment فعال باعث حذف Slot از نتایج می‌شود.

---

## State Model

### Appointment Statuses

- `BOOKED`
- `CANCELLED`
- `COMPLETED`
- `NO_SHOW`

### Visit Statuses

- `NOT_STARTED`
- `WAITING`
- `IN_PROGRESS`
- `COMPLETED`
- `NO_SHOW`

### Key Transition Rules

- `Appointment Status` و `Visit Status` دو state machine جدا هستند.
- Appointment در زمان ایجاد با `status = BOOKED` و `visitStatus = NOT_STARTED` ساخته می‌شود.
- `VisitStatus = COMPLETED` باید به `AppointmentStatus = COMPLETED` منجر شود.
- `VisitStatus = NO_SHOW` باید به `AppointmentStatus = NO_SHOW` منجر شود.
- `AppointmentStatus = CANCELLED` terminal است و بعد از آن visit نباید پیشروی کند.

مرجع کامل transitionها، guardها و actorهای مجاز در `docs/architecture/domain/state-machines.md` آمده است.

---

## Inbound Interfaces

- application service برای رزرو توسط بیمار
- application service برای رزرو/لغو/جابجایی توسط پذیرش
- query service برای مشاهده نوبت‌های بیمار
- query service برای مشاهده نوبت‌های پزشک و لیست روزانه کلینیک

---

## Outbound Interfaces

- query/validation contract به `Patient`
- query/validation contract به `Doctor`
- event publication به `Notification`

---

## Boundaries

- این context مالک `Slot` نیست.
- این context مالک `Patient Profile` یا `Doctor Profile` نیست.
- snapshotهای موردنیاز برای read model و audit مجاز هستند، اما منبع حقیقت نیستند.

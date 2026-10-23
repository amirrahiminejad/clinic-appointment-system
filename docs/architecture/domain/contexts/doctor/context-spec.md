# Doctor Context Specification

## Purpose

`Doctor` context مسئول مدیریت پزشک، تخصص، برنامه کاری و عرضه ظرفیت رزرو است. این context تصمیم می‌گیرد چه بازه‌هایی واقعاً برای رزرو قابل ارائه هستند.

---

## Business Responsibilities

- ثبت و ویرایش پزشک
- فعال/غیرفعال کردن پزشک
- مدیریت تخصص‌ها
- تعریف و ویرایش schedule پزشک
- ثبت استثناهای برنامه کاری و تعطیلی‌ها
- ایجاد یا تولید Slotهای قابل رزرو
- غیرفعال‌سازی Slotهای غیرقابل ارائه

---

## Owned Concepts

- `Doctor`
- `Specialty`
- `Schedule`
- `ScheduleException`
- `Slot`
- `DoctorAvailability`

---

## Aggregate Design

### Aggregate: `Doctor`

- `doctorId`
- `fullName`
- `status`
- `specialties`
- `defaultVisitDuration`

### Aggregate: `Schedule`

- `scheduleId`
- `doctorId`
- `recurrencePattern`
- `workingWindows`
- `exceptions`
- `effectiveFrom`
- `effectiveTo`

### Aggregate: `Slot`

- `slotId`
- `doctorId`
- `scheduledDate`
- `startTime`
- `endTime`
- `status`
- `createdSource` (`manual` یا `generated`)

---

## Invariants

- Slot هم‌پوشان برای یک پزشک در یک بازه مجاز نیست.
- Slot گذشته نباید برای رزرو جدید عرضه شود.
- پزشک غیرفعال نباید Slot جدید قابل رزرو تولید کند.
- Slot غیرفعال نباید در نتایج availability برای رزرو منتشر شود.
- تغییر schedule نباید داده تاریخی Appointment را مستقیماً بازنویسی کند.

---

## Slot State Model

### Slot Statuses

- `AVAILABLE`
- `RESERVED`
- `INACTIVE`
- `EXPIRED`

### Key Transition Rules

- Slot تازه ایجادشده یا تولیدشده در صورت معتبر بودن با `AVAILABLE` شروع می‌شود.
- رزرو موفق Slot را به `RESERVED` می‌برد.
- لغو/جابجایی فقط در صورت برقرار بودن guardها می‌تواند Slot را دوباره `AVAILABLE` کند.
- `INACTIVE` یعنی Slot عمداً از عرضه خارج شده است، نه اینکه لزوماً زمانش گذشته باشد.
- `EXPIRED` terminal است.

مرجع کامل transitionها، guardها و actorهای مجاز در `docs/architecture/domain/state-machines.md` آمده است.

---

## Commands

- `CreateDoctor`
- `UpdateDoctor`
- `DeactivateDoctor`
- `CreateSpecialty`
- `UpdateSpecialty`
- `DeactivateSpecialty`
- `DefineSchedule`
- `ChangeSchedule`
- `AddScheduleException`
- `GenerateSlots`
- `CreateSlot`
- `UpdateSlot`
- `DeactivateSlot`

---

## Domain Events

- `DoctorCreated`
- `DoctorDeactivated`
- `ScheduleDefined`
- `ScheduleChanged`
- `SlotCreated`
- `SlotDeactivated`

---

## Inbound Interfaces

- application service برای مدیریت پزشک توسط مدیر کلینیک
- application service برای مدیریت schedule توسط مدیر یا پزشک مجاز
- query service برای جستجوی پزشک و مشاهده زمان‌های آزاد

---

## Outbound Interfaces

- availability contract برای `Appointment`
- schedule/slot change events برای `Notification`

---

## Boundaries

- این context درباره موفق یا ناموفق بودن رزرو تصمیم نمی‌گیرد.
- این context مالک lifecycle `Appointment` نیست.
- این context داده‌های بیمار را مالک نیست.

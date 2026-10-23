# Patient Context Specification

## Purpose

`Patient` context مسئول مدیریت هویت کسب‌وکاری بیمار، پروفایل بیمار و داده‌های موردنیاز برای رزرو و اطلاع‌رسانی است.

---

## Business Responsibilities

- ثبت‌نام بیمار
- مشاهده و ویرایش پروفایل بیمار
- نگه‌داری داده‌های تماس بیمار
- فراهم کردن lookup معتبر بیمار برای عملیات رزرو و اطلاع‌رسانی

---

## Owned Concepts

- `Patient`
- `PatientProfile`
- `PatientContactInfo`
- `PatientStatus`

---

## Aggregate Design

### Aggregate: `Patient`

- `patientId`
- `firstName`
- `lastName`
- `mobileNumber`
- `dateOfBirth`
- `email`
- `status`
- `createdAt`
- `updatedAt`

---

## Invariants

- شماره موبایل بیمار باید یکتا باشد.
- بیمار فقط به پروفایل خودش دسترسی دارد.
- اطلاعات تماس باید برای اعلان‌های لازم قابل استفاده باشند.

---

## Commands

- `RegisterPatient`
- `UpdatePatientProfile`
- `GetPatientProfile`
- `FindPatientById`
- `FindPatientByMobile`

---

## Domain Events

- `PatientRegistered`
- `PatientProfileUpdated`
- `PatientContactInfoChanged`

---

## Inbound Interfaces

- application service برای ثبت‌نام و ویرایش پروفایل
- query service برای بازیابی پروفایل بیمار
- internal query contract برای جستجوی بیمار توسط پذیرش یا `Appointment`

---

## Outbound Interfaces

- patient validation/query contract برای `Appointment`
- patient contact contract برای `Notification`

---

## Boundaries

- این context مالک قوانین رزرو، لغو یا جابه‌جایی نیست.
- این context مسئول تصمیم‌گیری درباره slot availability نیست.
- concernهای pure authentication می‌توانند در آینده به context یا module جدا منتقل شوند.

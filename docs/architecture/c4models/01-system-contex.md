# C4 Model - Level 1: System Context

```mermaid
C4Context
title System Context Diagram for Clinic Booking System

Person(patient, "بیمار", "ثبت نوبت، پرداخت و دریافت یادآور")
Person(doctor, "پزشک", "مدیریت برنامه زمانی و ویزیت‌ها")
Person(admin, "مدیر کلینیک", "مدیریت پزشکان و گزارش‌گیری")

System(clinic_system, "سیستم نوبت‌دهی کلینیک", "مدیریت کامل فرآیند رزرو، تقویم پزشکان و اعلان‌ها")

System_Ext(payment_gateway, "درگاه پرداخت شاپرک", "پرداخت آنلاین هزینه نوبت")
System_Ext(sms_provider, "سرویس پیامکی", "ارسال پیامک تایید و یادآوری")

Rel(patient, clinic_system, "جستجو و رزرو نوبت", "HTTPS")
Rel(doctor, clinic_system, "ثبت ساعات حضور", "HTTPS")
Rel(admin, clinic_system, "پیکربندی و نظارت", "HTTPS")

Rel(clinic_system, payment_gateway, "تایید تراکنش مالی", "REST API")
Rel(clinic_system, sms_provider, "ارسال پیامک نوبت", "REST API")
```
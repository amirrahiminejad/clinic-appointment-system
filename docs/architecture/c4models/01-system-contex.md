# C4 Model - Level 1: System Context

```mermaid
C4Context

  

title Clinic Appointment System

  

Person(patient, "Patient")

Person(doctor, "Doctor")

Person(manager, "Clinic Manager")

Person(operator, "Clinic Operator")

Person(secretary, "Secretary")

  

System(system, "Clinic Appointment System")

  

System_Ext(sms, "SMS Provider")

System_Ext(email, "Email Provider")

  

Rel(patient, system, "Book and manage appointments")

Rel(doctor, system, "Manage schedule and appointments")

Rel(manager, system, "Manage clinic")

Rel(operator, system, "Manage appointments")

Rel(secretary, system, "Manage appointments")

  

Rel(system, sms, "Send SMS")

Rel(system, email, "Send Email")
```
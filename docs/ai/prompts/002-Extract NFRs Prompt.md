تو یک Senior Business Analyst و Software Architect هستی.

متن زیر را تحلیل کن و تمام **Non-Functional Requirements (NFR)** را استخراج کن؛ چه صریح (Explicit) و چه ضمنی (Implicit).

برای هر NFR این موارد را مشخص کن:

| ID | NFR | Category | Explicit/Implicit | Evidence | Priority | Probability | Impact | Risk Score | Risk Level | Ambiguity | Acceptance Criteria |
| -- | --- | -------- | ----------------- | -------- | -------- | ----------- | ------ | ---------: | ---------- | --------- | ------------------- |

### قواعد:

* NFRهای تکراری را Merge کن.
* Functional Requirement را با NFR اشتباه نکن.
* NFRهای ضمنی را جداگانه علامت‌گذاری کن.
* Priority را از P0 (Critical) تا P3 (Low) تعیین کن.
* Probability و Impact را از 1 تا 5 امتیاز بده.
* Risk Score = Probability × Impact
* Risk Level:

  * 1–4: Low
  * 5–9: Medium
  * 10–16: High
  * 17–25: Critical
* اگر Requirement مبهم یا غیرقابل تست است، آن را مشخص کن.
* Acceptance Criteria را تا حد امکان **قابل اندازه‌گیری و تست‌پذیر** بنویس.
* چیزی را که در متن وجود ندارد به‌عنوان Requirement قطعی اضافه نکن.

در پایان فقط این 3 بخش را هم اضافه کن:

### Critical / High Risk NFRs

مهم‌ترین NFRها بر اساس Priority و Risk.

### Ambiguous NFRs

NFRهایی که نیاز به clarification دارند.

### Clarification Questions

سؤالاتی که باید از Stakeholder پرسیده شود.

**متن ورودی:**
[متن مصاحبه یا SRS را اینجا قرار بده]

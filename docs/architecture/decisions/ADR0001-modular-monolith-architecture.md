# ADR-0001: انتخاب Modular Monolith به‌عنوان Architectural Style

- **وضعیت:** پیشنهادی
    
- **تاریخ:** 2026-09-24
    
- **تصمیم‌گیرندگان:** تیم معماری و توسعه
    

## ۱. مسئله و کانتکست (Context)

سیستم موردنظر یک سیستم نوبت‌دهی کلینیک است که قابلیت‌هایی مانند مدیریت Slotهای پزشکان، رزرو نوبت توسط بیمار، لغو و جابه‌جایی نوبت، مدیریت مراجعه بیمار و ارسال Notification را پوشش می‌دهد.

در فاز فعلی، هدف اصلی توسعه سریع MVP و رسیدن به یک نسخه پایدار با **Fast Time-to-Market** است.

محدودیت‌ها و الزامات اصلی عبارت‌اند از:

- تیم توسعه کوچک و متشکل از ۳ تا ۵ نفر است.
    
- پیچیدگی عملیاتی و DevOps باید حداقل باشد.
    
- در این فاز، نیاز مشخصی برای معماری توزیع‌شده وجود ندارد.
    
- عملیات حساس مانند رزرو Slot باید بتواند با Transactionهای محلی و ساده پیاده‌سازی شود.
    
- در عین سادگی، مرزهای Business و Domain باید شفاف باشند تا سیستم به یک **Big Ball of Mud** تبدیل نشود.
    
- معماری باید امکان استخراج یک یا چند بخش به Microservice در آینده را با کمترین اصطکاک فراهم کند.
    
- Slot و Appointment دارای قواعد کسب‌وکاری مهمی هستند؛ از جمله جلوگیری از رزرو همزمان یک Slot توسط چند بیمار و مدیریت وضعیت Slot پس از لغو نوبت.
    

بنابراین معماری انتخاب‌شده باید بین **سادگی فعلی** و **قابلیت تکامل آینده** تعادل ایجاد کند.

## ۲. گزینه‌های بررسی‌شده (Options Considered)

- **گزینه ۱: Traditional Monolith**
    
    - یک Application یکپارچه با دسترسی آزاد بخش‌های مختلف به یکدیگر.
        
    - ساده و سریع برای شروع، اما مستعد ایجاد Coupling شدید و Big Ball of Mud با رشد سیستم.
        
- **گزینه ۲: Microservices Architecture**
    
    - تفکیک سیستم به چند سرویس مستقل با Deployment و Runtime مستقل.
        
    - قابلیت استقلال و Scale کردن سرویس‌ها را فراهم می‌کند، اما پیچیدگی عملیاتی و Distributed Systems را از ابتدای پروژه وارد می‌کند.
        
- **گزینه ۳: Modular Monolith**
    
    - یک Application و یک Deployment، اما با تقسیم داخلی سیستم به ماژول‌های دارای مسئولیت و مرز مشخص.
        
    - ماژول‌ها از طریق Interface/API داخلی با یکدیگر تعامل می‌کنند و نباید به جزئیات داخلی یکدیگر وابسته باشند.
        

## ۳. تصمیم نهایی (Decision Outcome)

**Modular Monolith به‌عنوان Architectural Style سیستم انتخاب می‌شود.**

سیستم در فاز MVP به صورت یک Application و با یک Database اصلی پیاده‌سازی خواهد شد، اما از نظر Domain و Code Structure به ماژول‌های مستقل تقسیم می‌شود.

ماژول‌های اولیه می‌توانند بر اساس Business Capability تعریف شوند؛ برای مثال:

- Scheduling / Slot Management
    
- Appointment
    
- Notification
    
- Doctor / Patient
    
- Reporting
    

مرزهای ماژول‌ها باید واقعی باشند و وابستگی مستقیم به Implementation داخلی یکدیگر نداشته باشند.

به‌عنوان مثال، ماژول `Appointment` نباید مستقیماً به Repository یا Entity داخلی `Scheduling` وابسته باشد و تعامل باید از طریق قرارداد مشخص ماژول انجام شود.

این انتخاب به دلایل زیر انجام می‌شود:

1. **Fast Time-to-Market:** یک Deployment و یک Runtime باعث کاهش قابل‌توجه پیچیدگی توسعه و استقرار می‌شود.
    
2. **سادگی عملیاتی:** نیازی به مدیریت چندین Service، Network، Service Discovery، Distributed Tracing و Message Broker در MVP وجود ندارد.
    
3. **کاهش Cognitive Load:** تیم ۳ تا ۵ نفره مجبور نیست همزمان با پیچیدگی Domain و پیچیدگی Distributed Systems مقابله کند.
    
4. **Transactionهای ساده‌تر:** عملیات حساس مانند رزرو Slot می‌توانند در یک Transaction محلی Database انجام شوند.
    
5. **مرزبندی Domain:** برخلاف Traditional Monolith، ساختار سیستم از ابتدا بر اساس Business Capability و Domain Boundary شکل می‌گیرد.
    
6. **Decoupling Path:** در صورت نیاز آینده، یک ماژول با مرز مشخص می‌تواند به یک Service مستقل استخراج شود.
    

در صورت افزایش بار یا نیاز به استقلال عملیاتی یک Domain، استخراج آن ماژول به Microservice به‌عنوان یک **Evolutionary Architecture Decision** بررسی خواهد شد و Microservices از ابتدا به‌عنوان الزام معماری در نظر گرفته نمی‌شود.

## ۴. پیامدها (Consequences & Trade-offs)

### مزایا (Pros)

- سرعت بالای توسعه و عرضه MVP
    
- یک Deployment ساده و کم‌هزینه
    
- حداقل پیچیدگی زیرساخت و DevOps
    
- امکان استفاده ساده از Transactionهای محلی
    
- کاهش Cognitive Load برای تیم کوچک
    
- مرزبندی شفاف Business Moduleها
    
- جلوگیری بهتر از ایجاد Big Ball of Mud نسبت به Traditional Monolith
    
- امکان Scale کردن کل Application بدون ورود زودهنگام به Distributed Systems
    
- فراهم شدن مسیر تدریجی برای استخراج Microserviceهای آینده
    

### ریسک‌ها و هزینه‌ها (Cons)

- Modular Monolith همچنان یک Process و معمولاً یک Deployment دارد و استقلال عملیاتی Microserviceها را فراهم نمی‌کند.
    
- Enforce کردن مرزهای Module نیازمند Discipline و Ruleهای معماری در تیم است.
    
- با رشد سیستم، ممکن است برخی Moduleها به دلیل نیازهای متفاوت Scaling یا Deployment به Service مستقل تبدیل شوند.
    
- مهاجرت از Module به Microservice کاملاً بدون هزینه نیست و نیازمند تعریف API، مدیریت ارتباطات و احتمالاً Eventual Consistency خواهد بود.
    
- اگر دسترسی مستقیم Moduleها به Repository، Entity و Database داخلی یکدیگر آزاد باشد، مزیت اصلی Modular Monolith از بین خواهد رفت.
    

**اصل کلیدی این تصمیم:**

> ابتدا Domain Boundaryها را جدا می‌کنیم؛ اگر در آینده نیاز شد، Runtime و Deployment را نیز جدا خواهیم کرد.
# Test Role Definitions (fa-IR)

آزمایش تعاریف نقش (فارسی)

|شناسه          |
|------------|
|WSTG-IDNT-01|

## خلاصه

برنامه ها انواع مختلفی از عملکردها و خدمات دارند و آن ها بر اساس نیاز کاربر به مجوزهای دسترسی نیاز دارند. آن کاربر می تواند:

- یک مدیر (administrator)، جایی که آنها عملکردهای برنامه را مدیریت می کنند.
- یک حسابرس (auditor)، جایی که آنها معاملات برنامه را بررسی می کنند و گزارش مفصلی ارائه می دهند.
- یک مهندس پشتیبانی (support engineer)، جایی که آنها به مشتریان کمک می کنند تا اشکالات را در حساب های خود رفع کنند.
- یک مشتری (customer)، جایی که آنها با برنامه در تعامل هستند و از خدمات آن بهره مند می شوند.

به منظور رسیدگی به این کاربردها و هر مورد دیگر برای آن برنامه، تعاریف نقش تنظیم شده است (که بیشتر به عنوان [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control) شناخته می شود). بر اساس این نقش ها، کاربر قادر به انجام وظیفه مورد نیاز است.

## اهداف آزمایش

- نقش های استفاده شده توسط برنامه را شناسایی و مستند کنید.
- برای جابجایی، تغییر یا دسترسی به نقش دیگری تلاش کنید.
- جزئیات نقش ها و نیازهای پشت مجوزهای داده شده را بررسی کنید.

## چگونه آزمایش کنیم

### شناسایی نقش ها (Roles Identification)

آزمایش‌کننده باید با شناسایی نقش‌های برنامه در حال آزمایش از طریق یکی از روش‌های زیر شروع کند:

- مستندات برنامه.
- راهنمایی توسط توسعه دهندگان یا مدیران برنامه.
- کامنت های (نظرات) برنامه.
- نقش های احتمالی Fuzz:
  - متغیر کوکی (مثلاً `role=admin` ،`isAdmin=True`)
  - متغیر حساب (مثلاً `Role: manager`)
  - دایرکتوری ها یا فایل های مخفی (مثلاً `admin`، `/mod`، `/backups/`)
  - جابجایی به کاربران شناخته شده (مثلاً `admin`، `backups` و غیره)

### جابجایی به نقش های موجود (Switching to Available Roles)

پس از شناسایی بردارهای حمله احتمالی، آزمایش کننده باید آزمایش و اعتبارسنجی کند که می تواند به نقش های موجود دسترسی داشته باشد.

> برخی از برنامه‌ها نقش‌های کاربر را هنگام ایجاد، از طریق بررسی‌ها و سیاست‌های دقیق، یا با اطمینان از اینکه نقش کاربر به درستی از طریق امضای ایجاد شده توسط backend محافظت می‌شود، تعریف می‌کنند. یافتن اینکه نقش ها وجود دارند به این معنی نیست که آنها یک آسیب پذیری هستند.

### بررسی مجوزهای نقش (Review Roles Permissions)

پس از دسترسی به نقش های موجود در سیستم، آزمایش کننده باید مجوزهای ارائه شده برای هر نقش را درک کند.

یک مهندس پشتیبانی نباید بتواند عملکردهای اداری را انجام دهد، پشتیبان‌گیری‌ها را مدیریت کند یا تراکنش‌هایی را به جای کاربر انجام دهد.

یک مدیر نباید قدرت کامل روی سیستم داشته باشد. عملکرد حساس ادمین باید از یک اصل چک‌کننده سازنده استفاده کند یا از MFA برای اطمینان از انجام تراکنش توسط مدیر استفاده کند. یک مثال واضح در این مورد [حادثه توییتر در سال 2020](https://blog.twitter.com/en_us/topics/company/2020/an-update-on-our-security-incident.html) بود.

## ابزارها

آزمایش های فوق را می توان بدون استفاده از هیچ ابزاری به جز ابزاری که برای دسترسی به سیستم استفاده می شود انجام داد.

برای ساده تر و مستندتر کردن کارها، می توان از موارد زیر استفاده کرد:

- [Burp's Autorize extension](https://github.com/Quitten/Autorize)
- [ZAP's Access Control Testing add-on](https://www.zaproxy.org/docs/desktop/addons/access-control-testing/)

## منابع

- [Role Engineering for Enterprise Security Management, E Coyne & J Davis, 2007](https://www.bookdepository.co.uk/Role-Engineering-for-Enterprise-Security-Management-Edward-Coyne/9781596932180)
- [Role engineering and RBAC standards](https://csrc.nist.gov/projects/role-based-access-control#rbac-standard)
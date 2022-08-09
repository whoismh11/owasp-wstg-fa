# Test RIA Cross Domain Policy (fa-IR)

سیاست دامنه های متقابل RIA را آزمایش کنید (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-08|

## خلاصه

ا Rich Internet Applications (RIA) فایل های سیاست (خط مشی) Adobe crossdomain.xml را برای دسترسی کنترل شده بین دامنه به داده ها و مصرف سرویس با استفاده از فناوری هایی مانند Oracle Java، Silverlight و Adobe Flash اتخاذ کرده اند. بنابراین، یک دامنه می تواند دسترسی از راه دور به خدمات خود را از یک دامنه دیگر اعطا کند. با این حال، اغلب فایل‌های سیاست که محدودیت‌های دسترسی را توصیف می‌کنند، پیکربندی ضعیفی دارند. پیکربندی ضعیف فایل‌های سیاست، حملات جعل درخواست بین‌سایتی را فعال می‌کند و ممکن است به اشخاص ثالث اجازه دسترسی به داده‌های حساس برای کاربر را بدهد.

### فایل های سیاست متقابل دامنه چیست؟ (?What are cross-domain policy files)

یک فایل سیاست متقابل دامنه مجوزهایی را مشخص می کند که یک سرویس گیرنده وب مانند جاوا، ادوبی فلش، ادوبی ریدر و غیره برای دسترسی به داده ها در دامنه های مختلف استفاده می کند. برای Silverlight، مایکروسافت زیرمجموعه‌ای از Adobe crossdomain.xml را پذیرفت و علاوه بر آن فایل سیاست متقابل دامنه خود را ایجاد کرد: clientaccesspolicy.xml.

هر زمان که یک سرویس گیرنده وب تشخیص دهد که منبعی باید از دامنه دیگری درخواست شود، ابتدا به دنبال یک فایل سیاست در دامنه هدف می گردد تا تعیین کند که آیا انجام درخواست های بین دامنه، از جمله هدرها و اتصالات مبتنی بر سوکت مجاز است یا خیر.

فایل های سیاست اصلی در ریشه دامنه قرار دارند. ممکن است به یک کلاینت دستور داده شود که فایل سیاست دیگری را بارگیری کند، اما همیشه ابتدا فایل سیاست اصلی را بررسی می کند تا مطمئن شود که فایل سیاست اصلی به فایل سیاست درخواستی اجازه می دهد.

#### <div dir="rtl" align="right">Crossdomain.xml در مقابل Clientaccesspolicy.xml &#x202b;(Crossdomain.xml vs. Clientaccesspolicy.xml)</div>

اکثر برنامه های RIA از crossdomain.xml پشتیبانی می کنند. اما در مورد Silverlight، فقط در صورتی کار می کند که crossdomain.xml مشخص کند که دسترسی از هر دامنه ای مجاز است. برای کنترل دقیق تر با Silverlight، باید از clientaccesspolicy.xml استفاده شود.

فایل های سیاست چندین نوع مجوز را می دهند:

- فایل های سیاست پذیرفته شده (فایل های سیاست اصلی می توانند فایل های سیاست خاصی را غیرفعال یا محدود کنند)
- مجوزهای سوکت
- مجوزهای سرصفحه
- مجوزهای دسترسی HTTP/HTTPS
- اجازه دسترسی بر اساس اعتبار رمزنگاری

نمونه ای از یک فایل سیاست بیش از حد مجاز:

```xml
<?xml version="1.0"?>
<!DOCTYPE cross-domain-policy SYSTEM
"http://www.adobe.com/xml/dtds/cross-domain-policy.dtd">
<cross-domain-policy>
    <site-control permitted-cross-domain-policies="all"/>
    <allow-access-from domain="*" secure="false"/>
    <allow-http-request-headers-from domain="*" headers="*" secure="false"/>
</cross-domain-policy>
```

### چگونه می توان از فایل های سیاست دامنه های متقابل سوء استفاده کرد؟ (?How can cross domain policy files can be abused)

- سیاست های بیش از حد مجاز بین دامنه ای.
- ایجاد پاسخ‌های سرور که ممکن است به عنوان فایل‌های سیاست بین دامنه‌ای در نظر گرفته شوند.
- استفاده از قابلیت آپلود فایل برای آپلود فایل هایی که ممکن است به عنوان فایل های سیاست بین دامنه تلقی شوند.

### تأثیر سوء استفاده از دسترسی بین دامنه ای (Impact of Abusing Cross-Domain Access)

- شکست دادن محافظت های CSRF.
- خواندن داده‌های محدود شده یا محافظت شده توسط سیاست‌های مبدأ متقابل.

## اهداف آزمایش

- فایل های سیاست را بررسی و تایید کنید.

## چگونه آزمایش کنیم

### آزمایش ضعف فایل های سیاست RIA &#x202b;(Testing for RIA Policy Files Weakness)

برای آزمایش ضعف فایل سیاست RIA، آزمایش کننده باید سعی کند فایل های سیاست crossdomain.xml و clientaccesspolicy.xml را از ریشه برنامه و از هر پوشه یافت شده بازیابی کند.

به عنوان مثال، اگر URL برنامه `http://www.owasp.org` است، آزمایش کننده باید سعی کند فایل های `http://www.owasp.org/crossdomain.xml` و `http://www.owasp.org/clientaccesspolicy.xml` را دانلود کند.

پس از بازیابی همه فایل های سیاست، مجوزهای مجاز باید تحت اصل حداقل امتیاز بررسی شوند. درخواست‌ها فقط باید از دامنه‌ها، پورت‌ها یا پروتکل‌هایی که لازم هستند ارائه شوند. از سیاست های بیش از حد سهل گیرانه باید اجتناب شود. سیاست هایی که `*` در آنها وجود دارد باید از نزدیک بررسی شوند.

#### مثال (Example)

```xml
<cross-domain-policy>
    <allow-access-from domain="*" />
</cross-domain-policy>
```

##### نتیجه مورد انتظار (Result Expected)

- فهرستی از فایل های سیاست پیدا شد.
- فهرستی از تنظیمات ضعیف در سیاست ها.

## ابزارها

- Nikto
- OWASP Zed Attack Proxy Project
- W3af

## منابع

- Adobe: ["Cross-domain policy file specification"](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html)
- Adobe: ["Cross-domain policy file usage recommendations for Flash Player"](http://www.adobe.com/devnet/flashplayer/articles/cross_domain_policy.html)
- Oracle: ["Cross-Domain XML Support"](http://www.oracle.com/technetwork/java/javase/plugin2-142482.html#CROSSDOMAINXML)
- MSDN: ["Making a Service Available Across Domain Boundaries"](http://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx)
- MSDN: ["Network Security Access Restrictions in Silverlight"](http://msdn.microsoft.com/en-us/library/cc645032(v=vs.95).aspx)
- Stefan Esser: ["Poking new holes with Flash Crossdomain Policy Files"](http://www.hardened-php.net/library/poking_new_holes_with_flash_crossdomain_policy_files.html)
- Jeremiah Grossman: ["Crossdomain.xml Invites Cross-site Mayhem"](http://jeremiahgrossman.blogspot.com/2008/05/crossdomainxml-invites-cross-site.html)
- Google Doctype: ["Introduction to Flash security"](http://code.google.com/p/doctype-mirror/wiki/ArticleFlashSecurity)
- UCSD: [Analyzing the Crossdomain Policies of Flash Applications](http://cseweb.ucsd.edu/~hovav/dist/crossdomain.pdf)

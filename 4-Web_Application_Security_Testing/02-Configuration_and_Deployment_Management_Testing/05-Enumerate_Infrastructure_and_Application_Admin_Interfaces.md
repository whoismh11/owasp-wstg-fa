# Enumerate Infrastructure and Application Admin Interfaces (fa-IR)

شمارش زیرساخت و رابط های مدیریت برنامه (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-05|

## خلاصه

رابط های مدیر (Administrator interfaces) ممکن است در برنامه یا سرور برنامه وجود داشته باشد تا به کاربران خاصی اجازه انجام فعالیت های ممتاز (privileged activities) در سایت را بدهد. باید آزمایش هایی انجام شود تا مشخص شود که آیا و چگونه می توان به این عملکرد ممتاز توسط یک کاربر غیرمجاز یا استاندارد دسترسی داشت یا خیر.

یک برنامه ممکن است به یک رابط مدیر نیاز داشته باشد تا یک کاربر ممتاز (privileged user) را قادر سازد به عملکردی دسترسی داشته باشد که ممکن است در نحوه عملکرد سایت تغییراتی ایجاد کند. چنین تغییراتی ممکن است شامل موارد زیر باشد:

- تامین حساب کاربری
- طراحی و چیدمان سایت
- دستکاری داده
- تغییرات پیکربندی

در بسیاری از موارد، چنین رابط‌هایی کنترل کافی برای محافظت از آنها در برابر دسترسی غیرمجاز ندارند. هدف آزمایش، کشف این رابط های مدیر و دسترسی به عملکردهای در نظر گرفته شده برای کاربران ممتاز است.

## اهداف آزمایش

- رابط‌ها و عملکرد پنهان مدیر را شناسایی کنید.

## چگونه آزمایش کنیم

### آزمایش جعبه سیاه (Black-Box Testing)

بخش زیر بردارهایی را توصیف می کند که ممکن است برای آزمایش وجود رابط های اداری استفاده شوند. این تکنیک‌ها همچنین ممکن است برای آزمایش مسائل مرتبط از جمله افزایش امتیازات مورد استفاده قرار گیرند و در جای دیگری در این راهنما توضیح داده شده‌اند (به عنوان مثال [Testing for bypassing authorization schema](../05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema.md) و [Testing for Insecure Direct Object References](../05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References.md) با جزئیات بیشتر).

- فهرست و فهرست فایل. یک رابط اداری ممکن است وجود داشته باشد اما به طور قابل مشاهده برای آزمایش کننده در دسترس نباشد. تلاش برای حدس زدن مسیر رابط اداری ممکن است به سادگی درخواست باشد: `admin/` یا `administrator/` و غیره. یا در برخی سناریوها می‌تواند در عرض چند ثانیه با استفاده از [Google dorks](https://www.exploit-db.com/google-hacking-database) آشکار شود.
- ابزارهای زیادی برای انجام محتویات سرور وجود دارد، برای اطلاعات بیشتر به بخش ابزارها زیر مراجعه کنید. آزمایشگر ممکن است مجبور باشد نام فایل صفحه مدیریت را نیز شناسایی کند. مرور اجباری صفحه شناسایی شده ممکن است دسترسی به رابط را فراهم کند.
- نظرات و پیوندها در کد منبع بسیاری از سایت ها از کدهای مشترک استفاده می کنند که برای همه کاربران سایت بارگذاری می شود. با بررسی تمام منابع ارسال شده به مشتری، پیوندهایی به عملکرد مدیر ممکن است کشف شوند و باید بررسی شوند.
- بررسی اسناد سرور و برنامه اگر سرور یا برنامه در پیکربندی پیش‌فرض خود مستقر شده باشد، ممکن است با استفاده از اطلاعاتی که در پیکربندی یا مستندات راهنما توضیح داده شده است، به رابط مدیریت دسترسی پیدا کنید. در صورت یافتن یک رابط مدیریتی و نیاز به اعتبارنامه، باید از فهرست های رمز عبور پیش فرض استفاده شود.
- اطلاعات در دسترس عموم بسیاری از برنامه ها مانند وردپرس دارای رابط های مدیریتی پیش فرض هستند.
- پورت سرور جایگزین رابط های مدیریت ممکن است در پورت متفاوتی نسبت به برنامه اصلی روی هاست دیده شوند. به عنوان مثال، رابط مدیریت آپاچی تامکت (Apache Tomcat) اغلب در پورت 8080 قابل مشاهده است.
- دستکاری پارامترها ممکن است یک پارامتر GET یا POST یا یک متغیر کوکی برای فعال کردن عملکرد مدیر مورد نیاز باشد. سرنخ های این امر شامل وجود فیلدهای مخفی مانند:

```html
<input type="hidden" name="admin" value="no">
```

یا در یک کوکی:

`Cookie: session_cookie; useradmin=0`

هنگامی که یک رابط اداری کشف شد، ترکیبی از تکنیک های فوق ممکن است برای دور زدن احراز هویت استفاده شود. اگر این مورد ناموفق باشد، آزمایش‌کننده ممکن است بخواهد حمله‌ای با نیروی brute force انجام دهد. در چنین نمونه‌ای، آزمایش‌کننده باید از احتمال قفل کردن حساب اداری در صورت وجود چنین عملکردی آگاه باشد.

### آزمایش جعبه خاکستری (Gray-Box Testing)

بررسی دقیق تری از سرور و اجزای برنامه باید انجام شود تا از سخت شدن اطمینان حاصل شود (یعنی صفحات مدیر با استفاده از فیلتر IP یا سایر کنترل ها برای همه قابل دسترسی نیستند)، و در صورت لزوم، تأیید اینکه همه اجزا از اعتبار پیش فرض استفاده نمی کنند یا پیکربندی. کد منبع باید بازنگری شود تا اطمینان حاصل شود که مدل مجوز و احراز هویت از تفکیک واضح وظایف بین کاربران عادی و مدیران سایت اطمینان حاصل می کند. عملکردهای رابط کاربری مشترک بین کاربران عادی و مدیران باید بازنگری شوند تا از جدایی واضح بین ترسیم چنین اجزایی و نشت اطلاعات از چنین عملکرد مشترک اطمینان حاصل شود.

هر چارچوب وب (web framework) ممکن است صفحات یا مسیر پیش‌فرض مدیر خود را داشته باشد. مثلا

WebSphere:

```html
/admin
/admin-authz.xml
/admin.conf
/admin.passwd
/admin/*
/admin/logon.jsp
/admin/secure/logon.jsp
```

PHP:

```html
/phpinfo
/phpmyadmin/
/phpMyAdmin/
/mysqladmin/
/MySQLadmin
/MySQLAdmin
/login.php
/logon.php
/xmlrpc.php
/dbadmin
```

FrontPage:

```html
/admin.dll
/admin.exe
/administrators.pwd
/author.dll
/author.exe
/author.log
/authors.pwd
/cgi-bin
```

WebLogic:

```html
/AdminCaptureRootCA
/AdminClients
/AdminConnections
/AdminEvents
/AdminJDBC
/AdminLicense
/AdminMain
/AdminProps
/AdminRealm
/AdminThreads
```

WordPress:

```html
wp-admin/
wp-admin/about.php
wp-admin/admin-ajax.php
wp-admin/admin-db.php
wp-admin/admin-footer.php
wp-admin/admin-functions.php
wp-admin/admin-header.php
```

## ابزارها

- [OWASP ZAP - Forced Browse](https://www.zaproxy.org/docs/desktop/addons/forced-browse/) is a currently maintained use of OWASP's previous DirBuster project.
- [THC-HYDRA](https://github.com/vanhauser-thc/thc-hydra) is a tool that allows brute-forcing of many interfaces, including form-based HTTP authentication.
- A brute forcer is much better when it uses a good dictionary, for example the [netsparker](https://www.netsparker.com/blog/web-security/svn-digger-better-lists-for-forced-browsing/) dictionary.

## منابع

- [Cirt: Default Password list](https://cirt.net/passwords)
- [FuzzDB can be used to do brute force browsing admin login path](https://github.com/fuzzdb-project/fuzzdb/blob/master/discovery/predictable-filepaths/login-file-locations/Logins.txt)
- [Common admin or debugging parameters](https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/business-logic/CommonDebugParamNames.txt)

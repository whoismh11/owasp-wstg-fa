# Identify Application Entry Points (fa-IR)

شناسایی نقاط ورودی برنامه (فارسی)

|شناسه          |
|------------|
|WSTG-INFO-06|

## خلاصه

برشمردن برنامه و سطح حمله آن یک پیشرو کلیدی قبل از انجام هر گونه آزمایش کامل است، زیرا به آزمایش کننده اجازه می دهد نقاط ضعف احتمالی را شناسایی کند. هدف این بخش کمک به شناسایی و ترسیم مناطق درون برنامه است که باید پس از تکمیل شمارش و نقشه برداری بررسی شوند.

## اهداف آزمایش

- نقاط ورود و تزریق احتمالی را از طریق تجزیه و تحلیل درخواست و پاسخ شناسایی کنید.

## چگونه آزمایش کنیم

قبل از شروع هر آزمایشی، آزمایش کننده باید همیشه درک خوبی از برنامه و نحوه ارتباط کاربر و مرورگر با آن داشته باشد. هنگامی که آزمایش کننده در برنامه قدم می زند، باید به تمام درخواست های HTTP و همچنین هر پارامتر و فیلد فرمی که به برنامه ارسال می شود توجه کند. باید توجه ویژه ای به زمان استفاده از درخواست های GET و زمان استفاده از درخواست های POST برای ارسال پارامترها به برنامه داشته باشد. علاوه بر این، همچنین باید به زمان استفاده از روش های دیگر برای خدمات RESTful توجه کنند.

توجه داشته باشید که برای دیدن پارامترهای ارسال شده در بدنه درخواست ها مانند درخواست POST، آزمایش کننده ممکن است بخواهد از ابزاری مانند یک پروکسی رهگیری (intercepting proxy) استفاده کند (به [ابزارها](#ابزارها) مراجعه کنید). در درخواست POST، آزمایش کننده باید هر فیلد فرم پنهانی را که به برنامه ارسال می شود، یادداشت ویژه ای داشته باشد، زیرا این فیلدها معمولاً حاوی اطلاعات حساسی هستند، مانند اطلاعات وضعیت، تعداد اقلام، قیمت اقلام، در نظر گرفته شده برای هر کسی که ببیند یا تغییر دهد. که توسعه دهنده هرگز آن ها را ندارد.

در تجربه نویسنده، استفاده از یک پروکسی رهگیری و یک صفحه گسترده برای این مرحله از آزمایش بسیار مفید بوده است. پروکسی هر درخواست و پاسخی را که بین آزمایش کننده و برنامه بررسی می کند، پیگیری می کند. علاوه بر این، در این مرحله، آزمایش کننده ها معمولاً هر درخواست و پاسخی را به دام می اندازند تا بتوانند دقیقاً هر هدر، پارامتر و غیره ای را که به برنامه ارسال می شود و آنچه برگردانده می شود، ببینند. این ممکن است گاهی اوقات بسیار خسته کننده باشد، به خصوص در سایت های تعاملی بزرگ (به یک برنامه بانکی فکر کنید). با این حال، تجربه نشان خواهد داد که به دنبال چه چیزی باشید و این مرحله می تواند به میزان قابل توجهی کاهش یابد.

وقتی آزمایش کننده در برنامه قدم می زند، باید هر پارامتر جالب در URL، سرصفحه های سفارشی یا بدنه درخواست ها/پاسخ ها را یادداشت کند و آنها را در صفحه گسترده ذخیره کند. صفحه گسترده باید شامل صفحه درخواست شده باشد (ممکن است خوب باشد که شماره درخواست از پروکسی را نیز برای مراجعات بعدی اضافه کنید)، پارامترهای جالب، نوع درخواست (GET، POST و غیره)، اگر دسترسی احراز هویت یا احراز هویت نشده باشد، اگر از TLS استفاده می شود، اگر بخشی از یک فرآیند چند مرحله ای است، اگر از WebSockets استفاده می شود، و هر یادداشت مرتبط دیگری. هنگامی که آنها هر منطقه از برنامه را ترسیم کردند، سپس می توانند برنامه را مرور کنند و هر یک از مناطقی را که شناسایی کرده اند آزمایش کنند و یادداشت برداری کنند که چه چیزی کار می کند و چه چیزی کار نکرده است. بقیه این راهنما نحوه آزمایش هر یک از این زمینه های مورد علاقه را مشخص می کند.

در زیر برخی از نکات مورد علاقه برای همه درخواست ها و پاسخ ها آمده است. در بخش درخواست ها، روی روش های GET و POST تمرکز کنید، زیرا این روش ها اکثر درخواست ها را نشان می دهند. توجه داشته باشید که از روش های دیگری مانند PUT و DELETE نیز می توان استفاده کرد. اغلب، این درخواست های نادرتر، اگر مجاز باشند، می توانند آسیب پذیری ها را آشکار کنند. بخش ویژه ای در این راهنما برای آزمایش این روش های HTTP اختصاص داده شده است.

### درخواست ها

- محل استفاده از GET و محل استفاده از POST را مشخص کنید.
- تمام پارامترهای مورد استفاده در یک درخواست POST را شناسایی کنید (این پارامترها در بدنه درخواست هستند).
- در درخواست POST، به پارامترهای پنهان توجه ویژه ای داشته باشید. هنگامی که یک POST ارسال می شود، تمام فیلدهای فرم (از جمله پارامترهای پنهان) در متن پیام HTTP به برنامه ارسال می شود. اینها معمولاً دیده نمی شوند مگر اینکه از یک پروکسی یا مشاهده کد منبع HTML استفاده شود. علاوه بر این، صفحه بعدی نشان داده شده، داده های آن و سطح دسترسی بسته به مقدار پارامتر(های) پنهان می تواند متفاوت باشد.
- تمام پارامترهای مورد استفاده در یک درخواست GET (یعنی URL)، به ویژه رشته کوئری (معمولاً پس از علامت ?) را شناسایی کنید.
- تمام پارامترهای رشته پرس و جو را شناسایی کنید. اینها معمولاً در قالب جفت هستند، مانند `foo=bar`. همچنین توجه داشته باشید که بسیاری از پارامترها می توانند در یک رشته پرس و جو باشند، مثلاً با یک `&`, `~\`, `:` یا هر کاراکتر خاص یا رمزگذاری دیگری از هم جدا شوند.
- یک نکته خاص در مورد شناسایی چند پارامتر در یک رشته یا در یک درخواست POST این است که برخی یا همه پارامترها برای اجرای حملات مورد نیاز خواهند بود. آزمایش کننده باید تمام پارامترها را شناسایی کند (حتی اگر رمزگذاری شده باشد) و شناسایی کند که کدام یک توسط برنامه پردازش می شوند. بخش های بعدی راهنما نحوه آزمایش این پارامترها را مشخص می کند. در این مرحله، فقط مطمئن شوید که هر یک از آنها شناسایی شده است.
- همچنین به هر نوع هدر اضافی یا سفارشی که معمولاً دیده نمی شود توجه کنید (مانند `debug: false`).

### پاسخ ها

- محل تنظیم کوکی های جدید (`Set-Cookie` سرصفحه (هدر))، اصلاح یا اضافه شدن به آن را مشخص کنید.
- در طول پاسخ های معمولی (یعنی درخواست های اصلاح نشده) مکان هایی را که تغییر مسیرها وجود دارد (کد وضعیت HTTP 3xx)، کد وضعیت 400، به ویژه 403 ممنوعه (Forbidden)، و 500 خطای داخلی سرور (internal server errors) وجود دارد.
- همچنین توجه داشته باشید که در کجا از هدرهای جالب استفاده می شود. به عنوان مثال، `Server: BIG-IP` نشان می دهد که سایت دارای بار متعادل است. بنابراین، اگر یک سایت دارای بار متعادل باشد و یک سرور به درستی پیکربندی نشده باشد، ممکن است آزمایش کننده مجبور باشد بسته به نوع متعادل سازی بار مورد استفاده، چندین درخواست برای دسترسی به سرور آسیب پذیر ارائه دهد.

### آشکارساز سطح حمله OWASP &#x202b;(OWASP Attack Surface Detector)

ابزار Attack Surface Detector (ASD) کد منبع را بررسی می کند و نقاط پایانی یک برنامه وب، پارامترهایی که این نقاط پایانی می پذیرند و نوع داده آن پارامترها را آشکار می کند. این شامل نقاط پایانی بدون پیوندی است که عنکبوت قادر به یافتن آن نخواهد بود یا پارامترهای اختیاری که کاملاً در کد سمت مشتری (client-side) استفاده نشده اند. همچنین این قابلیت را دارد که تغییرات سطح حمله را بین دو نسخه از یک برنامه محاسبه کند.

آشکارساز سطح حمله به عنوان یک پلاگین (افزونه) برای ZAP و Burp Suite در دسترس است و یک ابزار خط فرمان نیز در دسترس است. ابزار خط فرمان سطح حمله را به عنوان یک خروجی JSON صادر می کند، که سپس می تواند توسط افزونه ZAP و Burp Suite استفاده شود. این برای مواردی مفید است که کد منبع مستقیماً در اختیار آزمایش کننده نفوذ قرار نمی گیرد. برای مثال، آزمایش کننده نفوذ می تواند فایل خروجی json را از مشتری (client) دریافت کند که نمی خواهد خود کد منبع را ارائه کند.

### نحوه استفاده

فایل CLI jar برای دانلود از [https://github.com/secdec/attack-surface-detector-cli/releases](https://github.com/secdec/attack-surface-detector-cli/releases) در دسترس است.

می توانید دستور زیر را برای ASD اجرا کنید تا نقاط پایانی را از کد منبع برنامه وب مورد نظر شناسایی کنید.

`java -jar attack-surface-detector-cli-1.3.5.jar <source-code-path> [flags]`

در اینجا مثالی از اجرای دستور در برابر [OWASP RailsGoat](https://github.com/OWASP/railsgoat) آورده شده است.

```text
$ java -jar attack-surface-detector-cli-1.3.5.jar railsgoat/
Beginning endpoint detection for '<...>/railsgoat' with 1 framework types
Using framework=RAILS
[0] GET: /login (0 variants): PARAMETERS={url=name=url, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/sessions_contro
ller.rb (lines '6'-'9')
[1] GET: /logout (0 variants): PARAMETERS={}; FILE=/app/controllers/sessions_controller.rb (lines '33'-'37')
[2] POST: /forgot_password (0 variants): PARAMETERS={email=name=email, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/
password_resets_controller.rb (lines '29'-'38')
[3] GET: /password_resets (0 variants): PARAMETERS={token=name=token, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/p
assword_resets_controller.rb (lines '19'-'27')
[4] POST: /password_resets (0 variants): PARAMETERS={password=name=password, paramType=QUERY_STRING, dataType=STRING, user=name=user, paramType=QUERY_STRING, dataType=STRING, confirm_password=name=confirm_password, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/password_resets_controller.rb (lines '5'-'17')
[5] GET: /sessions/new (0 variants): PARAMETERS={url=name=url, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/sessions_controller.rb (lines '6'-'9')
[6] POST: /sessions (0 variants): PARAMETERS={password=name=password, paramType=QUERY_STRING, dataType=STRING, user_id=name=user_id, paramType=SESSION, dataType=STRING, remember_me=name=remember_me, paramType=QUERY_STRING, dataType=STRING, url=name=url, paramType=QUERY_STRING, dataType=STRING, email=name=email, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/sessions_controller.rb (lines '11'-'31')
[7] DELETE: /sessions/{id} (0 variants): PARAMETERS={}; FILE=/app/controllers/sessions_controller.rb (lines '33'-'37')
[8] GET: /users (0 variants): PARAMETERS={}; FILE=/app/controllers/api/v1/users_controller.rb (lines '9'-'11')
[9] GET: /users/{id} (0 variants): PARAMETERS={}; FILE=/app/controllers/api/v1/users_controller.rb (lines '13'-'15')
... snipped ...
[38] GET: /api/v1/mobile/{id} (0 variants): PARAMETERS={id=name=id, paramType=QUERY_STRING, dataType=STRING, class=name=class, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/api/v1/mobile_controller.rb (lines '8'-'13')
[39] GET: / (0 variants): PARAMETERS={url=name=url, paramType=QUERY_STRING, dataType=STRING}; FILE=/app/controllers/sessions_controller.rb (lines '6'-'9')
Generated 40 distinct endpoints with 0 variants for a total of 40 endpoints
Successfully validated serialization for these endpoints
0 endpoints were missing code start line
0 endpoints were missing code end line
0 endpoints had the same code start and end line
Generated 36 distinct parameters
Generated 36 total parameters
- 36/36 have their data type
- 0/36 have a list of accepted values
- 36/36 have their parameter type
--- QUERY_STRING: 35
--- SESSION: 1
Finished endpoint detection for '<...>/railsgoat'
----------
-- DONE --
0 projects had duplicate endpoints
Generated 40 distinct endpoints
Generated 40 total endpoints
Generated 36 distinct parameters
Generated 36 total parameters
1/1 projects had endpoints generated
To enable logging include the -debug argument
```

همچنین می توانید یک فایل خروجی JSON با استفاده از پرچم (flag) `json-` ایجاد کنید، که می تواند توسط افزونه برای ZAP و Burp Suite استفاده شود. برای جزئیات بیشتر به لینک های زیر مراجعه کنید.

- [صفحه اصلی پلاگین ASD برای OWASP ZAP](https://github.com/secdec/attack-surface-detector-zap/wiki)
- [صفحه اصلی پلاگین ASD برای PortSwigger Burp](https://github.com/secdec/attack-surface-detector-burp/wiki)

### تست نقاط ورودی برنامه (Testing for Application Entry Points)

در زیر دو مثال در مورد نحوه بررسی نقاط ورودی برنامه آورده شده است.

#### مثال 1

این مثال یک درخواست GET را نشان می دهد که می تواند یک کالا را از یک برنامه خرید آنلاین خریداری کند.

```text
GET /shoppingApp/buyme.asp?CUSTOMERID=100&ITEM=z101a&PRICE=62.50&IP=x.x.x.x HTTP/1.1
Host: x.x.x.x
Cookie: SESSIONID=Z29vZCBqb2IgcGFkYXdhIG15IHVzZXJuYW1lIGlzIGZvbyBhbmQgcGFzc3dvcmQgaXMgYmFy
```

> تمام پارامترهای درخواست مانند CUSTOMERID، ITEM، PRICE، IP و Cookie که فقط می توانند پارامترها یا پارامترهای مورد استفاده برای وضعیت جلسه کدگذاری شوند.

#### مثال 2

این مثال یک درخواست POST را نشان می دهد که شما را به یک برنامه وارد می کند.

```text
POST /example/authenticate.asp?service=login HTTP/1.1
Host: x.x.x.x
Cookie: SESSIONID=dGhpcyBpcyBhIGJhZCBhcHAgdGhhdCBzZXRzIHByZWRpY3RhYmxlIGNvb2tpZXMgYW5kIG1pbmUgaXMgMTIzNA==;CustomCookie=00my00trusted00ip00is00x.x.x.x00

user=admin&pass=pass123&debug=true&fromtrustIP=true
```

می توان توجه داشت که پارامترها در چندین مکان ارسال می شوند:

1. در رشته پرس و جو: `service`
2. در هدر کوکی: `SESSIONID`, `CustomCookie`
3. در بدنه درخواست: `user`, `pass`, `debug`,`fromtrustIP`

وجود انواع مکان های تزریق، فرصت های زنجیره ای را برای مهاجم فراهم می کند که می تواند شانس یافتن یک اشکال در کد مدیریت را افزایش دهد.

## ابزارها

- [OWASP Zed Attack Proxy (ZAP)](https://www.zaproxy.org/)
- [Burp Suite](https://www.portswigger.net/burp/)
- [Fiddler](https://www.telerik.com/fiddler)

## منابع

- [RFC 2616 – Hypertext Transfer Protocol – HTTP 1.1](https://tools.ietf.org/html/rfc2616)
- [OWASP Attack Surface Detector](https://owasp.org/www-project-attack-surface-detector/)

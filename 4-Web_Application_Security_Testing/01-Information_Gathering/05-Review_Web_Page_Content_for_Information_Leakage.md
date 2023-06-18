# Review Web Page Content for Information Leakage (fa-IR)

بررسی محتوای صفحه وب برای نشت اطلاعات (فارسی)

|شناسه          |
|------------|
|WSTG-INFO-05|

## خلاصه

بسیار متداول است و حتی توصیه می شود که برنامه نویسان کامنت ها و متادیتا های دقیق را در کد منبع خود قرار دهند. با این حال، کامنت ها و متادیتا های موجود در کد HTML ممکن است اطلاعات داخلی را نشان دهد که نباید در دسترس مهاجمان بالقوه باشد. بررسی کامنت ها و متادیتا باید انجام شود تا مشخص شود که آیا اطلاعاتی درز کرده است یا خیر. به علاوه برخی از برنامه ها ممکن است اطلاعاتی را در بدنه پاسخ های تغییر مسیر نشت دهند.

برای برنامه های وب مدرن، استفاده از جاوا اسکریپت سمت مشتری (client-side JavaScript) برای قسمت فرانت اند محبوب تر می شود. فناوری های محبوب ساخت وساز فرانت اند از جاوا اسکریپت سمت کلاینت مانند ReactJS ،AngularJS یا Vue استفاده می کنند. مشابه کامنت ها و متادیتا ها در کد HTML، بسیاری از برنامه نویسان نیز اطلاعات حساس را در متغیرهای جاوا اسکریپت در قسمت فرانت اند کدگذاری می کنند. اطلاعات حساس می تواند شامل (اما محدود به این ها نیست): کلیدهای API خصوصی (به عنوان مثال یک کلید API نقشه Google بدون محدودیت)، آدرس های IP داخلی، مسیرهای حساس (مثلا مسیر به صفحات مدیریت مخفی یا عملکرد)، یا حتی اعتبارنامه ها. این اطلاعات حساس را می توان از چنین کدهای جاوا اسکریپت فرانت اند فاش کرد. برای تعیین اینکه آیا اطلاعات حساسی که می تواند توسط مهاجمان برای سوء استفاده استفاده شود، به بیرون درز کرده است، باید بررسی شود.

برای برنامه های وب بزرگ، مسائل مربوط به عملکرد یک نگرانی بزرگ برای برنامه نویسان است. برنامه نویسان از روش های مختلفی برای بهینه سازی عملکرد فرانت اند استفاده کرده اند، از جمله Syntactically Awesome Style Sheets (SASS)، Sassy CSS (SCSS)، webpack و غیره. با استفاده از این فناوری‌ها، گاهی اوقات درک کدهای فرانت اند سخت‌تر و اشکال‌زدایی آن دشوار می‌شود، و به همین دلیل، برنامه‌نویسان اغلب فایل‌های نقشه منبع (source map) را برای اهداف اشکال‌زدایی مستقر می‌کنند. "نقشه منبع" یک فایل ویژه است که یک نسخه کوچک شده/مثل یک دارایی (CSS یا جاوا اسکریپت) را به نسخه تالیف اصلی متصل می کند. برنامه نویسان هنوز در حال بحث هستند که آیا فایل های نقشه منبع را به محیط تولید بیاورند یا نه. با این حال، غیرقابل انکار است که فایل های نقشه منبع یا فایل هایی برای اشکال زدایی در صورت انتشار در محیط تولید، منبع آنها را برای انسان قابل خواندن تر می کند. این می تواند یافتن آسیب پذیری ها از قسمت فرانت اند یا جمع آوری اطلاعات حساس از آن را برای مهاجمان آسان تر کند. بررسی کد جاوا اسکریپت باید انجام شود تا مشخص شود که آیا فایل های اشکال زدایی از قسمت فرانت اند در معرض دید قرار می گیرند یا خیر. بسته به زمینه و حساسیت پروژه، یک کارشناس امنیتی باید تصمیم بگیرد که آیا فایل ها باید در محیط تولید وجود داشته باشند یا خیر.

## اهداف آزمایش

- کامنت های صفحه وب (web page)، متادیتا ها، و تغییر مسیر برای یافتن هرگونه نشت اطلاعات را بررسی کنید.
- فایل های جاوا اسکریپت را جمع آوری کنید و کد JS را برای درک بهتر برنامه و یافتن هرگونه نشت اطلاعات بررسی کنید.
- مشخص کنید که آیا فایل های نقشه منبع یا سایر فایل های اشکال زدایی فرانت اند وجود دارند یا خیر.

## چگونه آزمایش کنیم

### کامنت ها و متادیتا های صفحه وب را مرور کنید (Review Web Page Comments and Metadata)

کامت های HTML اغلب توسط توسعه دهندگان برای گنجاندن اطلاعات اشکال زدایی در مورد برنامه استفاده می شود. گاهی کامنت ها را فراموش می کنند و در محیط های تولیدی رها می کنند. آزمایش کنندگان باید به دنبال کامنت های HTML باشند که با `--!>` شروع میشوند.

کد منبع (سورس کد) HTML را برای کامنت های حاوی اطلاعات حساس که می تواند به مهاجم کمک کند بینش بیشتری در مورد برنامه کسب کند، بررسی کنید. ممکن است کد SQL، نام کاربری و رمز عبور، آدرس IP داخلی یا اطلاعات اشکال زدایی باشد.

```html
...
<div class="table2">
  <div class="col1">1</div><div class="col2">Mary</div>
  <div class="col1">2</div><div class="col2">Peter</div>
  <div class="col1">3</div><div class="col2">Joe</div>

<!-- Query: SELECT id, name FROM app.users WHERE active='1' -->

</div>
...
```

آزمایش کننده حتی ممکن است چیزی شبیه به این پیدا کند:

```html
<!-- Use the DB administrator password for testing:  f@keP@a$$w0rD -->
```

اطلاعات نسخه HTML را برای شماره های نسخه معتبر و URL های تعریف نوع داده (DTD) بررسی کنید.

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

- `strict.dtd` -- &#x202b;DTD سخت پیش فرض
- `loose.dtd` -- &#x202b;DTD شل
- `frameset.dtd` -- &#x202b;DTD برای اسناد فریم ست

برخی از `META` تگ ها بردارهای حمله فعال را ارائه نمی کنند، اما در عوض به مهاجم اجازه می دهند تا یک برنامه را نمایه کند:

```html
<META name="Author" content="Andrew Muller">
```

یک تگ رایج `META` (اما نه مطابق با [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/)) [Refresh](https://en.wikipedia.org/wiki/Meta_refresh) است.

```html
<META http-equiv="Refresh" content="15;URL=https://www.owasp.org/index.html">
```

استفاده رایج از تگ `META`، تعیین کلمات کلیدی است که یک موتور جستجو ممکن است برای بهبود کیفیت نتایج جستجو استفاده کند.

```html
<META name="keywords" lang="en-us" content="OWASP, security, sunshine, lollipops">
```

اگرچه اکثر وب سرورها فهرست (ایندکس) کردن موتور جستجو را از طریق فایل `robots.txt` مدیریت می کنند، اما می توان آن را با `META` تگ ها نیز مدیریت کرد. تگ زیر به ربات ها توصیه می کند که پیوند (لینک) های موجود در صفحه HTML حاوی برچسب را ایندکس نکنند و دنبال نکنند.

```html
<META name="robots" content="none">
```

[پلتفرم برای انتخاب محتوای اینترنتی (PICS)](https://www.w3.org/PICS/) و [پروتکل برای منابع توصیف وب (POWDER)](https://www.w3.org/2007/powder/) زیرساختی را برای مرتبط کردن متادیتا با محتوای اینترنتی فراهم می کند.

### شناسایی کد جاوا اسکریپت و جمع آوری فایل های جاوا اسکریپت (Identifying JavaScript Code and Gathering JavaScript Files)

برنامه نویسان اغلب اطلاعات حساس را با متغیرهای جاوا اسکریپت در فرانت اند هاردکد می کنند. آزمایش کنندگان باید کد منبع HTML را بررسی کنند و به دنبال کد جاوا اسکریپت بین تگ های `<script>` و `<script/>` بگردند. آزمایش کنندگان همچنین باید فایل های جاوا اسکریپت خارجی را برای بررسی کد شناسایی کنند (فایل های جاوا اسکریپت دارای پسوند `js.` و نام فایل جاوا اسکریپت هستند که معمولاً در ویژگی `src` (منبع) یک تگ `<script>` قرار می گیرند).

کد جاوا اسکریپت را برای هرگونه نشت اطلاعات حساس که می تواند توسط مهاجمان برای سوء استفاده بیشتر یا دستکاری سیستم مورد استفاده قرار گیرد، بررسی کنید. به دنبال مقادیری مانند: کلیدهای API، آدرس های IP داخلی، مسیرهای حساس یا اعتبارنامه ها باشید. مثلا:

```javascript
const myS3Credentials = {
  accessKeyId: config('AWSS3AccessKeyID'),
  secretAccessKey: config('AWSS3SecretAccessKey'),
};
```

آزمایشگر حتی ممکن است چیزی شبیه به این پیدا کند:

```javascript
var conString = "tcp://postgres:1234@localhost/postgres";
```

وقتی یک کلید API پیدا شد، آزمایش کنندگان می توانند بررسی کنند که آیا محدودیت های کلید API برای هر سرویس یا توسط IP، ارجاع دهنده HTTP، برنامه، SDK و غیره تنظیم شده اند.

برای مثال، اگر آزمایش کنندگان یک کلید Google Map API پیدا کنند، می توانند بررسی کنند که آیا این کلید API توسط IP محدود شده است یا فقط برای APIهای Google Map محدود شده است. اگر کلید Google API فقط بر اساس APIهای Google Map محدود شده باشد، مهاجمان همچنان می توانند از آن کلید API برای جستجو در APIهای نامحدود Google Map استفاده کنند و مالک برنامه باید هزینه آن را بپردازد.

```html
<script type="application/json">
...
{"GOOGLE_MAP_API_KEY":"AIzaSyDUEBnKgwiqMNpDplT6ozE4Z0XxuAbqDi4", "RECAPTCHA_KEY":"6LcPscEUiAAAAHOwwM3fGvIx9rsPYUq62uRhGjJ0"}
...
</script>
```

در برخی موارد، آزمایش کنندگان ممکن است مسیرهای حساسی را از کد جاوا اسکریپت پیدا کنند، مانند پیوندهایی به صفحات مدیریت داخلی یا پنهان.

```html
<script type="application/json">
...
"runtimeConfig":{"BASE_URL_VOUCHER_API":"https://staging-voucher.victim.net/api", "BASE_BACKOFFICE_API":"https://10.10.10.2/api", "ADMIN_PAGE":"/hidden_administrator"}
...
</script>
```

### شناسایی فایل های نقشه منبع (Identifying Source Map Files)

فایل های نقشه منبع معمولاً با باز شدن DevTools بارگذاری می شوند. آزمایش کنندگان همچنین می توانند فایل های نقشه منبع را با افزودن پسوند "map." پس از پسوند هر فایل جاوا اسکریپت خارجی پیدا کنند. برای مثال، اگر آزمایش کننده ای فایل `static/js/main.chunk.js/` را ببیند، می تواند با مراجعه به فایل نقشه منبع آن را بررسی کند `static/js/main.chunk.js.map/`.

فایل های نقشه منبع را برای هرگونه اطلاعات حساسی که می تواند به مهاجم کمک کند بینش بیشتری در مورد برنامه کسب کند، بررسی کنید. مثلا:

  ```json
{
  "version": 3,
  "file": "static/js/main.chunk.js",
  "sources": [
    "/home/sysadmin/cashsystem/src/actions/index.js",
    "/home/sysadmin/cashsystem/src/actions/reportAction.js",
    "/home/sysadmin/cashsystem/src/actions/cashoutAction.js",
    "/home/sysadmin/cashsystem/src/actions/userAction.js",
    "..."
  ],
  "..."
}
```

وقتی سایت ها فایل های نقشه منبع را بارگیری می کنند، کد منبع فرانت اند قابل خواندن و اشکال زدایی آسان تر می شود.

### شناسایی پاسخ های تغییر مسیر که اطلاعات درز می کنند (Identify Redirect Responses which Leak Information)

اگرچه به طور کلی انتظار نمی رود که پاسخ های تغییر مسیر حاوی محتوای وب قابل توجهی باشند، هیچ اطمینانی وجود ندارد که آنها نمی توانند حاوی محتوا باشند. بنابراین، در حالی که پاسخ های سری 300 (redirect) اغلب حاوی محتوای نوع "`/redirecting to https://example.com`" هستند، ممکن است محتوا نیز به بیرون درز کند.

وضعیتی را در نظر بگیرید که در آن یک پاسخ تغییر مسیر نتیجه یک بررسی احراز هویت یا مجوز است، اگر این بررسی ناموفق باشد، سرور ممکن است پاسخ دهد و کاربر را به صفحه "ایمن" یا "پیش فرض" هدایت کند، اما پاسخ تغییر مسیر ممکن است همچنان حاوی محتوا باشد. که در مرورگر نشان داده نمی شود اما در واقع به مشتری (client) منتقل می شود. این را می توان با استفاده از ابزارهای توسعه دهنده مرورگر یا از طریق یک پروکسی شخصی (مانند ZAP، Burp، Fiddler یا Charles) مشاهده کرد.

## ابزارها

- [Wget](https://www.gnu.org/software/wget/wget.html)
- Browser "view source" function
- Eyeballs
- [Curl](https://curl.haxx.se/)
- [Zaproxy](https://www.zaproxy.org)
- [Burp Suite](https://portswigger.net/burp)
- [Waybackurls](https://github.com/tomnomnom/waybackurls)
- [Google Maps API Scanner](https://github.com/ozguralp/gmapsapiscanner/)

## منابع

- [KeyHacks](https://github.com/streaak/keyhacks)
- [RingZer0 Online CTF](https://ringzer0ctf.com/challenges/104) - Challenge 104 "Admin Panel".

### کاغذهای سفید (Whitepapers)

- [HTML version 4.01](https://www.w3.org/TR/1999/REC-html401-19991224)
- [XHTML](https://www.w3.org/TR/2010/REC-xhtml-basic-20101123/)
- [HTML version 5](https://www.w3.org/TR/html5/)
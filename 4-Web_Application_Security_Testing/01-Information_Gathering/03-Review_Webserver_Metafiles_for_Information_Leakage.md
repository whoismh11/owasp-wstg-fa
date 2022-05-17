# Review Webserver Metafiles for Information Leakage (fa-IR)

بررسی متافایل های وب سرور برای نشت اطلاعات (فارسی)

|شناسه          |
|------------|
|WSTG-INFO-03|

## خلاصه

این بخش نحوه آزمایش فایل های متادیتا مختلف را برای نشت اطلاعات مسیر(ها) یا عملکرد برنامه وب توضیح می دهد. علاوه بر این، فهرست دایرکتوری هایی که باید توسط عنکبوت ها، ربات ها یا خزنده ها اجتناب شود، می تواند به عنوان یک وابستگی برای مسیرهای اجرای نقشه از طریق برنامه ([Map execution paths through application](07-Map_Execution_Paths_Through_Application.md)) ایجاد شود. اطلاعات دیگری نیز ممکن است برای شناسایی سطح حمله، جزئیات فناوری، یا برای استفاده در تعامل مهندسی اجتماعی جمع آوری شود.

## اهداف آزمایش

- شناسایی مسیرها و عملکردهای مخفی یا مبهم از طریق تجزیه و تحلیل فایل های متادیتا.
- استخراج و نقشه برداری اطلاعات دیگری که می تواند منجر به درک بهتر سیستم های موجود شود.

## چگونه آزمایش کنیم

> هر یک از اقدامات انجام شده در زیر با `wget` و با `curl` نیز می تواند انجام شود. بسیاری از ابزارهای Dynamic Application Security Testing (DAST) (تست امنیت برنامه های پویا) مانند ZAP و Burp Suite شامل بررسی یا تجزیه این منابع به عنوان بخشی از عملکرد عنکبوت/خزنده خود هستند. آنها همچنین می توانند با استفاده از [Google Dorks](https://en.wikipedia.org/wiki/Google_hacking) مختلف یا استفاده از ویژگی های جستجوی پیشرفته مانند `:inurl` باشند.

### ربات ها (Robots)

عنکبوت های وب، ربات ها یا خزنده ها یک صفحه وب را بازیابی می کنند و سپس به صورت بازگشتی از لینک ها عبور می کنند تا محتوای وب بیشتری را بازیابی کنند. رفتار پذیرفته شده آنها توسط پروتکل حذف ربات ها ([Robots Exclusion Protocol](https://www.robotstxt.org)) فایل [robots.txt](https://www.robotstxt.org/) در فهرست اصلی وب مشخص شده است.

به عنوان مثال، ابتدای فایل `robots.txt` از [Google](https://www.google.com/robots.txt) نمونه برداری شده در 5 مه 2020 در زیر نقل شده است:

```text
User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
Disallow: /sdch
...
```

دستورالعمل عامل کاربر ([User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)) به عنکبوت/ربات/خزنده وب خاص اشاره دارد. به عنوان مثال، `User-Agent: Googlebot` به عنکبوت از گوگل اشاره دارد در حالی که `User-Agent: bingbot` به خزنده ای از مایکروسافت اشاره دارد. `* :User-Agent` در مثال بالا برای همه [عنکبوت ها / ربات ها / خزنده های وب](https://support.google.com/webmasters/answer/6062608?visit_id=637173940975499736-3548411022&rd=1) اعمال می شود.

این دستورالعمل `Disallow` مشخص می کند که کدام منابع توسط عنکبوت ها / ربات ها / خزنده ها ممنوع است. در مثال بالا موارد زیر ممنوع است:

```text
...
Disallow: /search
...
Disallow: /sdch
...
```

عنکبوت ها/ربات ها/خزنده های وب می توانند عمدا دستورالعمل های `Disallow` مشخص شده در یک فایل `robots.txt` را نادیده بگیرند ([intentionally ignore](https://blog.isc2.org/isc2_blog/2008/07/the-attack-of-t.html)). از این رو، `robots.txt` نباید به عنوان مکانیزمی برای اعمال محدودیت در نحوه دسترسی، ذخیره یا انتشار مجدد محتوای وب توسط اشخاص ثالث در نظر گرفته شود.

فایل `robots.txt` از دایرکتوری ریشه وب سرور وب بازیابی می شود. به عنوان مثال، بازیابی `robots.txt` از `www.google.com` با استفاده از `wget` یا `curl`:

```bash
$ curl -O -Ss http://www.google.com/robots.txt && head -n5 robots.txt
User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
...
```

### تجزیه و تحلیل robots.txt با استفاده از Google Webmaster Tools

صاحبان وب سایت می توانند از "Google "Analyze robots.txt برای تجزیه و تحلیل وب سایت به عنوان بخشی از [Google Webmaster Tools](https://www.google.com/webmasters/tools) استفاده کنند. این ابزار می تواند به آزمایش کمک کند و روش آن به شرح زیر است:

1. با یک حساب Google وارد Google Webmaster Tools شوید.
2. در داشبورد، آدرس سایت مورد تجزیه و تحلیل را وارد کنید.
3. از بین روش های موجود یکی را انتخاب کنید و دستورالعمل روی صفحه را دنبال کنید.

### متاتگ ها (META Tags)

تگ های `<META>` در بخش `HEAD` هر سند HTML قرار دارند و در صورتی که نقطه شروع ربات/عنکبوت/خزنده از پیوند سندی غیر از webroot یعنی پیوند عمیق ([deep link](https://en.wikipedia.org/wiki/Deep_linking)) شروع نشود، باید در سراسر یک وب سایت سازگار باشند. دستورالعمل ربات ها را می توان از طریق استفاده از یک متاتگ ([META tag](https://www.robotstxt.org/meta.html)) خاص نیز مشخص کرد.

#### متاتگ ربات ها (Robots META Tag)

اگر ورودی `< ... "META NAME="ROBOTS>` وجود نداشته باشد، "پروتکل حذف روبات ها (Robots Exclusion Protocol)" به ترتیب به صورت پیش فرض `INDEX,FOLLOW` در نظر گرفته می شود. بنابراین، دو ورودی معتبر دیگر که توسط "پروتکل حذف روبات ها" تعریف شده اند با پیشوند `...NO` یعنی `NOINDEX` و `NOFOLLOW` هستند.

بر اساس دستور(های) Disallow فهرست شده در `robots.txt` فایل در webroot، جستجوی عبارات منظم برای `"META NAME="ROBOTS>` هر صفحه وب انجام می شود و نتیجه با `robots.txt` فایل در webroot مقایسه می شود.

#### تگ های متفرقه اطلاعاتی متا (Miscellaneous META Information Tags)

سازمان ها اغلب متاتگ های اطلاعاتی (informational META tags) را در محتوای وب تعبیه می کنند تا از فناوری های مختلفی مانند صفحه خوان ها، پیش نمایش شبکه های اجتماعی، فهرست سازی موتورهای جستجو و غیره پشتیبانی کنند. چنین فرااطلاعاتی می تواند برای آزمایش کنندگان در شناسایی فناوری های مورد استفاده، و مسیرها/کارکردهای اضافی برای کاوش و آزمایش ارزشمند باشد. متا اطلاعات (meta information) زیر از `www.whitehouse.gov` طریق View Page Source در 5 مه 2020 بازیابی شده است:

```html
...
<meta property="og:locale" content="en_US" />
<meta property="og:type" content="website" />
<meta property="og:title" content="The White House" />
<meta property="og:description" content="We, the citizens of America, are now joined in a great national effort to rebuild our country and to restore its promise for all. – President Donald Trump." />
<meta property="og:url" content="https://www.whitehouse.gov/" />
<meta property="og:site_name" content="The White House" />
<meta property="fb:app_id" content="1790466490985150" />
<meta property="og:image" content="https://www.whitehouse.gov/wp-content/uploads/2017/12/wh.gov-share-img_03-1024x538.png" />
<meta property="og:image:secure_url" content="https://www.whitehouse.gov/wp-content/uploads/2017/12/wh.gov-share-img_03-1024x538.png" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:description" content="We, the citizens of America, are now joined in a great national effort to rebuild our country and to restore its promise for all. – President Donald Trump." />
<meta name="twitter:title" content="The White House" />
<meta name="twitter:site" content="@whitehouse" />
<meta name="twitter:image" content="https://www.whitehouse.gov/wp-content/uploads/2017/12/wh.gov-share-img_03-1024x538.png" />
<meta name="twitter:creator" content="@whitehouse" />
...
<meta name="apple-mobile-web-app-title" content="The White House">
<meta name="application-name" content="The White House">
<meta name="msapplication-TileColor" content="#0c2644">
<meta name="theme-color" content="#f5f5f5">
...
```

### نقشه های سایت (Sitemaps)

نقشه سایت (sitemap) فایلی است که در آن یک توسعه دهنده یا سازمان می تواند اطلاعاتی در مورد صفحات، ویدیوها و سایر فایل های ارائه شده توسط سایت یا برنامه و ارتباط بین آنها ارائه دهد. موتورهای جستجو می توانند از این فایل برای بررسی هوشمندانه سایت شما استفاده کنند. آزمایش کنندگان می توانند از فایل های `sitemap.xml` برای کسب اطلاعات بیشتر درباره سایت یا برنامه استفاده کنند تا آن را به طور کامل کاوش کنند.

گزیده زیر از نقشه سایت اصلی گوگل است که در 5 می 2020 بازیابی شده است.

```xml
$ wget --no-verbose https://www.google.com/sitemap.xml && head -n8 sitemap.xml
2020-05-05 12:23:30 URL:https://www.google.com/sitemap.xml [2049] -> "sitemap.xml" [1]

<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.google.com/schemas/sitemap/0.84">
  <sitemap>
    <loc>https://www.google.com/gmail/sitemap.xml</loc>
  </sitemap>
  <sitemap>
    <loc>https://www.google.com/forms/sitemaps.xml</loc>
  </sitemap>
...
```

با کاوش از آنجا، آزمایش کننده ممکن است بخواهد نقشه سایت gmail را بازیابی کند `https://www.google.com/gmail/sitemap.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://www.google.com/intl/am/gmail/about/</loc>
    <xhtml:link href="https://www.google.com/gmail/about/" hreflang="x-default" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/el/gmail/about/" hreflang="el" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/it/gmail/about/" hreflang="it" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/ar/gmail/about/" hreflang="ar" rel="alternate"/>
...
```

### <div dir="rtl">TXT امنیتی (Security TXT)</div>

ا `security.txt` یک استاندارد پیشنهادی ([proposed standard](https://securitytxt.org/)) است که به وب سایت ها اجازه می دهد تا سیاست های امنیتی و جزئیات تماس را تعریف کنند. دلایل متعددی وجود دارد که ممکن است در سناریوهای آزمایشی جالب باشد، از جمله اما نه محدود به:

- شناسایی مسیرها یا منابع بیشتر برای گنجاندن در کشف/تحلیل.
- جمع آوری اطلاعات منبع باز.
- یافتن اطلاعات در مورد Bug Bounties و غیره.
- مهندسی اجتماعی.

فایل ممکن است در ریشه وب سرور یا دایرکتوری `/well-known.` موجود باشد. نمونه:

- `https://example.com/security.txt`
- `https://example.com/.well-known/security.txt`

در اینجا یک نمونه دنیای واقعی است که از LinkedIn در 5 مه 2020 بازیابی شده است:

```bash
$ wget --no-verbose https://www.linkedin.com/.well-known/security.txt && cat security.txt
2020-05-07 12:56:51 URL:https://www.linkedin.com/.well-known/security.txt [333/333] -> "security.txt" [1]
# Conforms to IETF `draft-foudil-securitytxt-07`
Contact: mailto:security@linkedin.com
Contact: https://www.linkedin.com/help/linkedin/answer/62924
Encryption: https://www.linkedin.com/help/linkedin/answer/79676
Canonical: https://www.linkedin.com/.well-known/security.txt
Policy: https://www.linkedin.com/help/linkedin/answer/62924
```

### <div dir="rtl">TXT انسان ها (Humans TXT)</div>

ا `humans.txt` ابتکاری برای شناخت افراد پشت وب سایت است. این به شکل یک فایل متنی است که حاوی اطلاعاتی در مورد افراد مختلفی است که در ساخت وب سایت مشارکت داشته اند. برای اطلاعات بیشتر به [humanstxt](http://humanstxt.org/) مراجعه کنید. این فایل اغلب (اگرچه نه همیشه) حاوی اطلاعاتی برای مشاغل یا سایت های شغلی/مسیرها است.

مثال زیر در 5 مه 2020 از Google بازیابی شده است:

```bash
$ wget --no-verbose  https://www.google.com/humans.txt && cat humans.txt
2020-05-07 12:57:52 URL:https://www.google.com/humans.txt [286/286] -> "humans.txt" [1]
Google is built by a large team of engineers, designers, researchers, robots, and others in many different sites across the globe. It is updated continuously, and built with more tools and technologies than we can shake a stick at. If you'd like to help us out, see careers.google.com.
```

### سایر منابع اطلاعاتی well-known.

ا RFC ها و پیش نویس های اینترنتی دیگری نیز وجود دارند که استفاده استاندارد شده از فایل ها را در `/well-known.` دایرکتوری پیشنهاد می کنند. لیستی از آنها را می توانید در [اینجا](https://en.wikipedia.org/wiki/List_of_/.well-known/_services_offered_by_webservers) یا [اینجا](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml) پیدا کنید.

بررسی RFC یا پیش نویس ها و ایجاد فهرستی برای ارائه به یک خزنده یا fuzzer برای یک آزمایش کننده نسبتاً ساده است تا وجود یا محتوای چنین فایل هایی را تأیید کند.

## ابزارها

- Browser (مرورگر) &#x202b;(مشاهده عملکرد منبع یا Dev Tools)
- curl
- wget
- Burp Suite
- ZAP

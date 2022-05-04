# Review Webserver Metafiles for Information Leakage

بررسی متافایل های وب سرور برای نشت اطلاعات (فارسی)

|شناسه          |
|------------|
|WSTG-INFO-03|

## خلاصه

این بخش نحوه آزمایش فایل های متادیتا مختلف را برای نشت اطلاعات مسیر(های) یا عملکرد برنامه وب توضیح می دهد. علاوه بر این، فهرست دایرکتوری هایی که باید توسط عنکبوت ها (Spiders)، ربات ها (Robots) یا خزنده ها (Crawlers) اجتناب شود، می تواند به عنوان یک وابستگی برای مسیرهای اجرای نقشه از طریق برنامه ([Map execution paths through application](07-Map_Execution_Paths_Through_Application.md)) ایجاد شود. اطلاعات دیگری نیز ممکن است برای شناسایی سطح حمله، جزئیات فناوری، یا برای استفاده در تعامل مهندسی اجتماعی جمع آوری شود.

## اهداف آزمون

• شناسایی مسیرها و عملکردهای مخفی یا مبهم از طریق تجزیه و تحلیل فایل های ابرداده (متادیتا).

• استخراج و نقشه برداری اطلاعات دیگری که می تواند منجر به درک بهتر سیستم های موجود شود.

## چگونه آزمایش کنیم

> هر یک از اقدامات انجام شده در زیر با `wget` نیز می تواند با `curl` انجام شود. بسیاری از ابزارهای Dynamic Application Security Testing (DAST) (تست امنیت برنامه های پویا) مانند ZAP و Burp Suite شامل بررسی یا تجزیه این منابع به عنوان بخشی از عملکرد عنکبوت/خزنده خود باشند. آنها همچنین می توانند با استفاده از [Google Dorks](https://en.wikipedia.org/wiki/Google_hacking) مختلف یا استفاده از ویژگی های جستجوی پیشرفته مانند `:inurl` باشند.

### ربات ها

عنکبوت های وب، ربات ها یا خزنده ها یک صفحه وب را بازیابی می کنند و سپس به صورت بازگشتی از لینک ها عبور می کنند تا محتوای وب بیشتری را بازیابی کنند. رفتار پذیرفته شده آنها توسط پروتکل حذف Robots ([Robots Exclusion Protocol](https://www.robotstxt.org)) فایل [robots.txt](https://www.robotstxt.org/) در فهرست اصلی وب مشخص شده است.

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

دستورالعمل عامل کاربر ([User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)) به عنکبوت/ربات/خزنده وب خاص اشاره دارد. به عنوان مثال، `User-Agent: Googlebot` به عنکبوت از گوگل اشاره دارد در حالی که `User-Agent: bingbot` به خزنده ای از مایکروسافت اشاره دارد. `User-Agent: *` در مثال بالا برای همه [عنکبوت ها / ربات ها / خزنده های وب](https://support.google.com/webmasters/answer/6062608?visit_id=637173940975499736-3548411022&rd=1) اعمال می شود.

این `Disallow` دستورالعمل مشخص می کند که کدام منابع توسط عنکبوت ها / روبات ها / خزنده ها ممنوع است. در مثال بالا موارد زیر ممنوع است:

```text
...
Disallow: /search
...
Disallow: /sdch
...
```

عنکبوت ها/ربات ها/خزنده های وب می توانند عمدا `Disallow` دستورالعمل های مشخص شده در یک فایل `robots.txt`، مانند دستورالعمل های شبکه های اجتماعی را نادیده بگیرند ([intentionally ignore](https://blog.isc2.org/isc2_blog/2008/07/the-attack-of-t.html)) تا اطمینان حاصل کنند که پیوندهای مشترک هنوز معتبر هستند. از این رو، `robots.txt` نباید به عنوان مکانیزمی برای اعمال محدودیت در نحوه دسترسی، ذخیره یا انتشار مجدد محتوای وب توسط اشخاص ثالث در نظر گرفته شود.

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

### فایل robots.txt را با استفاده از Google Webmaster Tools تجزیه و تحلیل کنید

صاحبان وب سایت می توانند از عملکرد «Analyze robots.txt» Google برای تجزیه و تحلیل وب سایت به عنوان بخشی از Google Webmaster Tools استفاده کنند. این ابزار می تواند به تست کمک کند و روش به شرح زیر است:

با یک حساب Google وارد Google Webmaster Tools شوید.

در داشبورد، آدرس سایت مورد تجزیه و تحلیل را وارد کنید.

بین روش های موجود انتخاب کنید و دستورالعمل روی صفحه را دنبال کنید.

### برچسب های متا

<META>تگ ها در بخش هر سند HTML قرار دارند و در صورتی که نقطه شروع ربات/عنکبوت/خزنده از پیوند سندی غیر از webroot یعنی پیوند عمیقHEAD شروع نشود، باید در یک وب سایت سازگار باشند . دستورالعمل روبات ها را می توان با استفاده از یک تگ متا خاص نیز مشخص کرد.

#### ربات متا تگ
اگر <META NAME="ROBOTS" ... >ورودی وجود نداشته باشد، "پروتکل حذف روبات ها" به ترتیب به صورت پیش فرض در نظر INDEX,FOLLOWگرفته می شود. بنابراین، دو ورودی معتبر دیگر که توسط «پروتکل حذف روبات ها» تعریف شده اند با NO...ie NOINDEXو NOFOLLOW.

بر اساس دستور(های) Disallow فهرست شده در robots.txtفایل در webroot، جستجوی عبارات منظم برای <META NAME="ROBOTS"هر صفحه وب انجام می شود و نتیجه با robots.txtفایل در webroot مقایسه می شود.

#### تگ های متفرقه اطلاعات متا
                                                                                                  
سازمان ها اغلب تگ های متا اطلاعاتی را در محتوای وب تعبیه می کنند تا از فناوری های مختلف مانند صفحه خوان ها، پیش نمایش شبکه های اجتماعی، نمایه سازی موتورهای جستجو، و غیره پشتیبانی کنند. و تست کنید. متا اطلاعات زیر از www.whitehouse.govطریق View Page Source در 5 مه 2020 بازیابی شده است:

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

### نقشه های سایت
  
نقشه سایت فایلی است که در آن یک توسعه دهنده یا سازمان می تواند اطلاعاتی در مورد صفحات، ویدیوها و سایر فایل های ارائه شده توسط سایت یا برنامه کاربردی و ارتباط بین آنها ارائه دهد. موتورهای جستجو می توانند از این فایل برای بررسی هوشمندانه سایت شما استفاده کنند. آزمایش کنندگان می توانند از sitemap.xmlفایل ها برای کسب اطلاعات بیشتر درباره سایت یا برنامه استفاده کنند تا آن را به طور کامل کاوش کنند.

گزیده زیر از نقشه سایت اصلی گوگل است که در 5 می 2020 بازیابی شده است.

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

با کاوش از آنجا، آزمایش کننده ممکن است بخواهد نقشه سایت gmail را بازیابی کند https://www.google.com/gmail/sitemap.xml:

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://www.google.com/intl/am/gmail/about/</loc>
    <xhtml:link href="https://www.google.com/gmail/about/" hreflang="x-default" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/el/gmail/about/" hreflang="el" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/it/gmail/about/" hreflang="it" rel="alternate"/>
    <xhtml:link href="https://www.google.com/intl/ar/gmail/about/" hreflang="ar" rel="alternate"/>
...

### TXT امنیتی

security.txtیک استاندارد پیشنهادی است که به وب سایت ها اجازه می دهد تا سیاست های امنیتی و جزئیات تماس را تعریف کنند. دلایل متعددی وجود دارد که ممکن است در سناریوهای آزمایشی جالب باشد، از جمله اما نه محدود به:

شناسایی مسیرها یا منابع بیشتر برای گنجاندن در کشف/تحلیل.
    
جمع آوری اطلاعات منبع باز
    
یافتن اطلاعات در مورد Bug Bounties و غیره
    
مهندسی اجتماعی.
    
فایل ممکن است در ریشه وب سرور یا .well-known/دایرکتوری موجود باشد. سابق:

https://example.com/security.txt
https://example.com/.well-known/security.txt

در اینجا یک نمونه دنیای واقعی است که از LinkedIn 2020 در 5 مه بازیابی شده است:

$ wget --no-verbose https://www.linkedin.com/.well-known/security.txt && cat security.txt
2020-05-07 12:56:51 URL:https://www.linkedin.com/.well-known/security.txt [333/333] -> "security.txt" [1]
# Conforms to IETF `draft-foudil-securitytxt-07`
Contact: mailto:security@linkedin.com
Contact: https://www.linkedin.com/help/linkedin/answer/62924
Encryption: https://www.linkedin.com/help/linkedin/answer/79676
Canonical: https://www.linkedin.com/.well-known/security.txt
Policy: https://www.linkedin.com/help/linkedin/answer/62924
### TXT انسان

humans.txtابتکاری برای شناخت افراد پشت وب سایت است. این به شکل یک فایل متنی است که حاوی اطلاعاتی در مورد افراد مختلفی است که در ساخت وب سایت مشارکت داشته اند. برای اطلاعات بیشتر به humanstxt مراجعه کنید . این فایل اغلب (اگرچه نه همیشه) حاوی اطلاعاتی برای سایت ها/مسیرهای شغلی یا شغلی است.

مثال زیر در 5 مه 2020 از Google بازیابی شده است:

$ wget --no-verbose  https://www.google.com/humans.txt && cat humans.txt
2020-05-07 12:57:52 URL:https://www.google.com/humans.txt [286/286] -> "humans.txt" [1]
Google is built by a large team of engineers, designers, researchers, robots, and others in many different sites across the globe. It is updated continuously, and built with more tools and technologies than we can shake a stick at. If you'd like to help us out, see careers.google.com.

### سایر منابع اطلاعاتی معروف

RFC ها و پیش نویس های اینترنتی دیگری نیز وجود دارند که استفاده استاندارد شده از فایل ها را در .well-known/دایرکتوری پیشنهاد می کنند. لیستی از آنها را می توانید در اینجا یا اینجا پیدا کنید .

بررسی RFC برای آزمایش کننده بسیار ساده است/پیش نویس ها ایجاد فهرستی برای ارائه به خزنده یا fuzzer به منظور تأیید وجود یا محتوای چنین فایل هایی است.

## ابزار

مرورگر (مشاهده عملکرد منبع یا Dev Tools)

حلقه

wget

سوئیت آروغ

ZAP
